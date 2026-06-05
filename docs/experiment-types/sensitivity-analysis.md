---
tags:
  - experiment-types
  - sensitivity-analysis
---

# Sensitivity Analysis

**Summary:** Quantify how model outputs (e.g. effluent TSS, S_NO) respond to changes in parameters (e.g. DO set-point, sludge settling properties).

**Source:** WEST Getting Started Tutorial, Chapters 9 (Local) and 10 (Global).

**Prerequisites:** [Running Simulations](../how-to/running-simulations.md) · WEST+ licence

---

## Local Sensitivity Analysis (LSA)

A Local Sensitivity Analysis computes a numerical approximation of how state variables, worker variables, or output variables change with respect to parameters or initial conditions, evaluated at a single operating point.

**What it answers:** "If I change parameter X by 1%, how much does output Y change?"

**When to use:** Understanding dominant parameters; identifying which parameters to focus calibration on.

### Setting up an LSA

1. Open (or save a copy of) the TwoASU project.
2. In **Project | Virtual Experiments**, click **Local Sensitivity Analysis** and select the base Dynamic experiment.
3. Open the **Analysis Properties** dialogue.
4. In the **Parameters and Variables** tab, drag the parameters to perturb (e.g. `Aeration.y_S`, `Clarifier.r_P`, `Clarifier.v0`, `InternRecycle.Q_Out2`) from Block Details into the Parameters panel.
5. Drag the output variables of interest (e.g. `out_1.TSS`, `out_1.Outflow(S_NO)`) into the Variables panel.
6. In **Simulation Output** tab, ensure Communication Interval ≠ 0 (e.g. 0.014).
7. Click **Execute**.

### Interpreting LSA results

Results appear in the **Runs** tab as Central Relative Sensitivity (CRS) functions — time-varying sensitivity of each output to each parameter. Plots show which parameters have the largest influence and when.

**TwoASU example findings:** Effluent TSS is most sensitive to sludge characteristics (`r_P`, `v0`) and occasionally to internal recycle. Nitrate concentration is most sensitive to internal recycle, also to `v0`.

> **Needs content.** Add CRS plot description and table of typical sensitivities from Tutorial ch 9.

---

## Global Sensitivity Analysis (GSA)

A Global Sensitivity Analysis samples the full parameter space (Monte Carlo) and uses linear regression to rank parameter importance across all operating conditions.

**What it answers:** "Which parameters matter most across the whole plausible range?"

**When to use:** Screening before detailed calibration; understanding uncertainty sources.

### Setting up a GSA

1. Save a copy of the TwoASU project.
2. In **Project | Virtual Experiments**, click **Global Sensitivity Analysis** and select the Dynamic experiment. Enable **Run a slave Steady-State simulation prior to Dynamic simulation**.
3. In **Analysis Properties → Parameters** tab, drag parameters and assign their distributions:

| Block | Parameter | Distribution | Settings |
|---|---|---|---|
| Aeration | `y_S` | Normal | Mean: 1.5, StDev: 0.05 |
| Clarifier | `r_P` | Normal | Mean: 0.00286, StDev: 0.0005 |
| Clarifier | `v0` | Normal | Mean: 474, StDev: 100 |
| Clarifier | `Q_Under` | Uniform | 10 000–30 000 m³/d |
| R_NO (internal recycle) | `Q_Out2` | Uniform | 20 000–70 000 m³/d |
| R_Sludge (wastage) | `Q_Out2` | Uniform | 100–500 m³/d |

4. In **Solver** tab, set Number of Shots to 50.
5. Click **Execute**.

### Interpreting GSA results

- **Linear Regression \| Scalars**: aggregated regression statistics per objective.
- **Linear Regression \| Vectors**: regression coefficients (LCC, PCC, SRC) — create a Tornado plot by selecting an objective and clicking **Plot**.
- **Runs tab**: objective values for each Monte Carlo run; best run is highlighted.

> **Needs content.** Add Tornado plot interpretation guidance from Tutorial ch 10.

---

## Related

- [Parameter Estimation](parameter-estimation.md)
- [Running Simulations](../how-to/running-simulations.md)
- [TwoASU Worked Example](../worked-examples/twoasu-mle.md)
