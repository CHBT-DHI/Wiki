---
tags:
  - how-to
  - calculator-variables
---

# Calculator Variables

**Summary**: Define custom expressions over model variables and display or export derived quantities without modifying the model code.

**Source**: WEST User Guide §3.2.5, Tutorial §8.4.

---

### What are calculator variables?

Calculator variables are user-defined mathematical expressions evaluated at each output time step. They allow you to compute derived quantities — such as removal efficiencies, load values, or ratios — without changing the underlying Modelica model.

Examples:
- COD removal efficiency: `(COD_in - COD_eff) / COD_in * 100`
- Nitrogen load: `Q_eff * S_NO3_eff / 1000`
- Sludge production: `Q_WAS * TSS_WAS`

---

### Adding a calculator variable

1. In the layout, go to **Project → Properties → Calculator Variables** tab.
2. Click **Add**.
3. Enter a **Name** (e.g. `COD_removal`).
4. Enter the **Expression** using block names and variable names, e.g.:
   ```
   (in_1.S_S_in - out_1.S_S) / in_1.S_S_in * 100
   ```
5. Optionally set a **Unit** label (e.g. `%`).
6. Click **OK**.

The variable appears in the Model Explorer under **Calculator Variables** and can be dragged onto a plot or added to a Data Output block.

---

### Variable naming

Variable names in expressions follow the pattern `BlockName.VariableName`. Use the **Model Explorer** to browse available variables for each block. Right-click a variable in the Model Explorer → **Copy Variable Path** to paste the exact path into your expression.

---

### Supported operators and functions

| Syntax | Description |
|---|---|
| `+`, `-`, `*`, `/` | Arithmetic |
| `^` | Power |
| `abs(x)` | Absolute value |
| `sqrt(x)` | Square root |
| `exp(x)`, `log(x)` | Exponential, natural log |
| `min(x,y)`, `max(x,y)` | Minimum, maximum |
| `if(cond, a, b)` | Conditional expression |

---

### Using calculator variables in experiments

Calculator variables can be used as **objectives** in parameter estimation and sensitivity analysis. In the experiment setup, expand the **Objectives** or **Run Criteria** panel and select your calculator variable from the variable tree.

---

### Related

- [Running Simulations](running-simulations.md)
- [Results & Output](results-and-output.md)
