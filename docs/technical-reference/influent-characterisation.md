---
tags:
  - technical-reference
  - influent
---

# Influent Characterisation

**Summary:** How to translate measured wastewater data into the state variable fractions required by WEST process models.

**Prerequisites:** Familiarity with the [process model](process-models.md) you are using.

---

## Why influent characterisation matters

WEST models represent wastewater as model state variables. Real lab measurements (total COD, BOD, TSS) must be fractionated into these components. Errors here are one of the most common causes of poor model calibration.

---

## Standard characterisation procedure

Influent characterisation converts bulk lab measurements into ASM component fractions. The procedure below is for **ASM1**; similar steps apply to ASM2d (with additional P and VFA fractionation).

### Step 1 — COD fractionation

Total COD (COD_T) is split into four primary fractions:

| Fraction | Symbol | Measurement / estimation method |
|---|---|---|
| Soluble inert COD | S_I | Effluent soluble COD from a fully nitrifying plant (non-biodegradable) |
| Readily biodegradable COD | S_S | Soluble COD after flocculation filtration, minus S_I |
| Particulate inert COD | X_I | Mass balance: X_I = COD_T − S_I − S_S − X_S − X_BH (or from VSS analysis) |
| Slowly biodegradable COD | X_S | Settled COD minus active biomass and inerts |
| Active heterotrophic biomass | X_BH | Estimated from VSS and oxygen uptake rate (OUR) batch tests |

Key relationships:
```
COD_T = S_I + S_S + X_I + X_S + X_BH  (for primary influent, X_BA ≈ 0)
S_S   ≈ filtered COD (0.45 µm) − S_I
X_S   ≈ BOD_ult / (1 − f_cv · Y_H) − X_BH  (approximate)
```

### Step 2 — TKN fractionation

Total Kjeldahl Nitrogen (TKN) is split as follows:

| Fraction | Symbol | Method |
|---|---|---|
| Ammonium nitrogen | S_NH | Direct NH4 measurement |
| Soluble biodegradable organic N | S_ND | Soluble TKN − S_NH |
| Particulate biodegradable organic N | X_ND | Particulate TKN − nitrogen in biomass (i_XB · X_BH) |

Nitrogen content factors: i_XB = 0.086 g N/g COD (biomass), i_XP = 0.06 g N/g COD (inert products).

### Step 3 — Dissolved oxygen and alkalinity

- **S_O2**: measure directly (DO probe); typically set to 0 g/m³ for raw influent.
- **S_NO**: measure directly; near zero for untreated municipal influent.
- **S_ALK**: measure as bicarbonate alkalinity (mg CaCO3/L); convert to molar: S_ALK [mol/m³] = alkalinity [mg CaCO3/L] / 50.

### Step 4 — ASM2d extensions (if applicable)

For ASM2d, additionally determine:
- **S_F** (fermentable substrate) and **S_A** (VFAs/acetate): separate via short-term anaerobic batch test or SCFA analysis.
- **S_PO4**: direct soluble phosphate measurement.
- **X_PP**, **X_PAO**, **X_PHA**: typically zero for raw influent (present only in return streams from EBPR sludge).

### Recommended lab analyses

| Measurement | Method |
|---|---|
| Total and filtered COD | ISO 15705 / Standard Methods 5220 |
| BOD5 | ISO 5815 |
| TSS / VSS | ISO 11923 / Standard Methods 2540 |
| TKN | ISO 5663 |
| NH4-N | ISO 7150 |
| NO3+NO2-N | ISO 13395 |
| Total P / ortho-P | ISO 6878 |
| Alkalinity | Standard Methods 2320 |
| OUR (batch) | Respirometry (e.g. APHA, 2005) |

---

## Importing an influent file

Prepared influent time-series data can be imported into WEST using the **Data Editor** (File → New → Data or double-click a `.txt`/`.xls` file in the project tree). The file must have a header row listing component names that match the model's state variable names. After importing, connect the data file to the **Influent** block by setting its **File** parameter to the imported data file path. WEST reads the file at runtime and interpolates between timesteps. See [Managing Input Data](../how-to/managing-input-data.md) for file format requirements.

