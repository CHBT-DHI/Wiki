---
tags:
  - how-to
  - simulation
  - initialisation
---

# Initialising Models

Set good initial conditions for a dynamic run by using steady-state results or manually specifying state variable values.

## Why initialisation matters

Dynamic simulations start from initial conditions. If initial conditions are far from the operating point, the simulation may:
- Take a long time to reach a realistic state (months of simulation time)
- Show unrealistic transients at the start
- Fail to converge if initial conditions are infeasible

A good initialisation dramatically reduces spin-up time.

---

## Method 1: Run steady-state first (recommended)

1. Set up a **Steady-State experiment** (Project → Experiments → Steady State).
2. Run it to convergence.
3. The steady-state results are automatically stored as initial conditions for the dynamic run.
4. Switch to your **Dynamic experiment** and run — it starts from the steady-state point.

This is the standard workflow for most WWTP simulations.

---

## Method 2: Warm-start from a previous dynamic run

If you have a previous dynamic run that reached a realistic operating state:

1. Run the dynamic simulation to a point that represents a good operating state (e.g. after 100 days of simulation).
2. Click **Simulation → Save State** (or use the toolbar button).
3. In the dynamic experiment properties, set **Initial conditions** → **From saved state file**.
4. The next run starts from the saved state, skipping the spin-up period.

---

## Method 3: Manual initial conditions

For each block, you can manually set state variable initial values:

1. Double-click the block → **Initial Conditions** tab.
2. Enter values for each state variable (e.g. X_BH = 2000 g/m³, S_O = 2 g/m³).
3. These values are used if no steady-state result is available.

Typical starting values for an activated sludge tank operating at SRT = 10 days:

| Variable | Typical value |
|---|---|
| X_BH (active heterotrophs) | 1500–2500 g COD/m³ |
| X_BA (active autotrophs) | 100–200 g COD/m³ |
| X_S (slowly biodegradable) | 100–300 g COD/m³ |
| S_O (dissolved oxygen) | 1–2 g/m³ |
| S_NH (ammonium) | 1–5 g N/m³ |
| S_NO (nitrate) | 5–15 g N/m³ |

---

## Checking initialisation quality

After running steady-state, inspect:
- **Effluent NH4**: should be < 5 mg/L for a nitrifying system
- **Effluent TSS**: should be 5–15 mg/L
- **MLSS**: should match design target (3000–5000 mg/L typically)
- **Mass balance**: check that COD in ≈ COD in effluent + COD in WAS + CO2 (via respiration)

If steady-state fails to converge, see [Troubleshooting](troubleshooting.md#simulation-wont-start--wont-converge).

---

## Related

- [Running Simulations](running-simulations.md)
- [Advanced Simulations](../manuals/advanced-simulations.md)
- [Troubleshooting](troubleshooting.md)
