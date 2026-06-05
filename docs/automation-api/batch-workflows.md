---
tags:
  - automation-api
  - batch
---

# Batch Workflows

**Summary:** Running parameter sweeps and automated scenario batches with the WEST API.

**Prerequisites:** [Running Simulations Programmatically](running-simulations.md)

---

## Parameter sweep example

The pattern for any parameter sweep is:

1. Open the project once.
2. Loop over parameter values.
3. Set the parameter, run the experiment, read the result.
4. Append results to a list.
5. Write to CSV after the loop.
6. Close the project.

### HRT sensitivity sweep (6 – 24 hours, 2-hour steps)

The example below varies the hydraulic retention time (HRT) of a bioreactor by adjusting the reactor volume at a fixed influent flow, then records key effluent quality indicators.

```python
import win32com.client
import csv
import logging

logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s  %(levelname)s  %(message)s",
    handlers=[logging.FileHandler("hrt_sweep.log"), logging.StreamHandler()],
)

PROJECT_PATH = r"C:\Projects\MyProject.wst"
RESULTS_CSV  = r"C:\Projects\hrt_sweep_results.csv"

Q_INFLUENT   = 10_000.0   # m³/d — fixed influent flow
HRT_HOURS    = range(6, 26, 2)   # 6, 8, 10, … 24 hours

west = win32com.client.Dispatch("DHI.WEST.Application")

try:
    west.OpenProject(PROJECT_PATH)
    project  = west.Project
    layout   = project.Layout
    reactor  = layout.GetUnit("Reactor1")
    ss_exp   = project.Experiments.Item(0)

    rows = []

    for hrt_h in HRT_HOURS:
        hrt_d = hrt_h / 24.0
        volume = Q_INFLUENT * hrt_d   # V = Q * HRT

        logging.info(f"Running HRT={hrt_h} h  (V={volume:.0f} m³)")
        reactor.SetParameter("V", volume)

        try:
            ss_exp.Run()

            if not ss_exp.RunSucceeded:
                logging.warning(f"  HRT={hrt_h} h: run did not converge — skipping.")
                continue

            results = ss_exp.Results
            row = {
                "HRT_h":    hrt_h,
                "V_m3":     volume,
                "SNH_gm3":  results.GetValue("Effluent.SNH"),
                "SCOD_gm3": results.GetValue("Effluent.SCOD"),
                "TN_gm3":   results.GetValue("Effluent.TN"),
                "TP_gm3":   results.GetValue("Effluent.TP"),
            }
            rows.append(row)
            logging.info(f"  SNH={row['SNH_gm3']:.3f}  SCOD={row['SCOD_gm3']:.1f}")

        except Exception as e:
            logging.error(f"  HRT={hrt_h} h failed: {e}")

    # Write all collected rows to CSV
    if rows:
        with open(RESULTS_CSV, "w", newline="") as fh:
            writer = csv.DictWriter(fh, fieldnames=rows[0].keys())
            writer.writeheader()
            writer.writerows(rows)
        logging.info(f"Results written to {RESULTS_CSV}")
    else:
        logging.warning("No successful runs — CSV not written.")

finally:
    west.CloseProject()
    logging.info("Project closed.")
```

---

## Parallel batch runs

WEST's COM server runs in a **single-process, single-threaded** model. Only one experiment can execute at a time within a single WEST instance. Do **not** call `Run()` from multiple threads against the same COM object — this will cause undefined behaviour or a crash.

To parallelise across machines or CPU cores, launch **separate WEST processes** (one per worker) and distribute project copies to each. A practical approach using Python's `multiprocessing`:

```python
from multiprocessing import Pool
import subprocess, pathlib, shutil, csv

BASE_PROJECT = r"C:\Projects\MyProject.wst"
WORK_DIR     = r"C:\Projects\BatchRuns"

def run_single(hrt_h):
    """Worker function: copies the project, sets HRT, runs, returns result row."""
    import win32com.client, os

    run_dir = pathlib.Path(WORK_DIR) / f"HRT_{hrt_h:02d}h"
    run_dir.mkdir(parents=True, exist_ok=True)
    proj = str(run_dir / "run.wst")
    shutil.copy(BASE_PROJECT, proj)

    west = win32com.client.Dispatch("DHI.WEST.Application")
    try:
        west.OpenProject(proj)
        p = west.Project
        p.Layout.GetUnit("Reactor1").SetParameter("V", 10_000 * hrt_h / 24)
        p.Experiments.Item(0).Run()
        snh = p.Experiments.Item(0).Results.GetValue("Effluent.SNH")
        return {"HRT_h": hrt_h, "SNH_gm3": snh}
    finally:
        west.CloseProject()

if __name__ == "__main__":
    hrt_values = list(range(6, 26, 2))
    with Pool(processes=4) as pool:
        results = pool.map(run_single, hrt_values)
    print(results)
```

> **Note:** Each worker requires its own WEST licence seat. Ensure your licence server has enough concurrent seats before launching parallel workers.

---

## Storing and analysing batch results

After a batch run, collect results using `sim.get_variable(block_path, variable_name)` for each run. Store them in a pandas DataFrame for analysis:

```python
import pandas as pd
records = []
for params, result in batch_results:
    records.append({**params, **result})
df = pd.DataFrame(records)
df.to_csv('batch_results.csv', index=False)
```

Use standard pandas or matplotlib operations to compute statistics, plot response surfaces, or rank parameter combinations by effluent quality. For large batches (>100 runs), consider writing results incrementally to avoid memory issues.

### Saving to CSV

Use Python's built-in `csv.DictWriter` (as shown above) for straightforward tabular results. Each row should include the swept parameter value(s) alongside all output metrics of interest.

### Loading and plotting with pandas

```python
import pandas as pd
import matplotlib.pyplot as plt

df = pd.read_csv(r"C:\Projects\hrt_sweep_results.csv")

fig, axes = plt.subplots(2, 2, figsize=(10, 7))
for ax, col in zip(axes.flat, ["SNH_gm3", "SCOD_gm3", "TN_gm3", "TP_gm3"]):
    ax.plot(df["HRT_h"], df[col], marker="o")
    ax.set_xlabel("HRT (h)")
    ax.set_ylabel(col)
    ax.set_title(col)
plt.tight_layout()
plt.savefig(r"C:\Projects\hrt_sweep.png", dpi=150)
```

---

## Best practices

| Practice | Reason |
|---|---|
| Always call `west.CloseProject()` in a `finally` block. | Prevents file locks and memory leaks if the script errors mid-run. |
| Log each run's parameters and outcome to a file. | Makes it easy to diagnose which runs failed and why. |
| Skip (do not abort) on individual run failures. | A single non-convergence should not stop the entire sweep. |
| Work on a **copy** of the project for each worker. | Avoids race conditions when writing results back into the `.wst` file. |
| Validate results before appending (`RunSucceeded`, range checks). | Guards against silently writing garbage values caused by poor convergence. |
| Use `range()` or `numpy.linspace()` for parameter grids. | Guarantees reproducible, evenly-spaced sweeps without floating-point drift. |
