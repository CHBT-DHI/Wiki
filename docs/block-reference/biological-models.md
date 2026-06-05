---
tags:
  - block-reference
  - biological-models
---

# Biological Models

**Summary:** The biological process models available in WEST — state variables, processes, and key parameters.

**Source:** WEST Models Guide, pp. 8–27 (ASM2dMod); WEST Getting Started Tutorial, ch 2–3.

---

## Model overview

| Model | Primary use | Nutrient removal | State variables |
|---|---|---|---|
| ASM1 | Carbon + nitrification/denitrification | N only | 13 |
| ASM2dMod | Biological N and P removal | N and P | 20 |
| ASM2dModNDHA | N, P + nitrous oxide | N, P, N₂O | 21 |
| ASM2dISS | N, P + inorganic suspended solids | N, P, mineral | 21 |
| PWM_SA | Plant-wide (WWTP + digester) | Full resource model | Multiple |

---

## ASM1

ASM1 is the IWA standard model for carbon removal, nitrification, and denitrification. It is the most widely calibrated biological model for municipal wastewater and the recommended starting point for most projects.

**Key assumptions:**

- Biomass divided into heterotrophs (`X_BH`) and autotrophs (`X_BA`)
- Oxygen and nitrate as electron acceptors
- No phosphorus dynamics

> **Needs content.** State variable table, process matrix summary, key kinetic parameters and typical values from Models Guide.

---

## ASM2dMod

ASM2dMod is a WEST modification of the IWA ASM2d model (Gernaey and Jørgenson, 2004). It extends ASM2d to make decay process rates electron-acceptor dependent. It covers carbon removal, nitrification, denitrification, and biological phosphorus removal (EBPR) via phosphate-accumulating organisms (PAOs).

### State variables (component vector)

| Name | Description | Unit |
|---|---|---|
| Q | Water flow | m³/d |
| S_I | Inert soluble matter | g COD/m³ |
| S_O | Dissolved oxygen | g COD/m³ |
| S_N2 | Nitrogen gas | g N/m³ |
| S_F | Fermentable, readily biodegradable organic matter | g COD/m³ |
| S_A | Fermentation products (acetate) | g COD/m³ |
| S_NO | Nitrate + nitrite nitrogen (NO3-N + NO2-N) | g N/m³ |
| S_PO | Inorganic soluble phosphorus (ortho-phosphates) | g P/m³ |
| S_NH | Ammonium nitrogen (NH4-N) | g N/m³ |
| S_ALK | Alkalinity | mol |
| X_I | Inert particulate matter | g COD/m³ |
| X_S | Slowly biodegradable matter | g COD/m³ |
| X_H | Heterotrophic organisms | g COD/m³ |
| X_PAO | Phosphorus-accumulating organisms | g COD/m³ |
| X_PP | Poly-phosphate | g P/m³ |
| X_PHA | Cell internal storage product of PAOs | g COD/m³ |
| X_AUT | Autotrophic (nitrifying) biomass | g COD/m³ |
| X_TSS | Total suspended solids | g TSS/m³ |
| X_MeOH | Metal hydroxides | g COD/m³ |
| X_MeP | Metal phosphate | g COD/m³ |

**Component groupings:**

| Organic matter | | | | |
|---|---|---|---|---|
| Soluble — Inert | Soluble — Fermentable | Soluble — Fermentation products | Particulate — Inert | Particulate — Slowly biodegradable |
| S_I | S_F | S_A | X_I | X_S |

| Biomass | | | | |
|---|---|---|---|---|
| Autotrophic | Heterotrophic | PAO biomass | PAO — Poly-phosphate | PAO — PHA |
| X_AUT | X_H | X_PAO | X_PP | X_PHA |

### Processes (conversion model)

