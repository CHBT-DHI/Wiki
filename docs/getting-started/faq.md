---
tags:
  - getting-started
  - faq
---

# Frequently Asked Questions

---

## Getting started

**What is WEST?**
: WEST (Wastewater Treatment Simulation) is a dynamic modelling and simulation platform for water quality systems — wastewater treatment plants, rivers, sewers, and urban catchments. It uses mathematical process models and a drag-and-drop graphical interface.

**What are the main editions?**
: WEST Basic (single-user, steady-state), WEST (full dynamic simulation), WEST+ (advanced experiments: sensitivity analysis, parameter estimation, scenario analysis), WEST SDK (programmatic access via .NET, MATLAB, C/C++, Java, COM, R, OpenMI APIs). See the [Quick Start Tutorial](quick-start.md) to get started with WEST.

**Where do I get sample projects?**
: Sample projects ship with WEST. From the Getting Started Sheet, select the **Samples** or **Tutorials** category in the project tree.

---

## Modelling questions

**Which biological model should I use?**
: For nitrogen removal only, start with ASM1 (simpler, well-calibrated). For nitrogen and phosphorus removal, use ASM2dMod. For systems with significant internal storage dynamics, consider ASM3. See [Choosing a Biological Model](../block-reference/choosing-a-model.md).

**What is a model Instance?**
: An Instance bundles a biological model category (e.g. ASM2dMod) with a temperature correction. Every block in a project uses the same Instance. You set the Instance when creating a new project under **Templates → Create blank Project**.

**How do I handle recycle loops?**
: Add a **Loop Breaker** block on the recycle connection. Loop breakers break the algebraic dependency by introducing a one-step delay.

**My steady-state run doesn't converge — what do I check?**
: Verify that (1) all splitter fractions sum to 1, (2) all volumes and flow rates are positive, (3) the influent input file was generated. See [Running Simulations — Convergence failures](../how-to/running-simulations.md#convergence-failures).

---

## Results & output

**How do I plot a variable during a simulation?**
: Open the **Model Explorer**, expand the block of interest, and drag the variable onto a Sheet. WEST creates a time-series plot that updates in real-time.

**How do I export results to Excel?**
: Right-click a plot and choose **Export**, or use the output-to-file option via **Insert → Output → Data Output** block.

**What is a Calculator Variable?**
: A user-defined expression over model variables (e.g. COD removal efficiency). See [Running Simulations — Calculator variables](../how-to/running-simulations.md#calculator-variables).

---

## Licensing

**What happens if the licence server is unreachable?**
: WEST will display a licence error at start-up. Check your network connection to the licence server and that the WEST licence service is running.

**Can I run WEST on a laptop without network access?**
: A local (node-locked) licence allows offline use. Contact your licence administrator to switch from a floating to a node-locked licence.

---

## Troubleshooting

**My simulation shows NaN values — what does this mean?**
: NaN (Not a Number) means a state variable went negative or a division by zero occurred. Check influent fractionation is consistent, all block volumes > 0, and splitter fractions sum to 1.

**WEST crashes when I open a project — what do I do?**
: Try opening the `.wst` file from a backup copy. If the project file is corrupt, check the autosave folder (same location as the project, prefixed with `~`).

**The layout looks correct but effluent TSS is zero — why?**
: Check the clarifier underflow recycle is connected. A Point clarifier with `f_under=1.0` sends all solids to underflow; verify the RAS connection is present.

---

## Performance tips

**How do I speed up a slow simulation?**
: Use steady-state initialisation, simplify the clarifier model, reduce the number of output variables written to file, and increase the output interval.

**Can I run multiple simulations at the same time?**
: With multiple WEST licences or a floating licence, yes — open multiple WEST instances. Each requires its own licence token.

---

!!! note "Question not answered here?"
    Open an issue on [GitHub](https://github.com/chbt-dhi/wiki/issues) or submit a pull request. See the [Contribution Guide](../contributing.md).
