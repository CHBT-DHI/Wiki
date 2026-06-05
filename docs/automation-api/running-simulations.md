---
tags:
  - automation-api
  - simulation
---

# Running Simulations Programmatically

**Summary:** How to configure and run a WEST simulation from a script.

**Prerequisites:** [Getting Started with the API](getting-started.md)

---

## Opening a project and selecting an experiment

Before running a simulation you must open a project and obtain a reference to the experiment you want to execute. Experiments are typed — the two most common types are `SteadyState` and `Dynamic`.

```python
import win32com.client

west = win32com.client.Dispatch("DHI.WEST.Application")
west.OpenProject(r"C:\Projects\MyProject.wst")

project = west.Project
experiments = project.Experiments

# Find an experiment by name
def get_experiment(name):
    for i in range(experiments.Count):
        exp = experiments.Item(i)
        if exp.Name == name:
            return exp
    raise KeyError(f"Experiment '{name}' not found")

ss_exp = get_experiment("SteadyState_Base")
```

---

## Setting parameter values before a run

Model parameters are accessible through the `Layout` object. Each process unit exposes its parameters by name. Always set parameters **before** calling `Run()`.

```python
layout = project.Layout

# Access a unit operation by name and set a parameter
reactor = layout.GetUnit("Reactor1")
reactor.SetParameter("V", 1500.0)        # volume in m³
reactor.SetParameter("T", 15.0)          # temperature in °C

# Alternatively, set a global model parameter
project.SetParameter("Influent.Q", 25000.0)  # flow in m³/d
```

---

## Steady-state run

Call `Run()` on the experiment object to start execution. For a steady-state experiment, WEST iterates until convergence. The call is **synchronous by default** on most WEST versions — it blocks until the solver finishes or fails.

```python
import win32com.client
import time

west = win32com.client.Dispatch("DHI.WEST.Application")
west.OpenProject(r"C:\Projects\MyProject.wst")

project    = west.Project
ss_exp     = project.Experiments.Item(0)   # first experiment

# Set a parameter, then run
project.Layout.GetUnit("Reactor1").SetParameter("V", 1500.0)

ss_exp.Run()   # blocks until complete (or raises on failure)

# Read a result
results = ss_exp.Results
snh_eff = results.GetValue("Reactor1.SNH")
print(f"Effluent NH4-N: {snh_eff:.3f} g/m³")

west.CloseProject()
```

---

## Dynamic run

Dynamic experiments require a simulation period and time step. These are set on the experiment's `SimulationSettings` object (or equivalent, depending on WEST version).

```python
dyn_exp = get_experiment("Dynamic_StormEvent")

settings = dyn_exp.SimulationSettings
settings.StartTime  = 0.0     # days
settings.EndTime    = 14.0    # days
settings.TimeStep   = 0.01    # days (~15 min)

dyn_exp.Run()
```

For long dynamic runs you can poll run status rather than blocking:

```python
import time

dyn_exp.RunAsync()   # non-blocking start

while dyn_exp.IsRunning:
    pct = dyn_exp.ProgressPercent
    print(f"\rProgress: {pct:.1f}%", end="", flush=True)
    time.sleep(5)

print("\nRun complete." if dyn_exp.RunSucceeded else "\nRun FAILED.")
```

---

## Reading results

After a successful run, results are accessible through `experiment.Results`. You can retrieve scalar output values or time-series arrays.

```python
results = ss_exp.Results

# Scalar output (steady-state or end-of-dynamic value)
cod_eff   = results.GetValue("Secondary.SCOD")
tn_eff    = results.GetValue("Secondary.TN")
print(f"COD={cod_eff:.1f}  TN={tn_eff:.1f}  g/m³")

# Time-series output from a dynamic run
times  = dyn_exp.Results.GetTimeSeries("Reactor1.SNH").Times
values = dyn_exp.Results.GetTimeSeries("Reactor1.SNH").Values
```

---

## Error handling

Wrap simulation calls in `try/except` blocks to catch COM errors, convergence failures, and licence issues.

```python
import pythoncom

try:
    ss_exp.Run()
    snh = ss_exp.Results.GetValue("Effluent.SNH")
except pythoncom.com_error as e:
    # COM-level error (e.g. WEST crashed, licence server unreachable)
    print(f"COM error: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
finally:
    # Always check convergence flag before trusting results
    if not ss_exp.RunSucceeded:
        print("WARNING: run did not converge — results may be invalid.")
    west.CloseProject()
```

Common failure modes:

| Symptom | Likely cause |
|---|---|
| `com_error` on `Dispatch` | COM server not registered; re-run WEST installer. |
| `Run()` returns but `RunSucceeded` is `False` | Solver did not converge; check initial conditions or reduce time step. |
| Licence error message | Licence server unreachable or all seats in use. |
| `GetValue` returns `None` | Variable name misspelled or output not enabled for that experiment. |
