# Block Reference

Reference documentation for every block type in the WEST model library.

**Source:** WEST Models Guide (version 6, February 2026).

---

## Block categories

| Category | Pages |
|---|---|
| Biological model selection | [Choosing a Biological Model](choosing-a-model.md) · [Biological Models](biological-models.md) |
| Bioreactors | [Activated Sludge Tanks](activated-sludge-tanks.md) |
| Solids separation | [Clarifiers](clarifiers.md) · [Sludge Treatment](sludge-treatment.md) |
| Oxygen supply | [Aeration](aeration.md) |
| Hydraulics | [Flow Management](flow-management.md) |
| Control | [Controllers & Timers](controllers-timers.md) |
| Measurement | [Sensors & Signal Treatment](sensors-signal.md) |
| Specialised | [Advanced Processes](advanced-processes.md) |

---

## How blocks work

Each block in WEST is a graphical representation of a mathematical model. A block has:
- **Terminals** — connection points that carry the wastewater state vector or data signals
- **Parameters** — constants set before a run (e.g. volume, kinetic coefficients)
- **Variables** — values computed during a run (state variables, derived outputs)

The block's underlying model (e.g. `ASM2dMod.VolumeConstant`) is selected from the WEST model library and can be changed without redrawing the layout.
