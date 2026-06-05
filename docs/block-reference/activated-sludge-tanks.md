---
tags:
  - block-reference
  - bioreactors
---

# Activated Sludge Tanks

**Summary:** Reference for bioreactor block variants in the WEST model library.

**Source:** WEST Models Guide — Activated Sludge Tanks section (pp. 202–217).

---

## Overview

Activated sludge tanks are the biological reactor blocks. They implement the biological model assigned to the project Instance (ASM1, ASM2dMod, etc.).

| Model variant | Description |
|---|---|
| `VolumeConstant` | Fixed volume, single compartment — most common |
| `VolumePumped` | Volume with pump-driven inflow |
| `VolumeVariable` | Volume varies with inflow/outflow balance |
| `VolumeConstant02–10` | Multi-input variants for complex configurations |
| `OxidationDitch.Sector` | Single sector of an oxidation ditch |
| `OxidationDitch.Ditch04 / Ditch06` | 4- or 6-sector oxidation ditch |
| `SBR_PS_R01–R08` | Sequencing Batch Reactor phases (fill, react, settle, decant) |

---

## VolumeConstant (standard bioreactor)

The most commonly used bioreactor block.

**Key parameters:**

| Parameter | Description | Typical value |
|---|---|---|
| `V` | Tank volume | m³ — design-specific |
| `Temp` | Temperature | 15–20 °C |
| `kLa` | Oxygen transfer coefficient (if aeration is internal) | h⁻¹ |

**Connection terminals:**
- Flow in (blue arrow in) — wastewater vector
- Flow out (blue arrow out) — wastewater vector
- Aeration (red square top) — connects to an Aeration block
- DO sensor (red circle) — connects to a sensor or controller

> **Needs content.** Add full parameter table from Models Guide pp. 203–204.

---

## Biofilm models

WEST also includes biofilm reactor blocks for attached-growth processes:

| Model | Process |
|---|---|
| `Filter1D` | 1D biofilm filter |
| `MBBR` | Moving Bed Biofilm Reactor |
| `GranuleCSTR` | Granular sludge CSTR |
| `MABR` | Membrane Aerated Biofilm Reactor |
| `FBBR` | Fixed Bed Biofilm Reactor |

> **Needs content.** Expand from Models Guide pp. 254–265.

---

## Related

- [Biological Models](biological-models.md)
- [Aeration](aeration.md)
- [Controllers & Timers](controllers-timers.md)
