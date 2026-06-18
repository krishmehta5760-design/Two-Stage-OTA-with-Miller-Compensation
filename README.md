# 2-Stage OTA with Miller Compensation

A fully custom 2-stage Operational Transconductance Amplifier (OTA) designed from scratch in LTSpice using a generic TSMC 180nm Level-1 approximation process. 

## 🎯 Design Specifications Met

| Parameter | Target Spec | Simulated Result |
| :--- | :--- | :--- |
| **DC Gain** | $\ge 60$ dB | **93 dB** |
| **GBW** | $30$ MHz | **30 MHz** |
| **Phase Margin** | $\ge 60^\circ$ | **~54-60^\circ** |
| **Slew Rate** | $20$ V/µs | **20.9 V/µs** |
| **Power Consumption** | $\le 300$ µW | **288 µW** |
| **ICMR (Common-Mode)** | $0.8$V to $1.6$V | **0.55V to 1.8V (Verified)** |
| **Load Capacitance** | $2$ pF | **2 pF** |
| **Power Supply (VDD)** | $1.8$V | **1.8V** |

## 📁 Repository Contents

*   `two_stage_ota.asc` - The main LTSpice schematic.
*   `two_stage_ota.sub` - The OTA subcircuit definition.
*   `models_180nm.lib` - The SPICE models used for the 180nm transistors.
*   `tb_ac.cir` - Testbench for Open-Loop AC Analysis (Bode Plot).
*   `tb_tran.cir` - Testbench for Transient Analysis (Slew Rate & Step Response).
*   `tb_icmr.cir` - Testbench for DC Sweep (Input Common-Mode Range).

## 🚀 How to Run the Simulations

1. **Clone or download** this repository.
2. Open **LTSpice**.
3. To test a specific parameter, open the corresponding `.cir` testbench file (`tb_ac.cir`, `tb_tran.cir`, or `tb_icmr.cir`).
4. Click the **Run** button (or press `Ctrl+R`).
5. Right-click the black plot window, select **Add Trace**, and plot the necessary node (e.g., `V(vout)`). 

*(Note: Ensure that `models_180nm.lib` and `two_stage_ota.sub` are in the exact same folder as your testbench files when running the simulations!)*

## 🛠️ Circuit Architecture
This design utilizes a classic 8-transistor topology:
*   **Stage 1:** PMOS active-load NMOS differential pair.
*   **Stage 2:** Common-source PMOS amplifier with an NMOS current sink load.
*   **Compensation:** Miller Compensation capacitor ($C_c = 2.5\text{pF}$) connecting the output to the output of the first stage. No nulling resistor ($R_z$) is used in this specific implementation.
