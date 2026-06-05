---
tags:
  - developer-topics
---

# Custom Models

**Summary:** How to write, register, and test a custom process model in WEST.

---

## What is WML?

**WML (WEST Modelling Language)** is a Modelica-based domain-specific language (DSL) designed for describing wastewater treatment process models. It sits on top of standard Modelica and adds wastewater-specific conventions: component declarations map to soluble and particulate state variables, process rate expressions follow the Petersen matrix notation, and stoichiometric coefficients are written as signed real expressions.

WML files use the extension `.wml` and are compiled by the WEST model compiler into a binary that WEST can load at runtime. Because WML is Modelica-based, standard Modelica tooling (syntax highlighting, static analysis) works with minor adaptation.

---

## Structure of a custom model

Every WML model has three required sections:

```
model MyProcessModel
  // 1. Component declarations — state variables
  components
    ...
  end components;

  // 2. Process rate expressions — kinetic equations
  processes
    ...
  end processes;

  // 3. Stoichiometric matrix — how each process affects each component
  stoichiometry
    ...
  end stoichiometry;
end MyProcessModel;
```

### Component declarations

Each line declares one state variable, its unit, and its type (`soluble` or `particulate`):

```
components
  soluble    SS    [g COD / m³]   "Soluble substrate";
  soluble    SO    [g O2 / m³]    "Dissolved oxygen";
  particulate XB   [g COD / m³]   "Active heterotrophic biomass";
end components;
```

### Process rate expressions

Each process is a named kinetic expression returning units of `[g / (m³·d)]`:

```
processes
  rDecay = b_H * XB;    // first-order decay of heterotrophs
end processes;
```

### Stoichiometric coefficients

Each row ties a process to its effect on each component. Positive = production, negative = consumption:

```
stoichiometry
  //            SS      SO      XB
  rDecay        0       0      -1.0;
end stoichiometry;
```

---

## Example: custom first-order decay process

The complete model below adds a first-order decay process for a custom inert particulate (`XI_custom`), producing inert soluble COD (`SI_custom`).

```modelica
model FirstOrderDecay
  // Parameters
  parameter Real k_dec = 0.05   "[1/d]"  "First-order decay rate constant";
  parameter Real f_SI  = 0.08   "[-]"    "Fraction of decay producing SI";

  components
    particulate  X_custom   [g COD / m³]  "Custom active particulate";
    soluble      SI_custom  [g COD / m³]  "Inert soluble COD from decay";
  end components;

  processes
    r_decay = k_dec * X_custom;
  end processes;

  stoichiometry
    //             X_custom   SI_custom
    r_decay        -1.0       f_SI;
  end stoichiometry;

end FirstOrderDecay;
```

Save this file as `FirstOrderDecay.wml`.

---

## Compiling and loading the custom model

### 1. Compile

Open a command prompt and run the WEST model compiler (adjust the path to your installation):

```cmd
"C:\Program Files\DHI\WEST\bin\WESTCompiler.exe" ^
    --input  "C:\MyModels\FirstOrderDecay.wml"  ^
    --output "C:\MyModels\compiled\"
```

A successful compile produces `FirstOrderDecay.dll` (or `.so` on Linux) in the output directory. Compiler errors are printed to `stdout`; fix any type or unit mismatches before proceeding.

### 2. Register with WEST

Copy the compiled binary to the WEST user model directory and register it:

```cmd
copy "C:\MyModels\compiled\FirstOrderDecay.dll" ^
     "%APPDATA%\DHI\WEST\UserModels\"

"C:\Program Files\DHI\WEST\bin\WESTModelManager.exe" --register FirstOrderDecay
```

Alternatively, in the WEST GUI: **Tools → Model Manager → Add Model → Browse** to the `.dll`.

### 3. Verify registration

Restart WEST. Open **Tools → Model Manager** — `FirstOrderDecay` should appear in the list with status **Registered**.

---

## Testing the custom model in a simple layout

1. Create a new WEST project (**File → New Project**).
2. In the **Layout Editor**, drag a **Generic Reactor** block onto the canvas.
3. Open the reactor's **Model** property and select `FirstOrderDecay` from the drop-down (it will appear under *User Models*).
4. Add an **Influent** source and an **Effluent** sink; connect them to the reactor.
5. Create a **Steady-State** experiment and set initial values for `X_custom` and `SI_custom`.
6. Run the experiment. Inspect **Results → Reactor1.X_custom** to confirm exponential decay over time.

For automated regression testing, use the automation API to run the model and assert the output value:

```python
import win32com.client, math

west = win32com.client.Dispatch("DHI.WEST.Application")
west.OpenProject(r"C:\Tests\FirstOrderDecay_test.wst")

exp = west.Project.Experiments.Item(0)
exp.Run()
assert exp.RunSucceeded, "Test run did not converge"

X_out = exp.Results.GetValue("Reactor1.X_custom")
# After 1 day at k_dec=0.05, X should be ~X0 * exp(-0.05)
assert X_out < 1.0, f"Unexpected X_custom={X_out}"
print("Custom model test passed.")
west.CloseProject()
```
