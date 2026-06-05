---
tags:
  - how-to
  - settings
---

# WEST Options and Preferences

**Summary**: Configure application-wide settings including solver defaults, display options, and file paths.

**Source**: WEST User Guide §2.5.1.

---

### Opening WEST Options

Go to **Application Menu** (the WEST icon, top-left) → **WEST Options**, or press **Alt+F, O**.

The Options dialog has several tabs:

---

### General tab

| Setting | Description |
|---|---|
| **Language** | Interface language (English, others if installed) |
| **Undo levels** | Number of undo steps (default: 20) |
| **Auto-save interval** | Minutes between auto-saves (default: 10 min) |
| **Auto-save location** | Folder for auto-save files (prefixed `~`) |
| **Recently used projects** | Number of recent projects shown in the Application Menu |

---

### Simulation tab

| Setting | Description |
|---|---|
| **Default integrator** | Default solver for new dynamic experiments (LSODA, RK45, etc.) |
| **Default step size** | Initial step size for dynamic runs |
| **Warm-start on run** | Automatically transfer SS results to dynamic initial conditions |
| **Output interval** | Default output frequency for new experiments |
| **Parallel runs** | Number of parallel CPU threads for scenario/uncertainty analysis |

---

### Display tab

| Setting | Description |
|---|---|
| **Grid size** | Layout canvas grid spacing (pixels) |
| **Snap to grid** | Snap blocks to grid when placing (default: on) |
| **Show block names** | Show/hide block names on the canvas |
| **Font size** | Default font size for block labels |
| **Connection style** | Orthogonal (right-angle) or curved connections |
| **Animation speed** | Speed of flow animation during live simulation |

---

### Paths tab

| Setting | Description |
|---|---|
| **Default project folder** | Where File → New Project saves by default |
| **Block library paths** | Additional folders WEST searches for `.wbl` block library files |
| **Model library paths** | Additional folders for custom Modelica model packages |
| **Temporary files** | Location for compiled model cache and temp output |

---

### Advanced tab

| Setting | Description |
|---|---|
| **Compiler path** | Path to `cl.exe` (Visual Studio C++ compiler) — auto-detected if Build Tools are installed |
| **Debug logging** | Enable verbose logging to the Logging Window for troubleshooting |
| **Memory limit** | Maximum RAM (GB) WEST may use for results storage |

---

### Related

- [Installation & Licensing](installation.md)
- [Running Simulations](running-simulations.md)
