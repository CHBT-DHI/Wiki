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

### Adding variables to a plot panel

1. After running a simulation, switch to the **Dashboard** or open an existing Sheet.
2. In the **Model Explorer** or **Block Details** pane, locate the variable you want to plot (e.g. `S_NH` in an effluent block).
3. Drag the variable onto an empty area of the Sheet. WEST automatically creates a **time-series plot** panel.
4. To add a second variable to the same plot, drag it onto the existing plot panel (not onto an empty area). WEST adds it as a new series on the same axes.
5. To overlay variables from different blocks on one plot, repeat step 4 for each variable.

### Time-series plots

- The X axis represents simulation time (default unit: days). The full simulation period is shown by default.
- Each variable appears as a coloured line. The legend is shown at the bottom of the panel.
- Use the **zoom** controls (scroll wheel or the zoom toolbar buttons) to examine a specific time window.
- Use **Crosshairs** (right-click → **Crosshairs**) to read exact time and value at any point on the curve.

### Axis settings

1. Right-click the plot → **Plot Properties**.
2. In the **X Axis** tab: set custom **Min** and **Max** time values, or tick **Auto** to fit the simulation period.
3. In the **Y Axis** tab: set **Min** and **Max** for the value range. Uncheck **Auto** to fix the scale, which is useful for comparing plots across scenarios.
4. In the **General** tab: set the plot **Title** and toggle the legend on/off.
5. Right-click any individual series → **Series Properties** to change line **colour**, **thickness**, **style** (solid/dashed), and the display **label** in the legend.
6. Click **OK** to apply.

---

## Tables

### Creating a results table

1. Drag a variable from **Block Details** or **Model Explorer** onto a Sheet.
2. In the drop-down that appears, choose **Table** (instead of Plot or Gauge).
3. WEST creates a table panel showing the variable's current simulated value, updated as the simulation runs.

### Selecting output variables

- To add more variables to the same table, drag additional variables onto the existing table panel.
- Each new variable appears as an additional row (or column, depending on the table orientation setting).
- Right-click the table → **Table Properties** to:
  - Switch between **row** and **column** orientation.
  - Set the number of decimal places displayed.
  - Choose whether to show the variable name, block name, or a custom label.
- Tables are useful for comparing final steady-state values or current dynamic values at a glance during a running simulation.

---

## Gauges

A Gauge is a real-time dial or bar indicator updated continuously as the simulation runs, useful for monitoring key process variables against operating limits.

### Adding a gauge

1. Drag a variable from **Block Details** onto a Sheet.
2. In the drop-down, choose **Gauge**.
3. WEST inserts a dial-type gauge panel.

### Configuring a gauge

1. Right-click the gauge → **Gauge Properties**.
2. Set **Min** and **Max** scale values to bracket the expected operating range (e.g. 0–10 mg/L for DO).
3. Optionally define **warning** (yellow) and **alarm** (red) threshold bands to give instant visual feedback when a variable goes out of range.
4. Set the **Label** to a descriptive name (e.g. "Effluent NH₄").
5. Gauges update in real time during dynamic simulations, making them ideal for Dashboard Sheets used as operator-style displays.

---

## Exporting to file

To write time-series output to a text file during a simulation:

1. Add a **Data Output** block (**Insert → Output → Data Output**).
2. Double-click to open the Interface Sheet.
3. Drag the variables to export from Block Details into the Interface Sheet's variable list.
4. In the block's **Parameters** tab, set the **output filename** (relative path within the project folder, e.g. `results\effluent_NH4.txt`) and the **output interval** (e.g. 0.0417 d = 1 hour).
5. Run the simulation. WEST writes a tab-delimited text file to the specified path, with one column per variable and one row per output timestep.
6. The file can be opened directly in Excel or imported into any data analysis tool.

### Exporting a plot to CSV from the Dashboard

1. Right-click a plot panel → **Export**.
2. Choose **CSV** to save the plotted time-series data as a comma-delimited file, or **Image** (PNG/BMP) to save the chart graphic.
3. Select a save location and click **Save**.

---

## Exporting to Excel

WEST provides a dedicated **Export to Excel** function that sends results from the Results tab directly into a formatted Excel workbook.

1. After a simulation completes, open the **Results** tab (bottom of the WEST window).
2. In the Results tab, select the variables you want to export by ticking their checkboxes in the variable list, or use **Select All** to include everything.
3. Click the **Export** button (or go to **File → Export → Export to Excel**).
4. In the Export dialogue:
   - Choose **Excel (*.xlsx)** as the format.
   - Set the **time range** (start and end) and the **export interval** (e.g. hourly, daily).
   - Choose whether to export each variable as a separate worksheet or all on one sheet.
5. Click **Export** and choose a save location.
6. Excel opens automatically (if installed) with each selected variable's time-series in a separate column, labelled with the block name and variable name.

> **Tip:** For large simulations, reduce the export interval (e.g. export daily averages rather than every solver step) to keep the Excel file to a manageable size.

---

## Related

- [Running Simulations](running-simulations.md)
- [Reports](reports.md)
