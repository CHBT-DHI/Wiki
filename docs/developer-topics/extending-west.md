---
tags:
  - developer-topics
---

# Extending WEST

**Summary:** Integration points, scripting support, and how to connect WEST to external tools.

See also [Automation & API](../automation-api/index.md) for driving WEST programmatically.

---

## Custom blocks vs custom models

WEST distinguishes two separate extension concepts that are often confused:

| Concept | What it is | Where it lives |
|---|---|---|
| **Custom model** | A set of differential/algebraic equations describing process kinetics, written in WML. | Compiled `.dll`; loaded by the WEST solver engine. |
| **Custom block** | A graphical UI element in the Layout Editor — the visual representation of a unit operation. | Block library file (`.wbl`); loaded by the WEST GUI. |

A custom model defines the *maths*. A custom block defines how that model appears on the flowsheet canvas: its icon, connection ports, parameter input form, and default display name. A block references exactly one model; a model can be referenced by multiple blocks.

You can use a custom model without creating a custom block (WEST will render it as a generic unit), but a production-quality extension should provide both.

---

## Creating a custom block library

### 1. Define the block in a `.wbl` file

Block library files are XML-based. A minimal block entry looks like:

```xml
<BlockLibrary name="MyOrganisation">
  <Block name="FirstOrderDecay" model="FirstOrderDecay" icon="decay_icon.svg">
    <Description>Custom first-order decay reactor</Description>
    <Ports>
      <Port name="Inlet"  type="WasteWater" direction="in"  position="left"/>
      <Port name="Outlet" type="WasteWater" direction="out" position="right"/>
    </Ports>
    <Parameters>
      <Parameter name="k_dec" displayName="Decay rate constant" unit="1/d" default="0.05"/>
      <Parameter name="f_SI"  displayName="Inert SI fraction"   unit="-"   default="0.08"/>
    </Parameters>
  </Block>
</BlockLibrary>
```

Save as `MyOrganisation.wbl`.

### 2. Install the block library

Copy the `.wbl` file and any icon assets to the WEST user block library directory:

```cmd
copy MyOrganisation.wbl "%APPDATA%\DHI\WEST\BlockLibraries\"
copy decay_icon.svg     "%APPDATA%\DHI\WEST\BlockLibraries\Icons\"
```

### 3. Load in WEST

Restart WEST (or use **Tools → Block Library Manager → Reload**). The new library appears in the Layout Editor's block palette under its library name.

---

## Registering a custom model with the WEST model registry

The model registry maps a model name to its compiled binary. Registration is needed both for the solver (to load the equations) and for the block system (to bind a block to its model).

### Via the GUI

1. **Tools → Model Manager → Add Model**
2. Browse to the compiled `.dll`.
3. Click **Register**. WEST validates the binary and adds an entry to `%APPDATA%\DHI\WEST\ModelRegistry.xml`.

### Via the command line

```cmd
"C:\Program Files\DHI\WEST\bin\WESTModelManager.exe" ^
    --register "C:\MyModels\compiled\FirstOrderDecay.dll"
```

### Verify registration

```cmd
"C:\Program Files\DHI\WEST\bin\WESTModelManager.exe" --list
```

The output lists all registered models with their version, path, and status (`OK` / `Missing` / `Version mismatch`).

---

## Sharing custom models with other users

### Option A: Simple file copy

For individual sharing, distribute:

1. The compiled model binary (`FirstOrderDecay.dll`).
2. The block library file (`MyOrganisation.wbl`) and icon assets.
3. A short README with the registration command.

The recipient runs `WESTModelManager.exe --register` and copies the `.wbl` to their block library directory.

### Option B: WEST Extension Package (`.wep`)

WEST supports a zip-based package format for bundling models and blocks together:

```
MyExtension.wep  (ZIP renamed to .wep)
├── models/
│   └── FirstOrderDecay.dll
├── blocks/
│   ├── MyOrganisation.wbl
│   └── icons/
│       └── decay_icon.svg
└── manifest.xml
```

The `manifest.xml` declares the package name, version, author, and lists the files:

```xml
<WESTExtensionPackage version="1.0">
  <Name>FirstOrderDecay</Name>
  <Version>1.0.0</Version>
  <Author>Your Name</Author>
  <Models>
    <Model path="models/FirstOrderDecay.dll"/>
  </Models>
  <BlockLibraries>
    <BlockLibrary path="blocks/MyOrganisation.wbl"/>
  </BlockLibraries>
</WESTExtensionPackage>
```

Install a `.wep` package via **Tools → Extension Manager → Install Package**, or from the command line:

```cmd
"C:\Program Files\DHI\WEST\bin\WESTExtensionManager.exe" ^
    --install "C:\Downloads\FirstOrderDecay.wep"
```

The extension manager registers the model, copies the block library, and records the package in the installed-extensions list for later uninstallation.

### Option C: Version-controlled repository

For team environments, store WML source files in a Git repository and use a CI pipeline (e.g., GitHub Actions) to compile and produce versioned `.wep` artefacts automatically. Colleagues install the latest artefact from the repository's releases page.

---

## Related

- [Custom Models](custom-models.md) — writing and compiling WML model files.
- [Automation & API](../automation-api/index.md) — driving WEST and custom models programmatically.
