# BS Analog Systems Lab — IIT Madras
### Class-D Audio Amplifier System

---

## Quick Navigation

| Lab | Topic | Status | Connections | LTSpice |
|-----|-------|--------|-------------|---------|
| [Lab 1](./Lab1_Ramp_Generator/README.md) | Ramp Generator | ✅ Done | [IC Connections](./Lab1_Ramp_Generator/connections/connections.md) | [.asc file](./Lab1_Ramp_Generator/simulation/Exp1.asc) |
| [Lab 2](./Lab2_PWM_Modulator/README.md) | PWM Modulator | ✅ Done | [IC Connections](./Lab2_PWM_Modulator/connections/connections.md) | [.asc file](./Lab2_PWM_Modulator/simulation/Exp2.asc) |
| [Lab 3](./Lab3_HBridge/README.md) | H-Bridge Driver | ✅ Done | [IC Connections](./Lab3_HBridge/connections/connections.md) | [.asc file](./Lab3_HBridge/simulation/Exp3.asc) |
| [Lab 4](./Lab4_Bandpass_Filter/README.md) | Bandpass Filter | ✅ Done | [IC Connections](./Lab4_Bandpass_Filter/connections/connections.md) | [.asc file](./Lab4_Bandpass_Filter/simulation/Exp4.asc) |
| [Lab 5](./Lab5_Adder/README.md) | Adder | ✅ Done | [IC Connections](./Lab5_Adder/connections/connections.md) | [.asc file](./Lab5_Adder/simulation/Exp5.asc) |

---

## System Block Diagram

```
Audio Input (CHA 156.25Hz or 625Hz)
         │
         ▼
  ┌──────────────┐    ┌──────────────┐
  │   BPF-1      │    │   BPF-2      │
  │  fo=156.25Hz │    │  fo=625Hz    │
  │  Q=10, Ao=1  │    │  Q=10, Ao=1  │
  └──────┬───────┘    └──────┬───────┘
         └──────────┬─────────┘
                    ▼
             ┌─────────────┐
             │    ADDER    │
             │  Vout=V1+V2 │
             └──────┬──────┘
                    │ Vin_a
         ┌──────────▼──────────┐
         │   CLASS-D AMP       │◄── RAMP GENERATOR 5kHz
         │  PWM + H-Bridge     │
         └──────────┬──────────┘
                    ▼
                 Speaker (32Ω)
```

---

## Key Specifications

| Parameter | Value |
|-----------|-------|
| VDD | 5V |
| VCM | 2.5V |
| Ramp frequency | ~5.3 kHz |
| Ramp amplitude | ~1 Vpp |
| BPF-1 fo | 156.25 Hz |
| BPF-2 fo | 625 Hz |
| Q factor | 10 |
| PWM frequency | 5 kHz |
| Speaker load | 32Ω |

---

## ICs Used

| IC | Part | Labs |
|----|------|------|
| MCP6004 | Quad op-amp, 1.8V-5.5V, 1MHz | Labs 1,2,4,5 |
| LM339 | Quad comparator, open-collector | Labs 1,2,3 |
| MC14069 | CMOS Hex Inverter | Labs 2,3 |
| SN74AHC00N | Quad NAND Gate | Lab 3 |

---

## How to Open LTSpice Files

1. Download LTSpice from [analog.com](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)
2. Download `LM339.sub` from [Lab1/simulation/](./Lab1_Ramp_Generator/simulation/)
3. Copy `LM339.sub` to your LTSpice lib/sub folder
4. Open any `.asc` file
5. Press **Run** (green play button)
6. Click on wires to probe signals

---

## Tools Used

- **LTSpice** — pre-lab simulation
- **ADALM1000 + PixelPulse2** — hardware measurement
- **Breadboard** — circuit implementation
