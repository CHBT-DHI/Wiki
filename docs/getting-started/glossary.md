---
tags:
  - getting-started
  - glossary
---

# Glossary

**Summary:** Definitions of terms, acronyms, and WEST-specific concepts used throughout this wiki.

---

## A

**ASM1, ASM2d, ASM3**
: Activated Sludge Model versions 1, 2d, and 3. IWA-standard biological process models. See [Biological Models](../block-reference/biological-models.md).

**Aeration**
: Supply of oxygen to a bioreactor. Represented in WEST through aeration models that translate blower settings or dissolved oxygen setpoints into oxygen transfer rates.

---

## B

**Bioreactor**
: A tank process unit in which biological reactions occur.

**Block**
: A process unit placed on the WEST Layout Sheet canvas. Each block is the graphical and computational representation of a unit process (e.g. a CSTR bioreactor, a settler, an influent generator). Blocks are dragged from the Block Library onto the canvas and connected via terminals.

**Block Library**
: The palette of available process blocks, shown as a dockable panel on the left side of the WEST window. Blocks are organised by category (biological reactors, settlers, sensors, controllers, influent/effluent generators, etc.). Drag a block from the library onto the Layout Sheet to create an instance.

**BSM** (Benchmark Simulation Model)
: A standardised wastewater treatment plant model defined by the IWA Task Group on Benchmarking of Control Strategies for WWTPs. BSM1 and BSM2 provide fixed plant configurations, influent files, and evaluation criteria, allowing fair comparison of control strategies across different research groups. WEST ships with BSM-compatible model components.

---

## C

**COD** (Chemical Oxygen Demand)
: A measure of total oxidisable organic matter. A key state variable in ASM models.

**Controller**
: A feedback or feed-forward control element. See [Controllers](../how-to/controllers.md).

---

## D

**Dynamic simulation**
: A time-varying simulation tracking how state variables evolve over a simulation period.

---

## E

**Effluent**
: The output stream leaving the final treatment stage.

**Effluent generator**
: An Output block in WEST that collects the final treated wastewater stream and performs de-fractionation — converting ASM model state variables back into standard effluent quality parameters (COD, TKN, TSS, TP). The results are used for compliance assessment and reporting.

**Experiment**
: A configured simulation run in WEST. An experiment defines the simulation type (steady-state, dynamic, sensitivity analysis, etc.), the solver and integrator settings, the output variables to record, and the simulation duration. Experiments are managed via **Project → Virtual Experiments**.

---

## F

**Feed-forward control**
: A control strategy that acts on a measured disturbance (e.g. influent flow or load) before it affects the controlled variable, as opposed to reactive feedback control.

**Fractionation**
: The conversion of bulk wastewater measurements (COD, TKN, TSS) into individual ASM model state variables. Performed by the Municipal/Industrial influent block in WEST.

**Flux**
: Mass per unit area per unit time (g m⁻² d⁻¹). Used especially in settler modelling to describe the downward transport of solids.

---

## G

**Gujer matrix** (stoichiometric matrix)
: A tabular representation of a biokinetic model in which rows are processes and columns are state variables; each cell contains the stoichiometric coefficient relating that component to that process.

**GSA** (Global Sensitivity Analysis)
: A technique that quantifies how uncertainty in each model parameter contributes to uncertainty in model output across the full parameter space. Available via the WEST Sensitivity Analysis module.

---

## H

**HRT** (Hydraulic Retention Time)
: The average time that liquid remains in a reactor, equal to volume divided by flow rate (V/Q). A key design parameter for biological reactors.

**Hydrolysis**
: A biological process by which slowly biodegradable particulate COD (X_S) is broken down into readily biodegradable soluble COD (S_S), enabling further aerobic or anoxic degradation.

---

## I

**Influent**
: The inlet wastewater stream. See [Managing Input Data](../how-to/managing-input-data.md).

---

## K

**Kinetic parameter**
: A model constant that governs the rate of a biological or chemical process (e.g. maximum growth rate μ_max, half-saturation constant K_S). Calibrated to match measured plant data.

**KLa** (oxygen transfer coefficient)
: The volumetric mass-transfer coefficient for oxygen (d⁻¹). Represents the efficiency of oxygen transfer from air bubbles to the liquid phase; used as the aeration input in WEST aerobic reactor models.

---

## L

**Layout**
: The process flow diagram in WEST — the graphical arrangement of process units and connections.

---

## M

