---
tags:
  - how-to
  - effluent
  - fractionation
---

# Effluent Characterisation and De-fractionation

**Summary**: Convert ASM model state variables back into measurable effluent quality parameters (COD, BOD, TSS, TN, TP) for comparison with permit limits and measured data.

**Source**: WEST User Guide §2.9.6, §5.6.5; Tutorial §7.5.

---

### What is de-fractionation?

ASM models track individual components (S_S, X_BH, S_NO3, etc.) rather than bulk parameters. De-fractionation converts these back to the conventional quality parameters reported in permit monitoring:

| Bulk parameter | Calculated from |
|---|---|
| Total COD | Sum of all COD-bearing components × iCOD factors |
| Soluble COD | Soluble components only (S_S, S_I, S_A…) |
| BOD₅ | Biodegradable COD fraction × BOD conversion factor |
| TSS | Sum of particulate components × iSS factors |
| VSS | Volatile fraction of TSS |
| TN (Total Nitrogen) | S_NH4 + S_NO3 + S_NO2 + organic N components |
| TKN | S_NH4 + organic N (S_ND, X_ND) |
| TP (Total Phosphorus) | S_PO4 + polyphosphate + biomass-bound P |
| Alkalinity | S_ALK (meq/L) |

---

### Setting up an Effluent Output block

1. From the Block Library, drag an **Output** block (Input and Output palette → `Effluent`) onto the canvas and connect it to the stream you want to monitor.
2. Double-click the Output block to open its properties.
3. Go to the **General** tab → set the **Model** to `Effluent Generator`.
4. Go to the **De-fractionation** tab.

---

### Configuring the de-fractionation model

In the **De-fractionation** tab you can:

- **Use the default de-fractionation**: WEST provides a standard layout that maps ASM components to bulk parameters using stoichiometric factors from the model Instance. Click **Load Default** to populate the layout automatically.
- **Customise the de-fractionation**: drag calculation blocks (Sum, Multiply, Constant) onto the de-fractionation canvas to define your own conversion expressions.
- **Add additional outputs**: right-click the de-fractionation canvas → **Add Output Variable** → name it (e.g. `BOD5_eff`) and write the expression.

Key stoichiometric factors used:

| Factor | Description | Typical value |
|---|---|---|
| `iCOD_VSS` | COD per VSS | 1.48 g COD/g VSS |
| `iSS_VSS` | SS per VSS | 1/0.75 (for active biomass) |
| `iN_XBH` | N content of active heterotrophs | 0.086 g N/g COD |
| `iP_XBH` | P content of active heterotrophs | 0.02 g P/g COD |

---

### Viewing effluent quality

After a simulation run, drag variables from the de-fractionation output (e.g. `Effluent1.COD_eff`, `Effluent1.TN_eff`) onto a plot sheet. These can also be used as **objectives** in parameter estimation.

---

### Related

- [Managing Input Data](managing-input-data.md) — influent fractionation (the reverse process)
- [Results & Output](results-and-output.md)
- [Parameter Estimation](../experiment-types/parameter-estimation.md)
