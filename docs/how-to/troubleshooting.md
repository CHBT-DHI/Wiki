---
tags:
  - how-to
  - troubleshooting
---

# Troubleshooting Common Problems

---

## Simulation won't start / won't converge

- **Check all terminals are connected**: unconnected terminals cause mass-balance errors that prevent convergence. Right-click the layout and choose **Validate Layout** to highlight unconnected terminals.
- **Check influent flow is non-zero**: open the Influent block properties and verify the flow rate parameter is set.
- **Run a short dynamic first**: if steady-state fails, try a 1-day dynamic run to reach a plausible state, then switch back to steady-state using that as the initial condition.
- **Increase max iterations**: Simulation → Experiment Settings → Solver → increase maximum iterations from 500 to 2000.
- **"Integrator failed"**: check that all tank volumes are greater than zero, all flow rates are positive, and that splitter fractions sum exactly to 1.0. A divide-by-zero in any splitter will immediately abort the integrator.
- **Steady-state doesn't converge**: reduce the solver step size and increase the maximum number of iterations in **Project | Simulation Properties → Solver**. If the model contains a recycle loop, ensure a **Loop Breaker** block is placed in the recycle stream — without one, the Newton-Raphson solver has no starting point for the loop.
- **"NaN in state variables"**: usually caused by negative concentrations propagating through the model. Check that influent fractionation components (S_I, S_S, X_I, X_S, X_BH) sum to the correct total COD and total TSS. Check that all initial conditions are non-negative values.

---

## Licence errors

- **"No licence found"**: ensure the USB dongle is inserted (try a different port) or that the network licence server address is correct (Help → Licence Manager).
- **"Licence expired"**: contact DHI support at support@dhigroup.com to renew. Apply the new licence file via Help → Licence Manager → Update.
- **"All seats in use"**: another user on your network is using the last available licence seat. Wait or contact your licence administrator to add seats.
- **"No valid licence found"**: verify the USB hardware dongle is inserted *before* starting WEST. Open Windows Services and confirm that **CmService** (CodeMeter Runtime) is in the Running state. If the service is stopped, start it and then re-launch WEST.
- **"Licence server unreachable"**: open the **Licence Manager** (Help → Licence Manager) and check that the server hostname or IP address is correct. Confirm that your firewall allows outbound and inbound traffic on **port 22350** (the default CodeMeter port). Run `ping <server>` from a command prompt to verify basic network connectivity.
- **"Licence has expired"**: contact DHI support to arrange renewal. While waiting, use **Help → Licence Manager** to view the exact expiry date and the features covered by the licence.

---

## Influent and fractionation issues

- **Negative concentrations in bioreactor**: usually caused by incorrect fractionation where component fractions sum to more than 1. Check that f_SS + f_XI + f_XS + f_SI ≤ 1.0 in the influent characterisation.
- **Influent file not found at runtime**: ensure the path in the Data Input block's File parameter is an absolute path or a path relative to the project file location.
- **Wrong units in data file**: WEST expects flow in m³/d and concentrations in g/m³ (= mg/L) by default. Check the unit settings in the Data Editor header.
- **COD fractions don't balance**: the sum S_I + S_S + X_I + X_S + X_BH (at time zero) must equal the measured total COD. Use the **Fractionation check** button in the influent fractionation dialog to run the built-in balance check and identify which fraction is out of range.
- **"Negative concentration at t=0"**: influent component values are internally inconsistent. Delete the current fractionation and regenerate it from the bulk parameters (total COD, total BOD5, VSS, TKN, TP) using the **Regenerate** button in the fractionation dialog.
- **TKN error**: S_NH + S_ND + X_ND must equal the measured total TKN. Open the fractionation dialog, go to the **Nitrogen** tab, and verify that the nitrogen fractions are correctly specified before running.

---

## Results and plots

- **Results panel is empty after run**: the variables may not be in the output list. Go to Simulation → Output Configuration and add the variables you need, then re-run.
- **Plot shows flat line at zero**: the variable was added to the output list but the block is not receiving flow (disconnected upstream). Check the layout connectivity.
- **Can't find a variable in the tree**: use the search box at the top of the variable tree, or check the block name — blocks are listed by their layout name, not their model type.
- **Plot shows a flat line**: the variable may not be included in the output configuration. Right-click the block → **Output Properties** (or open the **Output** tab of Simulation Properties) and add the variable to the recorded output list, then re-run.
- **"No data to export"**: a simulation must be completed before data can be exported. Check that the run has a green tick in the **Runs** tab, indicating it finished without errors.
- **Results file not found**: open the **Data Output** block and verify that the file path in the **File name** field points to a folder that already exists. WEST will not create missing directories automatically.

