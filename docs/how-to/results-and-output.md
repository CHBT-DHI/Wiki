---
tags:
  - how-to
  - results
  - output
---

# Results & Output

**Summary:** How to view, plot, configure, and export simulation results.

**Source:** WEST User Guide, Chapter 5.6–5.7.

**Prerequisites:** [Running Simulations](running-simulations.md)

---

## Output types

| Type | Block | Use |
|---|---|---|
| Wastewater output (ASM vector) | Effluent, Waste | Defractionation to standard quality parameters |
| Data output | Data Output | Write a variable time-series to file |
| Vector output | Vector Output | Write the full wastewater vector to file |

---

## Plots

Drag any variable from the **Model Explorer** or **Block Details** pane onto a Dashboard Sheet to create a time-series plot.

Right-click a plot for:
- **Add Series** — overlay multiple variables
- **Plot Properties** — change axis ranges, title, line styles
- **Series Properties** — colour, thickness, label
- **Export** — save as image or CSV
- **Crosshairs** — read exact values interactively

> **Needs content.** Expand from User Guide 5.7.3 with all plot property options.

---

## Tables

Drag a variable onto a Sheet and choose **Table** instead of Plot. Tables show current values and can be configured to show multiple columns.

> **Needs content.** From User Guide 5.7.2.

---

## Gauges

A Gauge is a real-time dial or bar indicator. Drag a variable onto a Sheet and choose **Gauge**.

> **Needs content.** From User Guide 5.7.4.

---

## Exporting to file

To write time-series output to a text file during a simulation:

1. Add a **Data Output** block (**Insert → Output → Data Output**).
2. Double-click to open the Interface Sheet.
3. Drag the variables to export from Block Details.
4. The file is written to the project folder during the simulation.

> **Needs content.** From User Guide 5.6.3 and 5.7.6.

---

## Exporting to Excel

> **Needs content.** From User Guide 5.7.7 — using the Export to Excel feature.

---

## Related

- [Running Simulations](running-simulations.md)
- [Reports](reports.md)
