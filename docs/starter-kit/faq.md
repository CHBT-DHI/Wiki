---
tags:
  - starter-kit
  - faq
---

# Frequently Asked Questions

**Summary:** Answers to the most common questions about using WEST.

---

## Getting started

### What is WEST?

WEST is a wastewater process modelling tool from DHI. It allows engineers to build dynamic simulation models of wastewater treatment plants, test operational scenarios, and evaluate process performance — without running physical experiments.

### Where do I download WEST?

Contact DHI or your DHI regional representative for a licensed installer. See [Installation](../manuals/installation.md) for setup steps once you have the installer.

### What are the system requirements?

| Component | Minimum | Recommended |
|---|---|---|
| Operating system | Windows 10 64-bit | Windows 11 64-bit |
| RAM | 8 GB | 16 GB or more |
| Disk space | 5 GB free | 10 GB free (for project files and results) |
| CPU | Dual-core 2 GHz | Quad-core 3 GHz or faster |
| Display | 1280 × 768 | 1920 × 1080 or higher |
| Other | USB port for dongle (if using hardware licence) | Network licence recommended for VMs and RDP |

WEST is a 64-bit Windows application and does not run natively on macOS or Linux.

---

## Modelling questions

### What is the difference between a layout and a scenario?

A **layout** is the process flow diagram — the set of blocks (tanks, clarifiers, controllers, influent sources) and the connections between them. It defines the physical structure of the plant being modelled.

A **scenario** is a set of parameter values applied to a layout. The layout stays the same, but different scenarios can specify different flow rates, kinetic parameters, controller setpoints, or influent time-series.

**Example:** You have a layout representing an activated sludge plant. You create three scenarios on that layout: *Baseline* (current operation), *Peak load* (doubled influent flow), and *Upgrade* (increased aeration capacity). All three share the same tank-and-clarifier structure but have different parameter values. Running a Scenario Analysis compares their results side by side.

See [Scenario Management](../advanced-topics/scenario-management.md) for the full workflow.

### Why does my simulation not converge?

Convergence failures are usually caused by one or more of the following:

1. **Poor initial conditions** — If the model starts far from a feasible state (e.g. zero biomass in all tanks), the solver may struggle through the early transient. Run a Steady State simulation first, save the state, and use it as the starting condition for dynamic runs.
2. **Solver step size too large** — Open **Experiment Settings → Solver** and reduce the maximum step size (try 0.005–0.01 d). For stiff models, switching to an implicit solver (DASSL or LSODA) also helps.
3. **Mass balance errors in the layout** — Check that all flow connections are complete and that influent and effluent flows balance. An unclosed loop or a missing waste stream can drive state variables to physically impossible values.
4. **Extreme parameter values** — Very high growth rates or very low half-saturation coefficients can create numerical stiffness. Check that all kinetic parameters are within literature ranges.
5. **Negative concentrations** — If any state variable goes negative, the solver may diverge. Add a lower-bound constraint or check whether a connected flow is reversing unexpectedly.

If the problem persists after the above checks, simplify the model (e.g. replace the secondary clarifier with a point settler) to isolate the failing component.

### How do I set initial conditions?

See [Running Simulations](../manuals/running-simulations.md) for the standard approach and [Advanced Simulations](../manuals/advanced-simulations.md) for warm-starting from a previous run.

### Can I import my own influent data?

Yes. WEST supports two main approaches:

1. **Data Input block** — Add a **Data Input** block to the layout and connect it to the influent source block. Double-click the Data Input block and import a CSV file with columns for time and each influent variable (e.g. flow, COD, TSS, NH₄). WEST interpolates between data points during the simulation.
2. **Influent Tool** — The built-in Influent Tool (accessible from **Tools → Influent Tool**) generates a synthetic diurnal or weekly influent time-series from daily average loads. It can also import measured data and fit a statistical pattern.

CSV files must have a header row and use either comma or tab as the delimiter. Time should be in days (or in the unit configured in Project Settings). See [Influent Characterisation](../technical-reference/influent-characterisation.md) for guidance on fractionating raw plant data into ASM model components.

---

## Results and output

### Where are my simulation results stored?

Simulation results are stored inside the **WEST project file** (`.wst` extension), in the same folder as the project. The project file is a binary archive that contains the model layout, all scenarios, experiment settings, and the results of the most recent simulation runs.

In addition:
- If you have added **Data Output** blocks to the layout, WEST writes tab-delimited text files to the project folder during the simulation. These files are named according to the block's configured filename.
- Exported CSV or Excel files are saved wherever you choose in the Export dialogue.

To locate your project file, go to **File → Project Properties** — the project path is shown at the top.

### How do I export results to Excel or CSV?

**From the Results tab (recommended for full time-series):**

1. After a simulation completes, open the **Results** tab at the bottom of the WEST window.
2. Select the variables to export by ticking their checkboxes.
3. Click **Export** → choose **Excel (*.xlsx)** or **CSV (*.csv)**.
4. Set the time range and output interval, then click **Export** and choose a save location.

**From a plot panel (quick export of plotted data):**

1. Right-click any plot panel on a Dashboard Sheet.
2. Choose **Export → CSV** to save the plotted time-series, or **Export → Image** to save a chart graphic.

See [Results & Output](../how-to/results-and-output.md) for the full procedure including Excel-specific options.

---

## Licensing and installation

### WEST says my licence has expired. What do I do?

1. **Check the dongle connection** — If you are using a hardware USB dongle, unplug it and re-insert it. Try a different USB port. Check that the DHI HASP driver is installed and running (visible in Windows Device Manager under "Universal Serial Bus controllers").
2. **Check the licence expiry date** — Open WEST → **Help → Licence Information** to see the current licence details and expiry date.
3. **Renew the licence** — Contact your DHI regional representative or log in to the DHI Customer Portal (portal.dhigroup.com) to request a licence renewal or extension.
4. **Activate a new licence file** — If DHI has issued a new licence file (`.v2c` or `.h2r`), apply it using the **DHI Licence Manager** (Start → DHI → DHI Licence Manager → Update Licence).
5. **Network licence issues** — If your organisation uses a network (floating) licence server, contact your IT administrator to verify that the licence server service is running and reachable from your workstation.

If you need urgent access, DHI support can issue a temporary emergency licence. Contact support@dhigroup.com with your licence number and a description of the issue.

### Can I run WEST on a virtual machine or remote desktop?

Yes, with some limitations:

- **Remote Desktop (RDP):** WEST runs normally over RDP on a Windows Server or workstation. Performance may be reduced for graphics-intensive dashboard layouts. Use a **network (floating) licence** rather than a dongle-based licence, as the licence server needs to be reachable from the server rather than the client machine.
- **Virtual machines (VMware, Hyper-V, etc.):** WEST can run in a VM, but USB dongle pass-through is required if using a hardware licence. USB pass-through reliability varies by hypervisor and host configuration. A **network licence** is strongly recommended for VM deployments to avoid dongle pass-through issues.
- **Cloud VMs (AWS, Azure, etc.):** Supported with a network licence. The licence server must be installed on a VM within the same network, or a VPN must be configured so the WEST VM can reach the on-premises licence server.
- **Citrix / VDI:** Supported with a network licence. Contact DHI support for specific Citrix configuration guidance.

In all VM and RDP scenarios, ensure the WEST installation uses the same Windows locale and regional settings as your data files to avoid CSV import issues with decimal separators.

---

!!! tip "Can't find your question?"
    Check the [Glossary](glossary.md) for terminology, or browse the [Manuals](../manuals/index.md) for step-by-step guidance.
