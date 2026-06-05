---
tags:
  - how-to
  - reports
---

# Generating Reports

**Summary:** How to create, configure, and export a WEST project report.

**Source:** WEST User Guide, Chapter 5.9.

---

## Available report types

WEST supports three principal report types:

| Report type | Contents |
|---|---|
| **Simulation summary** | Project metadata, layout snapshot, experiment settings, and key simulation statistics (run time, solver convergence, final state). |
| **Parameter report** | All model parameter values for every block in the layout, grouped by block. Useful for documentation and peer review. |
| **Results export** | Time-series or steady-state values for selected variables, exported as a data file for post-processing in Excel or MATLAB. |

---

## Creating a report

1. Go to **Project | Miscellaneous → Reports**.
2. A new empty report named "Report 1" is created and appears in the Report Manager panel.
3. In the report tree on the **Setup** tab, tick the elements to include (see recommended elements below).
4. Preview the composed report in the **Preview** tab before exporting.
5. To export, click **Save** and choose an output format (see [Format options](#format-options)).

---

## Format options

| Format | Menu option | Notes |
|---|---|---|
| **RTF / Word** | **Save → RTF** | Editable in Microsoft Word or LibreOffice. Default WEST export format. |
| **PDF** | **Print → Print to PDF** | Use your operating system's PDF printer (e.g. "Microsoft Print to PDF") from the Print dialogue. Produces a non-editable archival copy. |
| **HTML** | **Save → HTML** | Generates a self-contained HTML file with embedded images. Suitable for sharing via email or a web server. |

!!! tip
    For PDF output, install a PDF print driver (e.g. Adobe PDF, Microsoft Print to PDF) and select it from **File → Print** inside the WEST Report Viewer.

---

## Recommended elements to include

| Element | Notes |
|---|---|
| General \| Information | Project name, description, author, and creation date. |
| Layout | Snapshot of the plant layout canvas. |
| Sheets (individual plots) | Select only your key result plots — avoid including all sheets on large projects. |
| Input files | Steady-state or dynamic influent input file. |
| Model Quantities \| Include Parameters | Key parameter values for all blocks. |
| Experiments \| Run summary | Solver settings, simulation duration, convergence status. |

!!! warning
    Do not tick the top-most **Report** checkbox to include everything — generating the complete report can take a very long time on large projects with many sheets.

---

## Customising report content

You can control exactly which sections appear in the report:

1. In the **Setup** tab, expand each tree node and tick or untick individual elements.
2. To rename a report, right-click "Report 1" in the Report Manager and choose **Rename**.
3. To reorder sections, drag tree nodes up or down within the same level.
4. To add a custom text section, right-click any node and choose **Add Text Block**. This lets you insert notes or interpretation directly into the report.
5. To limit which plots appear, expand the **Sheets** node and tick only the specific named sheets you want to include.

---

## Saving and sharing reports

- **RTF file:** Click **Save → RTF**. The `.rtf` file can be opened and further edited in Microsoft Word or LibreOffice Writer.
- **HTML file:** Click **Save → HTML**. Distribute the `.html` file directly; all charts are embedded as images so recipients do not need WEST installed.
- **PDF file:** Use **Print → [PDF printer]**. PDFs are the recommended format for regulatory submission and archiving.
- **Results data export:** For raw time-series data, use **Project | Data Export** to write CSV or text files. These are independent of the Report module and suitable for import into spreadsheets or analysis tools.

!!! note
    Reports are saved inside the WEST project folder by default. Include the report file when sharing a project with colleagues or a regulatory body.

---

## Related

- [Running Simulations](running-simulations.md)
- [Results & Output](results-and-output.md)
