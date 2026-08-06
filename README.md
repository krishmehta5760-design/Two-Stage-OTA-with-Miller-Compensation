# 2-Stage OTA with Miller Compensation

A fully custom 2-stage Operational Transconductance Amplifier (OTA) designed from scratch in LTSpice using a generic TSMC 180nm Level-1 approximation process. 

## 🎯 Design Specifications Met

| Parameter | Target Spec | Simulated Result |
| :--- | :--- | :--- |
| **DC Gain** | $\ge80$ dB | **86 dB** |
| **GBW** | $30$ MHz | **30.9 MHz** |
| **Phase Margin** | $\ge 60^\circ$ | **67°** |
| **Slew Rate** | $25$ V/µs | **23 V/µs** |
| **Power Consumption** | $\le 400$ µW | **350 µW** |
| **ICMR (Common-Mode)** | 0.8V to 1.6V | **0.55V to 1.8V (Verified)** |
| **Load Capacitance** | $2$ pF | **2 pF** |
| **Power Supply (VDD)** | 1.8V | **1.8V** |

## 📁 Repository Contents

*   `two_stage_ota.asc` - The main LTSpice schematic.
*   `two_stage_ota.sub` - The OTA subcircuit definition.
*   `models_180nm.lib` - The SPICE models used for the 180nm transistors.

## LTSpice Schematic

<img width="1920" height="809" alt="image" src="https://github.com/user-attachments/assets/baa4ec9c-caef-4a7b-a18d-fb29bed9a597" />


## 🚀 How to Run the Simulations

1. **Clone or download** this repository.
2. Open **LTSpice**.
4. Click the **Run** button (or press `Ctrl+R`).
5. Right-click the black plot window, select **Add Trace**, and plot the necessary node (e.g., `V(vout)`). 

*(Note: Ensure that `models_180nm.lib` and `two_stage_ota.sub` are in the exact same folder as your testbench files when running the simulations!)*

## 🛠️ Circuit Architecture
This design utilizes a classic 8-transistor topology:
*   **Stage 1:** PMOS active-load NMOS differential pair.
*   **Stage 2:** Common-source PMOS amplifier with an NMOS current sink load.
*   **Compensation:** Miller Compensation capacitor ($C_c = 2.2\text{pF}$) connecting the output to the output of the first stage. No nulling resistor ($R_z$) is used in this specific implementation.