| # | Name | Description |
|---|---|---|
| P1 | AerHydrol | Aerobic hydrolysis of X_S |
| P2 | AnHydrol | Anoxic hydrolysis of X_S |
| P3 | AnaerHydrol | Anaerobic hydrolysis of X_S |
| P4 | AerGrowthOnSf | Aerobic growth of X_H on fermentable substrate S_F |
| P5 | AerGrowthOnSa | Aerobic growth of X_H on fermentation products S_A |
| P6 | AnGrowthOnSfDenitrif | Anoxic growth of X_H on S_F (denitrification) |
| P7 | AnGrowthOnSaDenitrif | Anoxic growth of X_H on S_A (denitrification) |
| P8 | Fermentation | Conversion of S_F → S_A (anaerobic) |
| P9 | LysisOfHetero | Lysis of heterotrophic organisms X_H |
| P10 | StorageOfXPHA | Storage of cell internal organic material X_PHA |
| P11 | AerStorageOfXPP | Aerobic storage of poly-phosphate X_PP |
| P12 | AnStorageOfXPP | Anoxic storage of poly-phosphate X_PP |
| P13 | AerGrowthOnXPHA | Aerobic growth of PAOs on X_PHA |
| P14 | AnGrowthOnXPHADenitrif | Anoxic growth of PAOs on X_PHA (denitrification) |
| P15 | LysisOfXPAO | Lysis of X_PAO |
| P16 | LysisOfXPP | Lysis of X_PP |
| P17 | LysisOfXPHA | Lysis of X_PHA |
| P18 | GrowthOfAuto | Growth of autotrophic (nitrifying) organisms X_AUT |
| P19 | LysisOfAuto | Lysis of X_AUT |
| P20 | Precipitation | Simultaneous phosphorus precipitation |
| P21 | Redissolution | Phosphorus redissolution |
| P22 | Aeration | Oxygen transfer |

### Nitrification–denitrification inhibition

Denitrification is inhibited by dissolved oxygen via a separate parameter **K_O_Denit** (default 0.2 mg O₂/l). Increasing `K_O_Denit` increases the denitrification rate at a given DO level without affecting other processes that use `K_O`.

![Effect of K_O_Denit on nitrate utilisation rate at different DO levels](../assets/images/asm2dmod-k-o-denit-graph.png)

*Nitrate Utilisation Rate (NUR) vs DO at three values of K_O_Denit. Default is 0.2 for both K_O and K_O_Denit.*

### Temperature dependency

Kinetic parameters are corrected for temperature using Arrhenius expressions:

```
k_i(Temp) = k_i(TempRef) × θ_ki ^ (Temp_Actual − Temp_Ref)
```

where `Temp_Ref` = 20°C and `θ_ki` is the Arrhenius coefficient for parameter `k_i`.

Oxygen saturation concentration:

```
S_O_Saturation = 14.65 − 0.41·T + 0.00799·T² − 7.78×10⁻⁵·T³   [g/m³]
```

### Parameters — Kinetics (default values at 20°C)

