# 2-Stage OTA with Miller Compensation — LTSpice Design

## Quick Start

1. Open any `tb_*.cir` file in LTSpice
2. Click **Run** (the running man icon) or press `Ctrl+R`
3. Right-click the plot area → **Add Trace** to view waveforms

## File Structure

```
two_stage_ota/
├── models_180nm.lib      ← MOSFET models (Level-1 approximation)
├── two_stage_ota.sub     ← OTA subcircuit definition (M1–M9, Cc)
├── tb_ac.cir             ← Open-loop AC analysis (gain, GBW, phase margin)
├── tb_tran.cir           ← Transient analysis (slew rate, settling time)
├── tb_icmr.cir           ← DC sweep for input common-mode range
└── README.md             ← This file
```

## Testbench Details

### tb_ac.cir — AC Analysis (Bode Plot)
- **What it measures:** Open-loop DC gain, GBW, phase margin
- **Technique:** Inductor-feedback trick (Lfb = 1 MH) provides DC bias while keeping the loop open at AC
- **How to plot:**
  - Gain: Add trace `Vdb(vout)` — magnitude in dB
  - Phase: Add trace `Vp(vout)` — phase in degrees
- **Check .meas results:** View menu → SPICE Error Log (`Ctrl+L`)

### tb_tran.cir — Transient (Slew Rate)
- **What it measures:** Slew rate (rise & fall), settling time
- **Configuration:** Unity-gain buffer with 400 mV step input
- **How to plot:** Add traces `V(inp)` and `V(vout)` to overlay input vs output

### tb_icmr.cir — ICMR (DC Sweep)
- **What it measures:** Input common-mode range
- **How to plot:**
  - Add trace `V(vout)` — output follows input in the valid ICMR range
  - Overlay `V(inp)` as the ideal 1:1 reference line
  - Where the two curves diverge = ICMR boundary

## Design Summary

| Spec            | Target   | Hand Calc |
|-----------------|----------|-----------|
| DC Gain         | ≥ 60 dB  | 91.0 dB   |
| GBW             | 30 MHz   | 30 MHz    |
| Phase Margin    | ≥ 60°    | ~60°      |
| Slew Rate       | 20 V/µs  | 20 V/µs   |
| ICMR            | 0.8–1.6V | 0.8–1.6V  |
| Power           | ≤ 300 µW | 279 µW    |
| Supply          | 1.8 V    | 1.8 V     |
| Load Cap        | 2 pF     | 2 pF      |

## Transistor Sizing

| Device  | Type | W (µm) | L (µm) | Role                    |
|---------|------|---------|---------|-------------------------|
| M1, M2  | NMOS | 17      | 1       | Differential pair       |
| M3, M4  | PMOS | 21      | 1       | Active load (mirror)    |
| M5      | NMOS | 10      | 1       | Tail current source     |
| M6      | PMOS | 314     | 1       | 2nd stage amplifier     |
| M7      | NMOS | 20      | 1       | 2nd stage load          |
| M8      | NMOS | 1       | 1       | Bias (NMOS diode)       |

| Component | Value  | Purpose                          |
|-----------|--------|----------------------------------|
| Cc        | 2.5 pF | Miller compensation capacitor    |

## Using Your Own TSMC 180nm Models

1. Replace the `.include models_180nm.lib` line in each testbench with:
   ```
   .include /path/to/your/tsmc180nm.lib
   ```
2. Update the model names in `two_stage_ota.sub`:
   - Change `nmos180` → your NMOS model name (e.g., `nch`)
   - Change `pmos180` → your PMOS model name (e.g., `pch`)
3. Re-run simulations — you may need to fine-tune W/L sizes

## Tuning Tips

- **Phase margin too low?** Increase (W/L)6 to boost gm6 — this pushes the RHP zero and p2 higher
- **GBW too low?** Decrease Cc (but check SR = I5/Cc)
- **Need more gain?** Increase L (e.g., L = 2 µm) to boost ro
- **Power too high?** Reduce I6; compensate by increasing (W/L)6 to maintain gm6
