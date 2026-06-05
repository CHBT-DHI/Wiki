---
tags:
  - experiment-types
  - scenario-analysis
---

# Scenario Analysis

**Summary:** Run and compare multiple predefined parameter scenarios in a single experiment.

**Source:** WEST Getting Started Tutorial, Chapter 12.

**Prerequisites:** [Running Simulations](../how-to/running-simulations.md) · WEST+ licence

---

## Concept

Scenario Analysis lets you define a table of parameter sets (scenarios) and run them all automatically. Each row in the table is one scenario; WEST runs the base simulation for each and compares results side by side.

**Typical uses:**
- Compare design alternatives (e.g. different tank volumes)
- Evaluate operating strategies (e.g. different DO set-points)
- Stress-test against different influent conditions

---

## Setting up a scenario analysis

1. Go to **Project | Virtual Experiments → Scenario Analysis** and select the base Dynamic experiment.
2. Open **Analysis Properties → Parameters** tab. Drag the parameters that will vary across scenarios.
3. In the **Runs** tab, define each scenario by entering parameter values row by row. Or use **Generate** to create a grid.
4. Define objectives in the **Variables** tab.
5. Click **Execute**. WEST runs one simulation per row.
6. Review results in the **Runs** tab — the best run (lowest overall objective) is highlighted. Use **Copy Values** to apply the best scenario's parameters.

> **Needs content.** Add example scenario table and results from Tutorial ch 12.

---

## Related

- [Parameter Estimation](parameter-estimation.md)
- [Uncertainty Analysis](uncertainty-analysis.md)
