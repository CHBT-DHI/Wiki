---
tags:
  - block-reference
  - biological-models
---

# Choosing a Biological Model

**Summary:** Which model Instance to select when creating a project, and what each covers.

**Source:** WEST Models Guide, Introduction; WEST Getting Started Tutorial, ch 2.

---

## Model Instance vs model category

In WEST, every project is created with a **model Instance** that bundles:
- A **biological model category** (e.g. ASM2dMod) — defines the state variables and process kinetics
- A **temperature correction** model (e.g. Temp = Arrhenius correction)

All bioreactor blocks in a project use the same Instance.

---

## Available model categories

| Instance name | Biological model | Nutrient removal | Typical use |
|---|---|---|---|
| `ASM1Temp` | ASM1 | C, N (nitrification/denitrification) | Municipal WWTP, calibration, teaching |
| `ASM2dModTemp` | ASM2dMod | C, N, P (biological phosphorus) | BNR plants, P-removal design |
| `ASM2dModNDHATemp` | ASM2dModNDHA | C, N, P + nitrous oxide | Greenhouse gas modelling |
| `ASM2dISSTemp` | ASM2dISS | C, N, P + inorganic suspended solids | Plants with mineral precipitation |
| `PWM_SATemp` | PWM_SA | Plant-Wide Model: anaerobic digestion + WWTP | Full plant energy/resource modelling |

---

## Decision guide

```
Need P removal? 
  Yes → ASM2dMod (standard) or ASM2dISS (with mineral precipitation)
  No  → ASM1 (simpler, fewer parameters)

Need N₂O / greenhouse gas accounting?
  Yes → ASM2dModNDHA

Modelling digester + full energy balance?
  Yes → PWM_SA
```

---

## Changing an Instance after project creation

Go to **Project | Properties → General** tab → change the **Instance** field. WEST will warn if any existing blocks are incompatible with the new model.

---

## Related

- [Biological Models](biological-models.md) — detailed description of each model
- [Quick Start Tutorial](../getting-started/quick-start.md) — uses ASM1Temp
