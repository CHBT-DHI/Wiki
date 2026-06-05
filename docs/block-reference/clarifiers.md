---
tags:
  - block-reference
  - clarifiers
---

# Clarifiers

**Summary:** Primary and secondary clarifier block variants.

**Source:** WEST Models Guide — Primary Clarifiers (pp. 266–276), Secondary Clarifiers (pp. 278–296).

---

## Primary clarifiers

| Model | Description |
|---|---|
| `PrimaryClarifier.Point` | Simple point-settler model |
| `PrimaryClarifier.OtterpohlFreund` | Physical model with density current |
| `PrimaryClarifier.Takacs_SVI` | Takács model with SVI correction |
| `PrimaryClarifier.Lamellae_Takacs` | Lamella clarifier |
| `PrimaryClarifier.BSM2` | Benchmark Simulation Model No. 2 primary clarifier |

---

## Secondary clarifiers

| Model | Description |
|---|---|
| `SecondaryClarifier.Point` | Ideal point-settler (no settling kinetics) |
| `SecondaryClarifier.Takacs_SVI` | 1D Takács model with SVI — **most common** |
| `SecondaryClarifier.BurgerDiehl30` | 30-layer Bürger-Diehl model (high accuracy) |
| `SecondaryClarifier.Lamellae_Takacs` | Lamella secondary clarifier |
| `SecondaryClarifier.Point_wVolumePre` | Point-settler with pre-thickening zone |
| `SecondaryClarifier.TakacsSVI_wVolumePre` | Takács with pre-thickening zone |

---

## Takács_SVI parameters

The Takács model is the standard 1D settling model used in BSM1 and most municipal WWTP models.

| Parameter | Description | Typical value |
|---|---|---|
| `A` | Surface area | m² — design-specific |
| `H` | Depth | 4–6 m |
| `Q_Under` | Underflow (return sludge) rate | m³/d |
| `r_P` | Slow-settling exponent | 0.003 m³/g |
| `v0` | Maximum Vesilind settling velocity | 474 m/d |
| `r_H` | Hindered-settling parameter | 0.000576 m³/g |
| `f_ns` | Non-settleable fraction | 0.00228 |

> **Needs content.** Add full parameter tables and guidance on setting surface overflow rate from Models Guide pp. 281–283.

---

## Related

- [Activated Sludge Tanks](activated-sludge-tanks.md)
- [Sludge Treatment](sludge-treatment.md)