| Name | Description | Default | Unit |
|---|---|---|---|
| `mu_H` | Maximum growth rate for heterotrophs | 6 | 1/d |
| `mu_AUT` | Maximum growth rate for autotrophs | 1 | 1/d |
| `mu_PAO` | Maximum growth rate for PAOs | 1 | 1/d |
| `b_H` | Lysis/decay rate for heterotrophs | 0.4 | 1/d |
| `b_AUT` | Decay rate for autotrophs | 0.15 | 1/d |
| `b_PAO` | Lysis rate for X_PAO | 0.2 | 1/d |
| `b_PHA` | Lysis rate for X_PHA | 0.2 | 1/d |
| `b_PP` | Lysis rate for X_PP | 0.2 | 1/d |
| `k_h` | Hydrolysis rate constant | 3 | 1/d |
| `Q_fe` | Maximum fermentation rate | 3 | 1/d |
| `Q_PHA` | Rate constant for PHA storage | 3 | 1/d |
| `Q_PP` | Rate constant for PP storage | 1.5 | 1/d |
| `k_PRE` | Rate constant for P precipitation | 1 | 1/d |
| `k_RED` | Rate constant for P redissolution | 0.6 | 1/d |
| `K_O` | Saturation/inhibition coeff for oxygen | 0.2 | g/m³ |
| `K_O_AUT` | Saturation/inhibition coeff for autotrophs for O₂ | 0.5 | g/m³ |
| `K_O_Denit` | Inhibition coeff of denitrifiers for O₂ | 0.2 | g/m³ |
| `K_NH` | Saturation coeff for ammonium | 0.05 | g/m³ |
| `K_NH_AUT` | Saturation coeff of autotrophs for NH₄ | 1 | g/m³ |
| `K_NO` | Saturation/inhibition coeff for nitrate | 0.5 | g/m³ |
| `K_F` | Saturation coeff for growth on S_F | 4 | g/m³ |
| `K_A` | Saturation coeff for S_A (acetate) | 4 | g/m³ |
| `K_P` | Saturation coeff for phosphorus | 0.01 | g/m³ |
| `K_ALK` | Saturation coeff for alkalinity | 0.1 | g/m³ |
| `K_ALK_AUT` | Saturation coeff of autotrophs for alkalinity | 0.5 | g/m³ |
| `K_X` | Saturation coeff for particulate COD | 0.1 | g/g |
| `K_MAX` | Maximum ratio X_PP/X_PAO | 0.34 | g/m³ |
| `K_IPP` | Inhibition coeff for X_PP storage | 0.02 | g/m³ |
| `K_PHA` | Saturation coeff for PHA | 0.01 | g/m³ |
| `K_PP` | Saturation coeff for poly-phosphate | 0.01 | g/g |
| `K_PS` | Saturation coeff for P in PP storage | 0.2 | g/m³ |
| `K_fe` | Saturation coeff for fermentation on S_F | 4 | g/m³ |
| `n_NO_Het` | Reduction factor for denitrification | 0.8 | — |
| `n_NO_Hyd` | Anoxic hydrolysis reduction factor | 0.6 | — |
| `n_fe` | Anaerobic hydrolysis reduction factor | 0.4 | — |
| `n_NO_PAO` | PAO activity under anoxic conditions | 0.6 | — |
| `n_NO_Het_d` | Anoxic reduction factor for decay of heterotrophs | 0.5 | — |
| `n_NO_AUT_d` | Anoxic reduction factor for decay of autotrophs | 0.33 | — |
| `n_NO_P_d` | Anoxic reduction factor for decay of PAO/PP/PHA | 0.33 | — |

### Parameters — Stoichiometry

| Name | Description | Default | Unit |
|---|---|---|---|
| `Y_H` | Yield for heterotrophic biomass | 0.625 | g COD/g COD |
| `Y_AUT` | Yield for autotrophic biomass | 0.24 | g COD/g COD |
| `Y_PAO` | Yield for PAOs | 0.625 | g COD/g COD |
| `Y_PHA` | PHA requirement for PP storage | 0.2 | g COD/g COD |
| `Y_PO` | PP release per PHA stored | 0.4 | g COD/g COD |
| `f_X_I` | Fraction of inert COD generated in biomass lysis | 0.1 | — |
| `f_S_I` | Fraction of inert COD in particulate substrate | 0.0 | — |

### Parameters — Composition

| Name | Description | Default | Unit |
|---|---|---|---|
| `i_N_BM` | Nitrogen content of biomass | 0.07 | g N/g COD |
| `i_N_S_F` | Nitrogen content of soluble substrate S_F | 0.03 | g N/g COD |
| `i_P_BM` | Phosphorus content of biomass | 0.02 | g P/g COD |
| `i_P_S_F` | Phosphorus content of S_F | 0.01 | g P/g COD |
| `i_TSS_BM` | TSS to biomass ratio (X_H, X_PAO, X_AUT) | 0.9 | g TSS/g COD |
| `i_TSS_X_I` | TSS to X_I ratio | 0.75 | g TSS/g COD |
| `i_TSS_X_S` | TSS to X_S ratio | 0.75 | g TSS/g COD |

### Parameters — Temperature corrections (Arrhenius θ values)

