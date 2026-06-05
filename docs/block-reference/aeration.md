---
tags:
  - block-reference
  - aeration
---

# Aeration

**Summary:** Aeration block models for dissolved oxygen transfer.

**Source:** WEST Models Guide — Aeration section (pp. 332–343).

---

## Overview

Aeration blocks are connected to bioreactor blocks via the aeration terminal. They translate an aeration input signal (e.g. air flow rate, blower speed, or DO set-point) into an oxygen transfer rate (`kLa`).

| Model | Description |
|---|---|
| `Aeration.Ideal` | `kLa` is directly the manipulated input (`y_S` = DO set-point) |
| `Aeration.IdealMulti` | Multiple zones with independent set-points |
| `Aeration.IdealMulti2` | Two-compartment ideal aeration |
| `Aeration.Surface` | Surface aeration model |
| `Aeration.FineBubble1` | Fine-bubble diffuser model (single-zone) |
| `Aeration.FineBubble2` | Fine-bubble diffuser model (two-zone) |

---

## Aeration.Ideal

The simplest and most commonly used aeration model. The controller drives the DO set-point `y_S`, and the aeration block computes `kLa` to maintain that set-point.

**Key parameters:**

| Parameter | Description |
|---|---|
| `y_S` | DO set-point (mg O₂/l) — typically 1.5–2.5 |
| `K_La_max` | Maximum oxygen transfer coefficient (h⁻¹) |

In the TwoASU example, a PI controller sets `y_S` and the `Aeration.Ideal` block (inside the Activated Sludge block) maintains it.

---

## Blowers

For energy modelling, replace `Aeration.Ideal` with a blower-connected model. Blower block types:

| Model | Description |
|---|---|
| `Blowers.CF_Q_VFD` | Centrifugal fan, flow-controlled VFD |
| `Blowers.CF_HQ_VFD` | Centrifugal fan with H-Q curve |
| `Blowers.CF_Q_IGV` | Centrifugal fan with inlet guide vanes |
| `Blowers.PD_Q_VFD` | Positive displacement blower |

> **Needs content.** Expand from Models Guide pp. 152–165, with energy calculation guidance.

---

## Related

- [Activated Sludge Tanks](activated-sludge-tanks.md)
- [Controllers & Timers](controllers-timers.md)
- [Advanced Processes](advanced-processes.md)
