# BS Analog Systems project — IIT Madras

**Course:** EE2003 · Analog Systems Lab  
**Project:** End-to-end Class-D Audio Amplifier — from signal filtering to speaker output  
**Tools:** LTSpice · ADALM1000 · Breadboard  

---

## What This Is

A 5-lab series building a complete Class-D audio amplifier from scratch:

- Two bandpass filters isolate audio frequency bands
- An op-amp adder combines the filtered signals  
- A ramp generator produces the PWM carrier
- A comparator + H-bridge drives a 32Ω speaker

Every lab includes a pre-simulation in LTSpice and hardware verification on breadboard using ADALM1000.

---

## System Block Diagram

```
┌─────────────────────────────────────────────────────┐
│                   SIGNAL CHAIN                      │
│                                                     │
│  CHA Input ──┬──► BPF-1 (156 Hz, Q=10) ──┐         │
│              │                            ├──► ADDER ──► Class-D Amp ──► 32Ω
│              └──► BPF-2 (625 Hz, Q=10) ──┘         │
│                                                     │
│  Ramp Generator (5 kHz) ──────────────────────────► │
└─────────────────────────────────────────────────────┘
```

---

## Labs

| Lab | Module | What It Does | Connections | Simulation |
|-----|--------|-------------|-------------|------------|
| 1 | Ramp Generator | Generates 5 kHz triangular wave for PWM carrier | [Connections](./Lab1_Ramp_Generator/connections/connections.md) | [Exp1.asc](./Lab1_Ramp_Generator/simulation/Exp1.asc) |
| 2 | PWM Modulator | Compares audio signal with ramp to generate PWM | [Connections](./Lab2_PWM_Modulator/connections/connections.md) | [Exp2.asc](./Lab2_PWM_Modulator/simulation/Exp2.asc) |
| 3 | H-Bridge Driver | Drives speaker with complementary switching | [Connections](./Lab3_HBridge/connections/connections.md) | [Exp3.asc](./Lab3_HBridge/simulation/Exp3.asc) |
| 4 | Bandpass Filter | MFB BPF at 156 Hz and 625 Hz, Q=10 | [Connections](./Lab4_Bandpass_Filter/connections/connections.md) | [Exp4.asc](./Lab4_Bandpass_Filter/simulation/Exp4.asc) |
| 5 | Adder | Sums BPF outputs into single audio signal | [Connections](./Lab5_Adder/connections/connections.md) | [Exp5.asc](./Lab5_Adder/simulation/Exp5.asc) |

---

## Specifications

| Parameter | Value |
|-----------|-------|
| Supply voltage | 5V |
| Common-mode voltage (VCM) | 2.5V |
| BPF-1 centre frequency | 156.25 Hz |
| BPF-2 centre frequency | 625 Hz |
| Q factor | 10 |
| Ramp frequency | ~5.3 kHz |
| Ramp amplitude | ~1 Vpp |
| PWM frequency | 5 kHz |
| Speaker load | 32Ω |

---

## ICs Used

| Part | Description | Labs |
|------|-------------|------|
| MCP6004 | Quad op-amp · 1.8–5.5V · 1MHz GBW | 1, 2, 4, 5 |
| LM339 | Quad comparator · open-collector output | 1, 2, 3 |
| MC14069 | CMOS hex inverter | 2, 3 |
| SN74AHC00N | Quad NAND gate | 3 |

---

## Running Simulations

1. Install [LTSpice](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)
2. Copy `LM339.sub` from `Lab1_Ramp_Generator/simulation/` into your LTSpice `lib/sub/` folder
3. Open any `.asc` file and press **Run**
4. Click any wire to probe that node

---

*IIT Madras · BS in Electronic Systems · Analog Systems Lab*| 1 | Ramp Generator | [Connections](./Lab1_Ramp_Generator/connections/connections.md) | [Exp1.asc](./Lab1_Ramp_Generator/simulation/Exp1.asc) |
| 2 | PWM Modulator | [Connections](./Lab2_PWM_Modulator/connections/connections.md) | [Exp2.asc](./Lab2_PWM_Modulator/simulation/Exp2.asc) |
| 3 | H-Bridge Driver | [Connections](./Lab3_HBridge/connections/connections.md) | [Exp3.asc](./Lab3_HBridge/simulation/Exp3.asc) |
| 4 | Bandpass Filter | [Connections](./Lab4_Bandpass_Filter/connections/connections.md) | [Exp4.asc](./Lab4_Bandpass_Filter/simulation/Exp4.asc) |
| 5 | Adder | [Connections](./Lab5_Adder/connections/connections.md) | [Exp5.asc](./Lab5_Adder/simulation/Exp5.asc) |

## Specifications

| Parameter | Value |
|-----------|-------|
| Supply voltage | 5V |
| Common-mode voltage | 2.5V |
| BPF-1 centre frequency | 156.25 Hz |
| BPF-2 centre frequency | 625 Hz |
| Q factor | 10 |
| Ramp frequency | ~5.3 kHz |
| Ramp amplitude | ~1 Vpp |
| PWM frequency | 5 kHz |
| Speaker load | 32Ω |

## ICs

| Part | Description | Used In |
|------|-------------|---------|
| MCP6004 | Quad op-amp · 1.8–5.5V · 1MHz GBW | Labs 1, 2, 4, 5 |
| LM339 | Quad comparator · open-collector | Labs 1, 2, 3 |
| MC14069 | CMOS hex inverter | Labs 2, 3 |
| SN74AHC00N | Quad NAND gate | Lab 3 |

## Tools

| Tool | Purpose |
|------|---------|
| LTSpice | Pre-lab simulation |
| ADALM1000 + PixelPulse2 | Hardware signal generation and measurement |
| Breadboard | Circuit implementation |

## Running Simulations

1. Install LTSpice from [analog.com](https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html)
2. Copy `LM339.sub` from `Lab1_Ramp_Generator/simulation/` into your LTSpice `lib/sub/` folder
3. Open any `.asc` file and press **Run**
4. Click any wire to probe that node
5. webpage for simulation :///Users/gagansharma/Downloads/analog_lab_interactive.html
````
