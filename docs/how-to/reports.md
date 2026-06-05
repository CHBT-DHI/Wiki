---
tags:
  - how-to
  - reports
---

# Generating Reports

**Summary:** How to create, configure, and export a WEST project report.

**Source:** WEST User Guide, Chapter 5.9.

---

## Creating a report

1. Go to **Project | Miscellaneous → Reports**.
2. A new empty report named "Report 1" is created.
3. In the report tree (Setup tab), tick the elements to include.
4. Preview in the **Preview** tab.
5. **Print** or **Save** (RTF format for editing outside WEST).

---

## Recommended elements to include

| Element | Notes |
|---|---|
| General \| Information | Project name, description, date |
| Layout | Snapshot of the plant layout |
| Sheets (individual plots) | Select only your key plots — not all |
| Input files | Steady-state input file |
| Model Quantities \| Include Parameters | Key parameter values |

!!! warning
    Do not tick the top-most **Report** checkbox to include everything — generating the complete report can take a very long time on large projects.

---

## Related

- [Running Simulations](running-simulations.md)
- [Results & Output](results-and-output.md)
