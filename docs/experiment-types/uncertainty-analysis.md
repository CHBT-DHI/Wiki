---
tags:
  - experiment-types
  - uncertainty-analysis
---

# Uncertainty Analysis

**Summary:** Run multiple simulations with parameter values sampled from statistical distributions to quantify how uncertain the model outputs are.

**Prerequisites:** A working dynamic simulation. See [Running Simulations](../manuals/running-simulations.md).

---

## Overview

Uncertainty Analysis (UA) propagates parameter uncertainty through the model. WEST samples parameter values from user-defined statistical distributions and runs a simulation for each sample. The resulting spread of output values shows how sensitive the predictions are to the combined uncertainty in all varied parameters.

UA differs from Sensitivity Analysis in focus: SA asks *which parameters matter most*, UA asks *how uncertain are the outputs*.

---

## Setting up an uncertainty analysis

1. Go to **Project → Virtual Experiments** and create or open an **Uncertainty Analysis** experiment. Select the base dynamic simulation to use.
2. In the **Parameters** tab, add the items whose values should be varied. The following item types are supported:
   - Manipulated variables
   - Parameters
   - Derived variables (initial conditions)
   - Input variables
3. For each item, assign a **probability distribution**:

   | Distribution | Notes |
   |---|---|
   | Uniform | Flat probability between a minimum and maximum. |
   | Normal | Gaussian; specify mean and standard deviation. |
   | LogNormal | For quantities that cannot go below zero (e.g. rate constants). |
   | Exponential | For waiting-time or decay-type quantities. |
   | Weibull | Flexible shape; specify scale and shape parameters. |
   | Triangular | Specify minimum, mode, and maximum. |

4. In the **Solver** tab, set the **Number of Shots** (number of Monte Carlo runs). A minimum of 100 is recommended; 500–1000 gives stable percentile estimates.
5. Choose a **sampling method**:
   - **Latin Hypercube Sampling (LHS):** Stratifies the sample space so that each distribution is evenly covered. Recommended — gives better coverage with fewer runs than pure random sampling.
   - **Pure Random (PR):** Draws each sample independently at random. Simpler but requires more runs for equivalent coverage.
6. Optionally add **data files** in the Data Files tab to provide reference time series for objective calculation. For each file, specify the weight and the decimal/thousand separators used in the file.
7. Define output **criteria** (see below) to specify which aggregated statistics to compute.
8. Click **Run**. WEST executes one simulation per sample and computes all criteria.

---

## Criteria types

Criteria aggregate raw time-series output into scalar values for analysis. Four types are available:

| Criteria type | What it computes |
|---|---|
| **Time series criteria** | Aggregates a variable's values across time for a single run (e.g. mean, max, integral over the run duration). |
| **Run criteria** | Aggregates a statistic across all runs at each time point. Available statistics: min, max, mean, std dev, median, percentiles, skewness, kurtosis, moment, run number for min/max/closest-to-mean. |
| **BVDF (Bound Value Duration Frequency)** | Counts how many times and what percentage of simulation time a variable violates a lower or upper bound. The same cross-run statistics as Run criteria are available. |
| **Classification criteria** | Classifies output values into user-defined classes (e.g. compliant / non-compliant). The same cross-run statistics as Run criteria apply to each class. |

---

## Viewing and using results

The **Runs** tab displays a matrix with one row per completed simulation:

- Columns for each sampled parameter value used in that run.
- Columns for each computed objective value.

| Button | Action |
|---|---|
| **Generate** | Draw a new sample set (replaces the existing parameter table). |
| **Export** | Export the runs matrix to Excel (XLS/XLSX). |
| **Print** | Print the runs table. |
| **Copy Values** | Copy the selected run's parameter values back to the base simulation. |
| **Run** | Execute all runs in the current sample. |
| **Plot** | Generate instant histograms of output distributions across runs. |

---

## LHS vs random sampling — a note

Latin Hypercube Sampling divides each parameter's distribution into *N* equal-probability strata and draws exactly one sample from each stratum. This guarantees uniform coverage of the distribution even with a small number of runs and typically gives more stable percentile estimates than pure random sampling for the same N. Use Pure Random only when you have a specific statistical reason to require independent draws.

---

## Related

- [Scenario Analysis](scenario-analysis.md)
- [Parameter Estimation](parameter-estimation.md)
- [Results and Output](../manuals/results-output.md)
