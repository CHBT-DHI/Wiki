---
tags:
  - advanced-topics
  - calibration
---

# Calibration

**Summary:** Fitting WEST model parameters to measured plant data to achieve a validated model.

**Prerequisites:**

- A complete plant model with a realistic influent. See [Influent Characterisation](../technical-reference/influent-characterisation.md).
- Measured effluent and in-process data from the plant being modelled.

---

## Calibration workflow overview

1. Fix the influent characterisation before adjusting kinetic parameters.
2. Calibrate steady-state first, then dynamic.
3. Adjust only parameters with physical meaning; stay within literature ranges.
4. Validate against an independent dataset.

---

## Step 1 — Define calibration targets

> **Needs content.** Which output variables to match and what measurement uncertainty to allow.

---

## Step 2 — Manual parameter adjustment

> **Needs content.** Which parameters to adjust first (µ_max, K_S, Y), reasonable ranges, how to interpret residuals.

---

## Step 3 — Automated parameter estimation

> **Needs content.** Whether WEST includes a built-in optimiser and how to configure it.

---

## Validation

> **Needs content.** How to test the calibrated model on a second dataset.

---

## Related

- [Process Models](../technical-reference/process-models.md)
- [Advanced Simulations](../manuals/advanced-simulations.md)