**MLSS** (Mixed Liquor Suspended Solids)
: The total concentration of suspended solids (active biomass + inert material) in the aeration tank mixed liquor. Typically 2–5 g/L in conventional activated sludge and 8–12 g/L in MBR systems.

**MLE** (Modified Ludzack-Ettinger)
: A two-zone activated sludge process configuration consisting of a pre-anoxic zone followed by an aerobic zone with internal recirculation, used for biological nitrogen removal.

**Model Instance**
: A WEST-specific concept representing one instantiation of a process model class (e.g. one CSTR using ASM2d). Each Instance carries its own parameter set and state variables.

**Modelica**
: An equation-based, object-oriented modelling language used as the foundation of WML (WEST Modelling Language). WEST model libraries are written in a Modelica-derived syntax.

---

## N

**Nitrification**
: The two-step biological oxidation of ammonium (NH₄⁺) first to nitrite (NO₂⁻) and then to nitrate (NO₃⁻), carried out by autotrophic nitrifying bacteria. Requires aerobic conditions.

**Nitrifier** (autotrophic biomass)
: Slow-growing bacteria (represented as X_AUT in ASM2d/ASM3) responsible for nitrification. Sensitive to low DO, low temperature, and toxic compounds.

---

## O

**ODE** (Ordinary Differential Equation)
: The mathematical form of mass-balance equations used in WEST reactor models; each state variable is described by dX/dt = (sum of reaction rates). WEST's solver integrates these over simulation time.

**Output block**
: A WEST block that collects the wastewater vector leaving the plant for reporting and de-fractionation into standard effluent quality parameters (e.g. TSS, TKN, TP).

---

## P

**PAO** (Phosphorus Accumulating Organism)
: Bacteria (X_PAO in ASM2d) capable of taking up large amounts of phosphorus under aerobic conditions after being exposed to anaerobic conditions. Key organisms in enhanced biological phosphorus removal (EBPR).

**PID controller**
: A Proportional-Integral-Derivative feedback controller. In WEST, the PID controller block takes a measured variable and setpoint as inputs and produces a manipulated variable output (e.g. aeration KLa). See [Controllers](../how-to/controllers.md).

**Process model**
: The mathematical description of a unit process (e.g. ASM1 for a bioreactor, Takács for a settler). Assigned to a block via the ClassName property.

---

## R

**RAS** (Return Activated Sludge)
: The sludge stream recycled from the bottom of the secondary clarifier back to the inlet of the aeration tank to maintain the desired MLSS concentration.

**Recycle**
: Any internal flow stream that returns material from a downstream unit back to an upstream unit (e.g. RAS, internal recirculation in MLE/BNR configurations).

**Run criteria**
: Conditions that terminate a simulation run (e.g. reaching steady state within a defined tolerance, or exceeding a maximum simulation time). Configured in the Scenario settings in WEST.

---

## S

**Scenario**
: A simulation run configuration within a layout.

**Settler / Secondary clarifier**
: A process unit that separates solids from mixed liquor. Typically modelled with a 1D settling model (e.g. Takács).

**Steady-state simulation**
: A simulation that finds the equilibrium operating point of the plant.

**State variable**
: A model variable tracked over time by differential equations (e.g. S_S, X_BH, S_O).

---

## T

**TSS** (Total Suspended Solids)
: Mass concentration of suspended particulate matter.

---

## U

**UA** (Uncertainty Analysis)
: A technique that propagates parameter uncertainty through a model to quantify the resulting uncertainty in model outputs. Typically uses Monte Carlo sampling. Available via the WEST Sensitivity Analysis module.

---

## V

**VSS** (Volatile Suspended Solids)
: The fraction of TSS that combusts at 550 °C, used as a proxy for organic (biological) solids content. VSS ≈ 0.75–0.85 × TSS in typical activated sludge.

**VolumeConstant**
: A WEST reactor block variant in which the reactor volume is fixed (does not vary with inflow/outflow imbalance). Most bioreactor blocks in the standard library use VolumeConstant behaviour.

---

## W

**WAS** (Waste Activated Sludge)
: The excess biomass wasted from the system (from the return sludge stream or directly from the aeration tank) to control SRT and maintain MLSS at the design level.

**WML** (WEST Modelling Language)
: The Modelica-derived language used to write WEST process model libraries. WML files define block interfaces, parameters, state variables, and balance equations.

**WWTP** (Wastewater Treatment Plant)
: A facility that removes pollutants from wastewater before discharge. WEST is used to model, design, and optimise WWTPs.

---

!!! note "Missing a term?"
    Open an issue or submit a pull request. See the [Contribution Guide](../contributing.md).
