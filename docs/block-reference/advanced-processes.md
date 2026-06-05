---
tags:
  - block-reference
  - advanced
---

# Advanced Processes

**Summary:** Chemical dosing, disinfection, energy, catchment, cost, and other specialised blocks.

**Source:** WEST Models Guide — various sections.

---

## Chemical dosing

| Model | Chemical | Purpose |
|---|---|---|
| `Dose_Acetate` | Acetate | Carbon source for denitrification |
| `Dose_Ethanol` | Ethanol | Carbon source |
| `Dose_Methanol` | Methanol | Carbon source |
| `Dose_Alum` | Alum (Al₂(SO₄)₃) | Chemical P-precipitation |
| `Dose_FeOH` | Iron hydroxide | P-precipitation |
| `Dose_FeCl3` | Ferric chloride | P-precipitation |
| `Dose_Fe2SO4` | Ferrous sulphate | P-precipitation |

---

## Screening & grit removal

| Model | Description |
|---|---|
| `Screening.Screen_Ideal` | Ideal bar screen (removes rags, large solids) |
| `GritRemoval.Grit_Ideal` | Ideal grit chamber |

---

## Buffer tanks

Used for flow equalisation:

| Model | Description |
|---|---|
| `Tanks_Buffer.VolumeConstant` | Fixed-volume buffer |
| `Tanks_Buffer.VolumePumped` | Buffer with pump drainage |
| `Tanks_Buffer.StormTank` | Storm overflow tank |

---

## Disinfection

| Model | Agent |
|---|---|
| `Disinfection.Chlorine` | Chlorination |
| `Disinfection.ChlorineInv` | Chlorination with inactivation |
| `Disinfection.PAA_02` | Peracetic acid |
| `Disinfection.UV` | UV disinfection |

---

## Quaternary treatment

| Model | Description |
|---|---|
| `GAC_01bed` | Granular activated carbon (single bed) |
| `CarbonActive.Default` | Generic activated carbon |
| `GAC_10beds` | 10-bed GAC system |

---

## Energy & heat exchange

| Model | Description |
|---|---|
| `HeatExchanger.Simple` | Simple heat exchanger |
| `HeatExchanger.SludgeRaw` | Raw sludge heat recovery |
| `HeatExchanger.SludgeRecirc` | Recirculation heat exchanger |
| `HeatExchanger.HeatPump` | Heat pump |
| `Energy.SolarPV_Simple` | Solar PV power generation |
| `Energy.Wind_Simple` | Wind power generation |

---

## Cost calculators

| Model | Description |
|---|---|
| `CostCalculators.Operation_Simple` | Simple operational cost (energy + chemicals) |
| `CostCalculators.Operation_wCFootprint` | Cost with carbon footprint |

---

## Catchment and sewer (IUWS)

For integrated urban water system models (Tutorial ch 15):

| Category | Models |
|---|---|
| Generators | `DWF2` (dry weather flow), `Calc_Evaporation`, `Runoff` |
| Catchment | `Combined_NoVol`, `Combined_WithVol` |
| Sewer Tank | `Freeflow`, `Runoff2`, `Retention_NoVol` |

---

## Related

- [Worked Examples — IUWS](../worked-examples/iuws.md)
- [Sludge Treatment](sludge-treatment.md)
