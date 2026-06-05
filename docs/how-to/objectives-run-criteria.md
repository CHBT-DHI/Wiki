---
tags:
  - how-to
  - experiments
  - objectives
---

# Objectives and Run Criteria

Add performance metrics to simulation runs — sum of squared errors, mass balances, permit compliance checks — for use in parameter estimation, sensitivity analysis, and scenario comparison.

## What are objectives?

Objectives (also called **run criteria**) are scalar performance metrics evaluated after each simulation run. They can represent:

- **Goodness-of-fit**: SSE between simulated and measured data
- **Permit compliance**: days exceeding an effluent limit
- **Economics**: energy cost, sludge disposal cost
- **Mass balance checks**: COD or nitrogen balance error

Objectives appear in the **Run Criteria** tab of experiment properties and in the **Runs** table after execution. They are mandatory for parameter estimation and useful for comparing scenario results.

---

## Types of run criteria

| Type | Description | Typical use |
|---|---|---|
| **Time Series** | Evaluates an expression over the simulation time series | SSE vs measured data |
| **Run** | Single scalar evaluated at end of run | Final effluent concentration |
| **BVDF** | Boundary Value Distribution Function — integrates exceedance | Permit compliance |
| **Classification** | Counts runs meeting a condition | Pass/fail criteria |

---

## Adding a run criterion

1. Open **Experiment Properties** → **Run Criteria** tab.
2. Click **Add**.
3. Choose the criterion **Type** (Time Series, Run, BVDF, or Classification).
4. Enter a **Name** (e.g. `SSE_NH4`).
5. Set the **Expression**:
   - For SSE: `(ASU2.S_NH - measured_NH4)^2` (summed over time)
   - For permit: `ASU2.S_NH > 2.0` (returns hours of exceedance)
6. For Time Series criteria, set the **Aggregation** method (Sum, Mean, Max, RMSE).
7. Click **OK**.

---

## Using measured data in objectives

To compare simulated results against measured data:

1. Import a measured data file via a **Data Input** block (see [Managing Input Data](managing-input-data.md)).
2. Reference the data block's output variable in your criterion expression.
3. The SSE is automatically computed at matching time steps.

Example expression for RMSE on effluent NH4:
```
sqrt(mean((Effluent1.S_NH4 - MeasuredData.NH4)^2))
```

---

## Viewing objectives in results

After a run, open the **Runs** tab. Each row shows one run with all objective values in columns. You can:

- **Sort** by any objective to rank scenarios
- **Export** the Runs table to Excel
- **Plot** an objective against a parameter using the scatter plot view

---

## Related

- [Parameter Estimation](../experiment-types/parameter-estimation.md)
- [Scenario Analysis](../experiment-types/scenario-analysis.md)
- [Calculator Variables](calculator-variables.md)
