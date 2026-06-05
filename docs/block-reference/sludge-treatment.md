---
tags:
  - block-reference
  - sludge
---

# Sludge Treatment

**Summary:** Thickeners, digesters, drying, biogas, and dewatering block types.

**Source:** WEST Models Guide — Thickeners (pp. 298–303), Aerobic Digesters (pp. 305–307), Sludge Drying (pp. 308–310), Biogas (pp. 311–320).

---

## Thickeners

| Model | Description |
|---|---|
| `Tanks_Dewater.ThickenerDS` | Gravity thickener with dry solids output |
| `Tanks_Dewater.ThickenerEfficiency` | Efficiency-based thickener |
| `Tanks_Dewater.ThickenerEfficiency2` | Two-stream efficiency thickener |

> **Needs content.** Parameters and typical thickening efficiency values.

---

## Aerobic digesters

| Model | Description |
|---|---|
| `Tanks_AerDig.VolumeConstant` | Aerobic digester at constant volume |

> **Needs content.** Parameters from Models Guide p. 306.

---

## Sludge drying

| Model | Description |
|---|---|
| `SludgeDrying_Ideal` | Ideal sludge dryer (evaporates water to target DS%) |

---

## Biogas accumulation and utilisation

| Model | Description |
|---|---|
| `GasHolder` | Biogas storage tank |
| `GasBoiler` | Biogas combustion for heat |
| `GasEngine` | CHP gas engine |
| `GasTurbine_Simple` | Simple gas turbine |
| `Flare` | Biogas flare |

> **Needs content.** Expand from Models Guide pp. 311–320.

---

## Related

- [Advanced Processes](advanced-processes.md)
- [Aeration](aeration.md)
