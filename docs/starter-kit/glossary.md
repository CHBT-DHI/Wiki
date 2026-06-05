---
tags:
  - starter-kit
  - glossary
---

# Glossary

**Summary:** Definitions of terms, acronyms, and WEST-specific concepts used throughout this wiki.

---

## A

**ASM1, ASM2d, ASM3**
: Activated Sludge Model versions 1, 2d, and 3. IWA-standard biological process models describing the conversion of organic matter, nitrogen, and phosphorus by microbial communities. See [Process Models](../technical-reference/process-models.md).

**Aeration**
: Supply of oxygen to a bioreactor. Represented in WEST through aeration models that translate blower settings or dissolved oxygen setpoints into oxygen transfer rates.

---

## B

**Block**
: The fundamental building unit of a WEST layout. Each block represents a process unit (tank, clarifier, pump, controller, etc.) and encapsulates its mathematical model, parameters, and interface terminals.

**BNR** (Biological Nutrient Removal)
: A wastewater treatment strategy that uses biological processes to remove both nitrogen and phosphorus from the effluent, typically combining anoxic, anaerobic, and aerobic zones.

**BOD** (Biochemical Oxygen Demand)
: The amount of dissolved oxygen consumed by biological activity when organic matter is decomposed in water. BOD₅ (5-day BOD) is the standard measurement used for effluent quality reporting.

**Bioreactor**
: A tank process unit in which biological reactions occur.

---

## C

**COD** (Chemical Oxygen Demand)
: A measure of total oxidisable organic matter in water, expressed in mg/l. COD is a key influent and effluent characterisation parameter in ASM models and is typically higher than BOD.

**Connector**
: A directed link between two block terminals in WEST that transfers a stream (flow, signal, or energy) from one block to another. Connectors are drawn by dragging from an output terminal to an input terminal on the canvas.

**Controller**
: A feedback or feed-forward control element. See [Controllers](../manuals/controllers.md).

**CSTR** (Continuously Stirred Tank Reactor)
: A perfectly mixed reactor in which concentrations are uniform throughout the volume. In WEST, biological tanks are typically modelled as CSTRs; a series of CSTRs can approximate plug-flow behaviour.

---

## D

**DO** (Dissolved Oxygen)
: The concentration of oxygen dissolved in the mixed liquor of a bioreactor, typically expressed in mg O₂/l. DO is a critical control variable: aerobic biological reactions require DO > 0, while denitrification requires DO ≈ 0.

**Dynamic simulation**
: A time-varying simulation tracking how state variables evolve over a simulation period in response to changing influent conditions or control actions.

---

## E

**Effluent**
: The output stream leaving the final treatment stage. In WEST, an Effluent block maps internal ASM state variables to reportable water-quality parameters (COD, TKN, TSS, etc.) using a defractionation layout file.

**Experiment**
: A named simulation configuration in WEST that specifies the simulation type (steady-state or dynamic), solver settings, duration, and any scenario overrides. Multiple experiments can be defined within a single project.

---

## H

**HRT** (Hydraulic Retention Time)
: The average time a parcel of liquid spends in a reactor, calculated as HRT = Volume / Flow. HRT determines contact time and influences pollutant removal efficiency.

---

## I

**Influent**
: The inlet wastewater stream entering the treatment plant. See [Influent Characterisation](../technical-reference/influent-characterisation.md).

**Interface**
: The set of terminals on a WEST block through which it exchanges flows, signals, or energy with neighbouring blocks. An interface is defined in the block's WML model declaration and determines which connectors can be attached.

---

## L

**Layout**
: The process flow diagram in WEST — the graphical arrangement of process units (blocks) and their connections (connectors) that defines the plant configuration.

---

## M

**MLE** (Modified Ludzack–Ettinger)
: A classic activated sludge configuration for biological nitrogen removal, consisting of an anoxic zone followed by an aerobic zone with an internal recycle of nitrified effluent back to the anoxic zone to drive denitrification.

**MLSS** (Mixed Liquor Suspended Solids)
: The total concentration of suspended solids (active biomass + inerts) in the aeration tank mixed liquor. Typical municipal plants operate at MLSS 2 000–5 000 mg/l.

**Modelica**
: An open, object-oriented modelling language for describing complex physical systems using differential-algebraic equations. WEST compiles WML model descriptions into Modelica for simulation.

---

## P

**PID controller** (Proportional–Integral–Derivative)
: A closed-loop controller that computes a correction signal from the proportional, integral, and derivative of the error between a measured variable and its setpoint. In WEST, PID and PI blocks are available in the Control library.

---

## R

**RAS** (Return Activated Sludge)
: The thickened sludge stream recycled from the underflow of the secondary clarifier back to the inlet of the biological tanks to maintain a target MLSS concentration.

---

## S

**Scenario**
: A named variation of a layout in which selected parameter values are overridden. Scenarios allow comparison of design or operating alternatives without duplicating the entire layout.

**Setpoint**
: The target value for a controlled variable. In WEST controllers, the setpoint (`y_S`) is the desired operating value (e.g. DO = 2.0 mg/l) that the controller acts to maintain.

**Settler / Secondary clarifier**
: A process unit that separates solids from mixed liquor by gravity settling. Typically modelled in WEST with a 1D settling model (e.g. Takács) that resolves the sludge-blanket profile.

**SRT** (Sludge Retention Time) / Sludge Age
: The average time that activated sludge solids remain in the biological system, calculated as the total mass of solids divided by the daily mass wasted. SRT controls the degree of nitrification and the fraction of active biomass.

**SVI** (Sludge Volume Index)
: A measure of sludge settleability, defined as the volume (ml) occupied by 1 g of sludge after 30 minutes of settling. Low SVI (< 100 ml/g) indicates good settling; high SVI (> 200 ml/g) may indicate bulking.

**Steady-state simulation**
: A simulation that finds the equilibrium operating point of the plant, where all time derivatives are zero. Used for design sizing and initial calibration.

**State variable**
: A model variable tracked over time by differential equations (e.g. S_S, X_BH, S_O). State variables define the instantaneous condition of a process unit.

---

## T

**TKN** (Total Kjeldahl Nitrogen)
: The sum of organic nitrogen and ammonia nitrogen (NH₄⁺-N) in a water sample. TKN is a key parameter in influent characterisation and effluent reporting.

**TSS** (Total Suspended Solids)
: Mass concentration of suspended particulate matter in a water sample, expressed in mg/l. In ASM models, TSS is calculated from the sum of particulate state variables scaled by their conversion factors.

---

## V

**VSS** (Volatile Suspended Solids)
: The fraction of TSS that is combustible (organic), determined by ignition at 550 °C. VSS approximates the active and inert biological mass; VSS/TSS ratios typically range 0.7–0.85 in activated sludge.

---

## W

**WAS** (Waste Activated Sludge)
: The sludge stream intentionally removed from the biological system to control SRT. WAS is typically wasted from the RAS line or directly from the aeration tank.

**WML** (WEST Modelling Language)
: The proprietary modelling language used in WEST to define process models. WML describes model equations, parameters, state variables, and interfaces; WEST compiles WML into executable Modelica code.

---

!!! note "Missing a term?"
    Open an issue or submit a pull request. See the [Contribution Guide](../contributing.md).