| Name | Parameter corrected | Default |
|---|---|---|
| `theta_mu_H` | mu_H | 1.072 |
| `theta_mu_AUT` | mu_AUT | 1.111 |
| `theta_mu_PAO` | mu_PAO | 1.041 |
| `theta_b_H` | b_H | 1.072 |
| `theta_b_AUT` | b_AUT | 1.116 |
| `theta_b_PAO` | b_PAO | 1.072 |
| `theta_b_PHA` | b_PHA | 1.072 |
| `theta_b_PP` | b_PP | 1.072 |
| `theta_k_h` | k_h | 1.041 |
| `theta_Q_fe` | Q_fe | 1.072 |
| `theta_Q_PHA` | Q_PHA | 1.041 |
| `theta_Q_PP` | Q_PP | 1.041 |
| `theta_K_X` | K_X | 0.896 |

### Fractionation parameters (influent characterisation)

| Name | Description | Default | Unit |
|---|---|---|---|
| `F_BOD_COD` | Ratio of BOD to COD | 0.65 | g BOD/g COD |
| `F_TSS_COD` | Ratio of TSS to COD | 0.75 | g TSS/g COD |
| `f_S_A` | Soluble COD to S_A ratio | 0.25 | — |
| `f_S_F` | Soluble COD to S_F ratio | 0.375 | — |
| `f_S_NH` | Total N to ammonia ratio | 0.6 | — |
| `f_S_PO` | Total P to PO ratio | 0.6 | — |
| `f_X_H` | Particulate COD to X_H ratio | 0.17 | — |
| `f_X_S` | Particulate COD to X_S ratio | 0.69 | — |

### Interface variables (measurable outputs from bioreactor block)

| Name | Terminal | Description | Unit |
|---|---|---|---|
| `DO` | out_2 | Dissolved oxygen | g/m³ |
| `NH4` | out_2 | Ammonium concentration | g/m³ |
| `NO3` | out_2 | Nitrate + nitrite | g/m³ |
| `PO4` | out_2 | Phosphate | g/m³ |
| `TSS` | out_2 | Total suspended solids | g/m³ |
| `OnlineCOD` | out_2 | Chemical oxygen demand | g/m³ |
| `OnlineTN` | out_2 | Total nitrogen | g/m³ |
| `OnlineTP` | out_2 | Total phosphorus | g/m³ |
| `OUR_ASU` | out_2 | Oxygen uptake rate | g/m³/d |
| `NUR` | out_2 | Nitrate utilisation rate | g/m³/d |
| `PUR` | out_2 | Phosphate uptake rate | g/m³/d |
| `Kla_ASU` | out_2 | kLa | 1/d |
| `V_ASU` | out_2 | Volume | m³ |

### References

Gernay, K.V. and Jørgenson, S.B. (2004) Benchmarking combined biological phosphorous and nitrogen removal wastewater treatment processes. *Control Engineering Practice* 12:357–373.

Henze, M., Gujer, W., Mino, T. and van Loosdrecht, M. (2006) *Activated Sludge Models ASM1, ASM2, ASM2d and ASM3*. IWA Publishing.

---

## ASM2dModNDHA

Extension of ASM2dMod adding nitrous oxide (N₂O) as an intermediate in denitrification. Used for greenhouse gas (GHG) modelling.

> **Needs content.** From Models Guide pp. 29–56.

---

## ASM2dISS

Extension of ASM2dMod adding inorganic suspended solids dynamics (mineral precipitation/dissolution). Relevant for plants with chemical P-removal or high mineral loads.

> **Needs content.** From Models Guide pp. 57–83.

---

## PWM_SA

Plant-Wide Model combining ASM-based WWTP activated sludge processes with anaerobic digestion, enabling full carbon, energy, and resource balances across the entire plant.

> **Needs content.** From Models Guide pp. 84–96.

---

## Related

- [Choosing a Biological Model](choosing-a-model.md)
- [Activated Sludge Tanks](activated-sludge-tanks.md)
- [Glossary](../getting-started/glossary.md)
