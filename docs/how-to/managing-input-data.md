---
tags:
  - how-to
  - influent
  - input-data
---

# Managing Input Data

**Summary:** How to create influent input files, configure fractionation models, and import time-series data.

**Source:** WEST User Guide, Chapter 5.4.

**Prerequisites:** [Building a Plant Layout](building-layouts.md)

---

## Input types

| Type | Block | Use |
|---|---|---|
| Wastewater input (ASM vector) | Municipal wastewater, Industrial, Truck load | Standard influent with biological model fractionation |
| Data input | Data Input | Time-series CSV/TXT file driving a single variable |
| Vector input | Vector Input | Time-series file driving the full wastewater vector |

---

## Standard wastewater input (fractionation)

The **Influent Generator** creates an input file by fractionating measured parameters (BOD, COD, TSS, TKN, etc.) into biological model state variables.

1. Double-click the influent block to open the **Influent Generator** (Interface Sheet).
2. **General tab**: select the fractionation layout XML file (e.g. `WEST.ASM1.Input.Layout.xml` for ASM1, or the ASM2dMod equivalent for ASM2d).
3. **Fractionation tab**: enter measured concentrations or import from a data file.
4. **Data Import tab**: select a time-series influent file (e.g. `WEST.BODCOD.Month.Influent.txt`).
5. **Generate**: click to produce the `.Steady State.in.txt` and `.Dynamic.in.txt` input files.

> **Needs content.** Expand fractionation tab fields and explain the XML layout files (User Guide 5.4.1, 5.4.5).

---

## Custom influent vector

> **Needs content.** From User Guide 5.4.2 — defining a custom state variable mapping.

---

## Data input (single variable time-series)

> **Needs content.** From User Guide 5.4.3 — CSV format, header requirements, linking to top-level interface variable.

---

## Merging data sources

> **Needs content.** From User Guide 2.9.5 — how to combine multiple input sources.

---

## Related

- [Building a Plant Layout](building-layouts.md)
- [Running Simulations](running-simulations.md)
- [Influent & Effluent blocks](../block-reference/advanced-processes.md)
