# Lab 3 — H-Bridge Driver & Class-D Amplifier

## Objective
Build H-bridge output stage using BJT transistors and CMOS inverter buffers to drive a 32Ω speaker load.

---

## Components Required

| Component | Value | Purpose |
|-----------|-------|---------|
| MC14069 / CD4069 | Hex inverter | Buffer for BJT base drive |
| SN74AHC00N | Quad NAND | Non-overlapping clock |
| NPN BJT | 2N2222 or similar | Low-side switch |
| PNP BJT | 2N2907 or similar | High-side switch |
| RB | 2.2kΩ Red-Red-Red | Base current limiting |
| RL (load) | 32Ω | Speaker or resistor |

---

## Circuit Structure

```
VPWM_P ──► Non-overlap gen ──► 2× inverter buffers ──► RB ──► Qp (PNP)  ──► VOUT_P
                              └──► 2× inverter buffers ──► RB ──► Qn (NPN) ──┘

VPWM_N ──► Non-overlap gen ──► 2× inverter buffers ──► RB ──► Qp (PNP)  ──► VOUT_N
                              └──► 2× inverter buffers ──► RB ──► Qn (NPN) ──┘

VOUT_P ──── 32Ω Speaker ──── VOUT_N
```

---

## IC Pin Connections

### MC14069 Hex Inverter (CD4069)
```
        ┌──────────┐
  IN1───┤1        14├── VDD (+5V)
 OUT1───┤2        13├── IN6
  IN2───┤3        12├── OUT6
 OUT2───┤4        11├── IN5
  IN3───┤5        10├── OUT5
 OUT3───┤6         9├── IN4
  GND───┤7         8├── OUT4
        └──────────┘
```

### SN74AHC00N NAND Gates
```
        ┌──────────┐
  1A────┤1        14├── VCC (+5V)
  1B────┤2        13├── 4B
  1Y────┤3        12├── 4A
  2A────┤4        11├── 4Y
  2B────┤5        10├── 3B
  2Y────┤6         9├── 3A
  GND───┤7         8├── 3Y
        └──────────┘
```

### Non-overlap Generator (using NAND gates)
```
VPWM ──► NAND1A input
         NAND1 output ──RC delay──► NAND2A input
         NAND2 output ──► Q  (drives one transistor)
         NAND1 output ──► Q' (drives other transistor)
```

---

## Test Sequence

| Step | Input | Probe | Expected |
|------|-------|-------|---------|
| 1 | VPWM_P from Lab2 | VOUT_P | PWM 0-5V |
| 2 | VPWM_N from Lab2 | VOUT_N | Complementary PWM |
| 3 | Add 32Ω load | differential | Switching output |
| 4 | Add RC filter | filtered output | Sine wave shape |

---

## Files

- [IC Connections](./connections/connections.md)
- [LTSpice simulation](./simulation/Exp3.asc)
