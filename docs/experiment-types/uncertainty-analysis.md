---
tags:
  - experiment-types
  - uncertainty-analysis
---

# Uncertainty Analysis

**Summary:** Propagate parameter uncertainty through the model to quantify output uncertainty.

**Source:** WEST Getting Started Tutorial, Chapter 13.

**Prerequisites:** [Global Sensitivity Analysis](sensitivity-analysis.md#global-sensitivity-analysis) · WEST+ licence

---

## Concept

Uncertainty Analysis (UA) is similar to Global Sensitivity Analysis — it uses Monte Carlo sampling of parameter distributions and runs multiple simulations. The difference is the focus: GSA asks *which parameters drive uncertainty*, while UA asks *how uncertain are the outputs*.

Results are expressed as distributions over output variables (e.g. 5th–95th percentile range of effluent TSS across all Monte Carlo runs).

---

## Setting up an uncertainty analysis

1. Go to **Project | Virtual Experiments → Uncertainty Analysis** and select the base Dynamic experiment.
2. Assign probability distributions to parameters (same as GSA setup).
3. Set number of Monte Carlo runs (e.g. 100+).
4. Define output variables of interest.
5. Execute. WEST generates output distributions.

> **Needs content.** Add results interpretation guidance and example output from Tutorial ch 13.

---

## Related

- [Global Sensitivity Analysis](sensitivity-analysis.md#global-sensitivity-analysis)
- [Parameter Estimation](parameter-estimation.md)
