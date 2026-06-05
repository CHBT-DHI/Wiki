---
tags:
  - worked-examples
  - iuws
  - catchment
---

# Integrated Urban Water System (TWOASU_IUWS)

**Summary:** Extend the TwoASU model with a catchment, sewer network, and receiving river — modelling the full urban water cycle in one layout.

**Source:** WEST Getting Started Tutorial, Chapter 15.

**Prerequisites:** [TwoASU MLE Layout](twoasu-mle.md)

---

## What is an IUWS model?

An Integrated Urban Water System model connects:

1. **Catchment** — rainfall-runoff, surface runoff loads
2. **Sewer** — combined/separate sewer transport and storage
3. **WWTP** — the TwoASU treatment plant
4. **River** — receiving water quality

This allows assessment of combined sewer overflows (CSOs), storm events, and their downstream impact.

---

## Layout extension steps

### Step 1 — Adapt the WWTP layout

Start from the TwoASU project. Modify the influent block to accept:
- Dry weather flow (from DWF2 Generator)
- Storm runoff (from Catchment blocks)

> **Needs content.** From Tutorial 15.1 (pp. 101–102).

### Step 2 — Set up the catchment and sewer layout

Add on a second layout region (or a linked sub-model) for:

- `DWF2` Generator — dry weather flow characterisation
- `Catchment.Combined_NoVol` or `Combined_WithVol` — surface runoff
- `SewerTank.Freeflow` / `Retention_NoVol` — sewer transport and CSO tank

> **Needs content.** From Tutorial 15.2 (pp. 103–104).

### Step 3 — Set up the river layout

Add river blocks downstream of the WWTP effluent:

> **Needs content.** From Tutorial 15.3 (pp. 104).

### Step 4 — Run the simulation

> **Needs content.** From Tutorial 15.4 (pp. 105–107).

---

## Related

- [TwoASU MLE Layout](twoasu-mle.md)
- [Advanced Processes — Catchment blocks](../block-reference/advanced-processes.md)
