---
tags:
  - experiment-types
  - calibration
  - parameter-estimation
---

# Parameter Estimation & Calibration

**Summary:** Automatically fit model parameters to measured plant data using WEST's built-in optimisation.

**Source:** WEST Getting Started Tutorial, Chapter 11.

**Prerequisites:** [Running Simulations](../how-to/running-simulations.md) · [Sensitivity Analysis](sensitivity-analysis.md) · WEST+ licence

---

## Concept

Parameter Estimation (also called Model Calibration) minimises the difference between simulated outputs and measured data by iteratively adjusting selected parameters. WEST uses gradient-based and evolutionary optimisation algorithms.

Two sub-modes are available:
- **Model Calibration** (11.1): minimise deviation from measurement time-series
- **Optimisation** (11.2): minimise an arbitrary objective function (cost, energy, etc.)

---

## Setting up parameter estimation

1. Save a copy of the TwoASU project.
2. Go to **Project | Virtual Experiments → Parameter Estimation** and select the base Dynamic experiment.
3. Open **Analysis Properties**.
4. In the **Parameters** tab, drag the parameters to calibrate (e.g. `Clarifier.r_P`, `Clarifier.v0`, internal recycle flow). Set bounds for each.
5. In the **Variables** tab, drag measured data series (from a data file) and the corresponding model outputs. Configure the **Objective** criterion for each pair (e.g. AbsSquared).
6. In the **Solver** tab, select the optimisation algorithm and settings.
7. Click **Execute**. WEST runs multiple simulations, adjusting parameters toward the optimum.
8. In the **Runs** tab, select the best run and click **Copy Values** to apply the calibrated parameters to the simulation.

> **Needs content.** Expand from Tutorial ch 11 with algorithm options, stopping criteria, and results interpretation guidance.

---

## Related

- [Sensitivity Analysis](sensitivity-analysis.md) — use LSA/GSA first to identify which parameters matter
- [Uncertainty Analysis](uncertainty-analysis.md) — quantify remaining uncertainty after calibration
