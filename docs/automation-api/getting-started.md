---
tags:
  - automation-api
---

# Getting Started with the API

**Summary:** Prerequisites, installation, and a first working script that connects to WEST.

**Prerequisites:**

- WEST installed and licensed.
- Python 3.9 or later.

---

## What interface does WEST expose?

WEST exposes a **COM (Component Object Model) automation interface** — a Windows-native inter-process communication mechanism that allows external scripts and applications to control WEST programmatically. Any language or environment that can act as a COM client can drive WEST, including:

- **Python** (via `pywin32` / `win32com.client`)
- **MATLAB** (via `actxserver`)
- **VBA / VBScript** (via `CreateObject`)
- **R** (via the `RDCOMClient` package)

The COM server is registered automatically when WEST is installed. There is no separate REST API or Python package to install — everything goes through the COM dispatch layer.

---

## Installation

The WEST Python API is included with WEST SDK edition. Install the Python package from the WEST installation directory:

```bash
pip install "C:\Program Files\DHI\WEST\sdk\west_api-*.whl"
```

Requires Python 3.8 or later. The package depends on `numpy`, `pandas`, and `pywin32` (Windows). Verify the installation:

```python
import west_api
print(west_api.__version__)
```

If `import west_api` fails, ensure the WEST installation directory is on the system PATH and that the correct Python interpreter (64-bit) is being used.

### 1. Verify the COM server is registered

After a standard WEST installation the COM server (`DHI.WEST.Application`) is registered in the Windows registry. Open a command prompt and run:

```cmd
reg query HKCR\DHI.WEST.Application
```

If the key is missing, re-run the WEST installer and ensure the **Register COM Server** component is selected, or run from the WEST installation directory:

```cmd
WESTApplication.exe /regserver
```

### 2. Install the Python COM client library

The only Python dependency is `pywin32`:

```bash
pip install pywin32
```

No other packages are required to connect to the COM interface. For result processing you may also want `pandas` and `csv` from the standard library.

---

## First script — connect and list scenarios

The entry point for every WEST automation session is the `DHI.WEST.Application` COM class. Once connected you can navigate the object hierarchy (`Project` → `Layout` → `Experiment` → `Results`).

```python
import win32com.client

# Connect to (or launch) the WEST application
west = win32com.client.Dispatch("DHI.WEST.Application")

# Open an existing project
west.OpenProject(r"C:\Projects\MyProject.wst")

project = west.Project

# List all experiments defined in the project
for i in range(project.Experiments.Count):
    exp = project.Experiments.Item(i)
    print(f"Experiment {i}: {exp.Name}  type={exp.ExperimentType}")
```

### Top-level object hierarchy

| Object | Description |
|---|---|
| `Application` | Root object; controls the WEST process itself. |
| `Project` | Represents the open `.wst` project file. |
| `Layout` | The process flowsheet (unit operations and connections). |
| `Experiment` | A configured simulation run (steady-state or dynamic). |
| `Results` | Output variable values produced after a run completes. |

Access each level through its parent:

```python
west        = win32com.client.Dispatch("DHI.WEST.Application")
project     = west.Project
layout      = project.Layout
experiments = project.Experiments
results     = experiments.Item(0).Results   # results of first experiment
```

---

## Related

- [Running Simulations Programmatically](running-simulations.md)
- [Batch Workflows](batch-workflows.md)
- [Developer Topics](../developer-topics/index.md)
