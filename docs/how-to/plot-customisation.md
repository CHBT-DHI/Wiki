---
tags:
  - how-to
  - plots
  - visualisation
---

# Plot and Dashboard Customisation

Customise plot appearance, add multiple series, use different chart types, and build informative dashboard sheets.

**Source**: WEST User Guide §2.10.14–2.10.15, §5.7.3–5.7.4, Tutorial §8.1.

---

### Creating a plot

1. On a Dashboard Sheet, go to **Insert → Output → Plot** (or drag a variable directly from the Model Explorer — WEST creates a plot automatically).
2. Resize and position the plot window by dragging its corners.

---

### Adding series to a plot

Right-click the plot → **Add Series** → choose the series type:

| Series type | When to use |
|---|---|
| **Line** | Time-series variables during a dynamic run |
| **Bar** | Comparing discrete values across scenarios |
| **Scatter (X,Y)** | Correlation between two variables |
| **Histogram** | Distribution of a variable across UA/SA runs |
| **Z(X,Y)** | 3D surface for two-parameter sensitivity |

For each series, select the variable from the Model Explorer or type the variable path.

---

### Plot properties

Double-click the plot (or right-click → **Properties**) to open the Plot Properties dialog:

**General tab:**
- Title, font size, background colour
- Show/hide legend, gridlines

**X-axis / Y-axis tabs:**
- Axis label and units
- Manual min/max scale (uncheck Auto-scale)
- Logarithmic scale option
- Secondary Y-axis (right axis) for dual-unit plots

**Series tab:**
- Colour, line style, marker shape for each series
- Line thickness
- Smoothing (moving average)

---

### Plot types for experiment results

For sensitivity and scenario analysis, additional plot types are available in the Results tab:

| Plot type | Description |
|---|---|
| **CRS plot** | Cumulative relative sensitivity — ranked parameter influence |
| **Tornado chart** | Horizontal bar chart of ±sensitivity per parameter |
| **Scatter matrix** | All-pairs scatter for GSA correlation |
| **Uncertainty envelope** | Percentile bands around the median run |
| **Cumulative distribution** | CDF of output variable across UA runs |

---

### Gauge widgets

Gauges display a single variable as an analogue dial or numeric indicator:

1. **Insert → Output → Gauge** to place a gauge.
2. Double-click → set the variable, min/max range, and warning/alarm thresholds.
3. The gauge updates in real-time during a live dynamic simulation.

Gauge types: Dial, Linear (horizontal/vertical bar), Digital (numeric display).

---

### Saving and reusing plot layouts

- Right-click a sheet tab → **Save Sheet as Template** to save the dashboard layout.
- Apply a template to a new project via **Insert → Sheet → From Template**.

---

### Related

- [Results & Output](results-and-output.md)
- [Reports](reports.md)