---

## Controllers

- **Controller output not changing**: verify the sensor block is connected to the controller's measurement input, and the controller output is connected to the actuator's control input.
- **Oscillating control response**: the PID gains may be too aggressive. Reduce Kp or increase Ti. Use the auto-tune function if available for the controller type.
- **Controller saturates at min/max**: check that the output limits in the controller parameters match the actuator's expected input range.
- **Controller output oscillates wildly**: the proportional gain Kp is too high. Reduce Kp by a factor of 10 and re-run. Gradually increase until the response is fast but stable, then add integral action. Also check whether sensor noise is amplifying the derivative term — if so, reduce or disable Kd.
- **DO stays at zero despite controller being active**: verify that the **Interface Link** connecting the controller output maps to the correct aeration parameter on the target block (`Q_air` or `KLa`). A common mistake is linking to a parameter rather than a manipulated variable — see [Parameters vs manipulated variables](../manuals/controllers.md#parameters-vs-manipulated-variables).
- **Setpoint never reached**: check the **output limits** (min/max) on the PID block. If the upper output limit is set too low, the controller is clamped and cannot supply enough signal to reach the setpoint. Increase the upper limit and also increase the integral gain Ki to eliminate steady-state error.

---

## Model compilation errors (Model Editor)

- **"Unknown identifier"**: a variable or parameter name is used before it is declared. Check spelling and ensure all variables are listed in the variable declaration section.
- **"Type mismatch"**: a flow variable is being assigned to a concentration variable or vice versa. Check that connected terminals are of the same type.
- **"Circular dependency"**: two calculator variables reference each other. Break the loop by introducing an intermediate state variable.
- **"C++ compiler not found"**: Visual Studio Build Tools are not installed or are not on the system PATH. Follow the steps in the [Installation guide](installation.md) to install the correct compiler version and configure the PATH environment variable.
- **"Undefined symbol in model"**: one or more state variable names in the Modelica code do not match the connector definition exactly. Variable names are case-sensitive — compare the code carefully against the connector and correct any mismatch.
- **Model compiles but results are wrong**: open the **Gujer matrix editor** and verify the signs of each stoichiometric coefficient in the process matrix. A sign error will cause a component to be consumed instead of produced (or vice versa) without triggering a compilation error.

---

## Performance

- **Run takes more than 10 minutes for a simple model**: check the communication interval — very small values (< 0.001 d) generate huge output files and slow I/O. Increase to 0.01–0.1 d for most purposes.
- **Memory usage keeps growing**: a very long dynamic run (> 1 year) with many output variables can fill RAM. Reduce the output variable list or split the run into shorter segments.
- **Simulation is very slow**: simplify the secondary clarifier model — replace a layered Takacs clarifier with a **Point** clarifier for calibration runs. Reduce the number of compartments in the bioreactor. Use a steady-state run to initialise the dynamic simulation so the integrator starts from a near-equilibrium state rather than from zero.
- **WEST freezes during a large model run**: check available system RAM (WEST can require several GB for large models with high output frequency). Reduce the **communication interval** in the Output tab — writing results every 1 hour instead of every minute reduces memory and disk I/O by 60×.

---

## Installation

- **Installer hangs at "Installing components"**: disable antivirus real-time scanning and run the installer as Administrator.
- **WEST not appearing in Start menu**: re-run the installer as Administrator to register shortcuts.
- **Missing Visual C++ runtime error**: install the Microsoft Visual C++ Redistributable (x64) for the year matching your WEST version.
- **Installer fails silently**: right-click the installer executable and choose **Run as administrator**. If it still fails, open **Windows Event Viewer → Windows Logs → Application** and look for an MSI or installer error entry with the exact failure reason.
- **.NET error on launch**: WEST requires Microsoft .NET Framework 4.8. Download and install the **.NET Framework 4.8 Redistributable** from the Microsoft website, then reboot before launching WEST again.
- **WEST opens but shows a blank white screen**: this is usually a graphics driver conflict. Update your GPU driver to the latest version. If the problem persists, open **WEST Options** and disable hardware acceleration (software rendering mode).

---

## Related

- [Installation guide](installation.md)
- [Running Simulations](running-simulations.md)
- [Controllers](controllers.md)
- [Results and Output](results-and-output.md)
