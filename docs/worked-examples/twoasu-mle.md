---
tags:
  - worked-examples
  - twoasu
---

# TwoASU MLE Layout

**Summary:** The standard WEST sample project — a Modified Ludzack-Ettinger (MLE) layout with two activated sludge tanks, a secondary clarifier, and DO control.

**Source:** WEST Getting Started Tutorial, Chapters 7–8.

---

## Layout

The TwoASU project models a typical municipal WWTP for biological nitrogen removal:

```
Municipal influent → Combiner → Anoxic tank (2 000 m³) → Aerobic tank (4 000 m³) → Clarifier
                          ↑                                                               |
                    Loop breaker ← Internal recycle splitter (55 338 m³/d) ←————————————┘
                                                                                         |
                                                       Wastage (385 m³/d) → Effluent (sludge)
```

**Model Instance:** ASM1Temp  
**Biological model:** ASM1 (nitrification + denitrification, no P removal)  
**Controller:** PI controller maintaining aerobic tank DO at 1.5 mg O₂/l

For build instructions, see the [Quick Start Tutorial](../getting-started/quick-start.md).

---

## Key parameters

| Block | Parameter | Value |
|---|---|---|
| Anoxic tank | Volume | 2 000 m³ |
| Aerobic tank | Volume | 4 000 m³ |
| Aerobic tank | DO set-point | 1.5 mg O₂/l |
| Clarifier | Underflow Q | 18 831 m³/d |
| Internal recycle | Flow | 55 338 m³/d |
| Wastage | Flow | 385 m³/d |

---

## Steady-state results

Typical effluent concentrations at steady-state:

| Variable | Value |
|---|---|
| COD | ~46 mg/l |
| NHx | ~7.6 mg/l |
| NOx | ~6.5 mg/l |
| TSS | ~15.9 mg/l |

---

## Dynamic results (30-day run)

Running with `WEST.BODCOD.Month.Influent.txt` (monthly load variation, VODE integrator):

| Variable | Mean ± StDev | 5–95 percentile |
|---|---|---|
| COD | 46.1 ± 5.8 mg/l | 36.1–54.1 |
| NHx | 7.6 ± 6.3 mg/l | 0.5–21.4 |
| NOx | 6.5 ± 2.1 mg/l | 3.4–10.5 |
| TSS | 15.9 ± 3.0 mg/l | 12.3–21.6 |

---

## Further analysis

This project is the starting point for the advanced experiment tutorials:

- [Sensitivity Analysis](../experiment-types/sensitivity-analysis.md) — which parameters drive TSS and NO?
- [Parameter Estimation](../experiment-types/parameter-estimation.md) — calibrate to real plant data
- [Scenario Analysis](../experiment-types/scenario-analysis.md) — compare operating strategies
- [Uncertainty Analysis](../experiment-types/uncertainty-analysis.md) — propagate parameter uncertainty

---

## Related

- [Quick Start Tutorial](../getting-started/quick-start.md) — step-by-step build instructions
- [Running Simulations](../how-to/running-simulations.md)
