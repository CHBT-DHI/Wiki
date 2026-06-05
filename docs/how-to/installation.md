---
tags:
  - how-to
  - installation
---

# Installation & Licensing

**Summary:** System requirements, installation procedure, and licence configuration for WEST.

**Source:** WEST User Guide, Chapter 6 (Technical Specifications).

---

## System requirements

| Component | Minimum | Recommended |
|---|---|---|
| Operating system | Windows 10 (64-bit) | Windows 11 (64-bit) |
| RAM | 8 GB | 16 GB or more |
| Disk space | 5 GB free | 10 GB free (for projects and simulation output) |
| .NET Framework | 4.8 | 4.8 (included with Windows 10/11; install manually on older images) |
| Display | 1280 × 768 | 1920 × 1080 or higher |
| Internet | Required for licence activation and updates | — |

WEST is a 64-bit application. 32-bit Windows is not supported. Administrator rights are required during installation and licence activation.

---

## Installation

1. **Download the installer** from the DHI customer portal or the MIKE by DHI download page. The installer is a single `.exe` file (e.g. `WEST_2026_Setup.exe`).

2. **Run as Administrator.** Right-click the installer file and choose **Run as administrator**. If User Account Control (UAC) prompts appear, confirm them.

3. **Accept the licence agreement.** Read the DHI Software Licence Agreement and click **I Agree** to proceed.

4. **Choose the installation path.** The default is `C:\Program Files\DHI\WEST\2026`. Accept the default or browse to a preferred location. Avoid paths with spaces or non-ASCII characters if possible.

5. **Install redistributable components.** The installer will detect and install any missing prerequisites (Microsoft Visual C++ Redistributables, .NET 4.8). Accept any prompts. If a reboot is required to complete a prerequisite installation, restart the computer and re-run the WEST installer afterwards.

6. **Complete the installation.** Click **Install** and wait for the progress bar to finish. Click **Finish** to close the installer.

7. **First launch.** Start WEST from the Start menu or desktop shortcut. On first launch WEST will check for a valid licence and open the Licence Manager if none is found.

---

## Licence configuration

WEST supports three licence modes. The active mode is configured in **Help → Licence Manager** within the application.

### Node-locked licence (USB dongle — CodeMeter)

A hardware USB dongle (CodeMeter CmStick, supplied by DHI) carries the licence. The CodeMeter runtime software must be installed on the PC; the WEST installer includes this automatically.

- Insert the dongle before starting WEST.
- The Licence Manager will detect the dongle automatically; no further configuration is needed.
- If the dongle is removed while WEST is running, a grace period of a few minutes applies before WEST becomes read-only.
- If CodeMeter is not detected, download and install the latest CodeMeter Runtime from Wibu-Systems (https://www.wibu.com) and reboot.

### Floating licence (network licence server)

A floating licence server runs CodeMeter Network Server on a central machine and issues licences to individual client PCs on the same network.

1. On the **licence server machine**, install CodeMeter Network Server and import the DHI licence container file (`.WibuCmRaU`) provided by DHI.
2. On each **client PC**, install the CodeMeter Runtime (included with the WEST installer).
3. In the WEST Licence Manager on the client, click **Configure Server** and enter the hostname or IP address of the licence server (e.g. `licserver.mycompany.local` or `192.168.1.10`).
4. The client will check out a licence token from the server when WEST starts and return it when WEST closes.
5. The number of simultaneous users is limited by the number of tokens purchased.

### Online soft-licence (no dongle)

DHI offers an online activation model for some licence types. No physical dongle is required.

1. In the Licence Manager, choose **Activate Online Licence** and enter the activation code provided by DHI.
2. WEST contacts the DHI licence server over HTTPS (port 443) to validate and bind the licence to the machine's hardware fingerprint.
3. An internet connection is required periodically for licence renewal (typically every 30 days). Offline grace periods are available; contact DHI support for details.
4. To move the licence to a different PC, first **deactivate** the licence in the Licence Manager on the original machine, then activate it on the new machine.

---

## Visual Studio Build Tools (required for Model Editor / code generation)

The WEST Model Editor compiles Modelica models into executable code using the Microsoft C++ toolchain. The **Visual Studio Build Tools** must be installed separately from WEST; they are not bundled with the WEST installer.

### Installation steps

1. Download **Visual Studio Build Tools** from Microsoft:  
   https://visualstudio.microsoft.com/downloads/ → scroll to **Tools for Visual Studio** → **Build Tools for Visual Studio 2022** (or the version matching your WEST release notes).

2. Run the Build Tools installer (`vs_BuildTools.exe`) as Administrator.

3. In the **Workloads** tab, select **Desktop development with C++**. Ensure the following individual components are included (they are selected by default with the workload):
   - MSVC v143 (or v142) — C++ x64/x86 build tools
   - Windows 10 SDK (or Windows 11 SDK)
   - C++ CMake tools (optional but recommended)

4. Click **Install** and wait for the download and installation to complete (approximately 3–5 GB).

5. **Restart WEST** after the Build Tools installation. WEST detects the compiler automatically via the system PATH; no manual configuration is required in WEST settings.

### Verification

Open the **Model Editor** in WEST and attempt to compile a model. If the compiler is found correctly, compilation proceeds without error. If WEST reports "C++ compiler not found", check that the Build Tools installation completed successfully and that the MSVC `bin` directory is on the system PATH (the Build Tools installer sets this automatically).

---

## Related

- [Getting Started](../getting-started/quick-start.md)
- [Model Editor](../west-tools/model-editor.md)