### Supported formats
WEST accepts influent time series as:
- **CSV / TXT** — comma- or tab-delimited, UTF-8. One row per time point.
- **Excel (.xlsx)** — via the Data Input block's built-in import dialog.
- **DHI MIKE time series (.dfs0)** — native DHI binary format for large datasets.

### File structure (CSV)
```
Time [d], Q [m3/d], S_I [g/m3], S_S [g/m3], X_I [g/m3], X_S [g/m3], X_BH [g/m3], S_O2 [g/m3], S_NO [g/m3], S_NH [g/m3], S_ALK [mol/m3], ...
0.000,    20000,    30,          115,         25,          85,          0,            0,            0,            35,          7.0, ...
0.042,    22500,    30,          120,         25,          90,          0,            0,            0,            37,          7.1, ...
```

- The **Time** column must be in days from the simulation start (or calendar date if using the absolute-time option).
- Column headers must match the component symbols defined in the active process model exactly (case-sensitive).
- Missing columns default to zero; a warning is raised in the simulation log.

### Loading into a WEST project
1. Place a **Data Input** block on the layout canvas.
2. Double-click the block and select **Browse** to locate the file.
3. Set **Interpolation method** (linear is standard; step for discrete measurements).
4. Connect the Data Input output port to the **Influent** block's composition inlet.
5. In the Influent block properties, set **Input mode** to "External (Data Input)".
6. Run a short steady-state initialisation first to confirm the loaded values appear in the results explorer.

Alternatively, use the **WEST Influent Tool** (standalone utility supplied with WEST) to guide the COD fractionation calculation and export a ready-to-use influent file.

---

## Typical influent values

The table below gives indicative ranges for **untreated municipal wastewater** after primary clarification (where applicable). Values vary significantly with catchment characteristics, sewer detention time, and season.

### Raw / primary-treated municipal influent

| Parameter | Raw influent | After primary clarification | Unit |
|---|---|---|---|
| Flow (dry weather) | — | — | m³/d (plant-specific) |
| Total COD | 400 – 600 | 250 – 400 | g COD/m³ |
| BOD5 | 200 – 300 | 120 – 200 | g BOD/m³ |
| TSS | 200 – 400 | 80 – 150 | g TSS/m³ |
| VSS / TSS ratio | 0.75 – 0.85 | 0.70 – 0.80 | — |
| TKN | 40 – 60 | 35 – 50 | g N/m³ |
| NH4-N | 25 – 40 | 25 – 40 | g N/m³ |
| Total P | 6 – 10 | 5 – 8 | g P/m³ |
| Ortho-P | 3 – 5 | 3 – 5 | g P/m³ |
| Alkalinity | 200 – 400 | 200 – 400 | mg CaCO3/L |
| Temperature | 10 – 20 | 10 – 20 | °C |

### ASM1 component fractions (typical primary effluent)

| Component | Symbol | Typical value | Unit |
|---|---|---|---|
| Soluble inerts | S_I | 25 – 40 | g COD/m³ |
| Readily biodegradable COD | S_S | 80 – 130 | g COD/m³ |
| Particulate inerts | X_I | 20 – 35 | g COD/m³ |
| Slowly biodegradable COD | X_S | 80 – 150 | g COD/m³ |
| Active heterotrophic biomass | X_BH | 10 – 30 | g COD/m³ |
| Ammonium | S_NH | 25 – 40 | g N/m³ |
| Soluble organic N | S_ND | 2 – 6 | g N/m³ |
| Particulate organic N | X_ND | 3 – 8 | g N/m³ |
| Dissolved oxygen | S_O2 | 0 | g O2/m³ |
| Nitrate | S_NO | 0 – 1 | g N/m³ |
| Alkalinity | S_ALK | 4 – 8 | mol/m³ |

### Diurnal flow variation
Municipal wastewater flow typically follows a diurnal pattern with a **peak-to-average ratio of 1.5–2.5** and a minimum at night of 0.4–0.6 × average. Peak loads in COD and NH4 may be offset by 1–2 hours from the flow peak. Use measured 24-hour composite samples where available, and apply diurnal scaling factors if only average daily values are measured.

---

## Related

- [Running Simulations](../manuals/running-simulations.md)
- [FAQ](../starter-kit/faq.md#can-i-import-my-own-influent-data)
