# Lab 5 — Summing Amplifier (Adder)

## Objective
Sum the two bandpass filtered signals (Vout_bpf1 and Vout_bpf2) into a single output.

---

## Design Equation

```
Vout_adder = -(Vin1 + Vin2) + 2×VCM
AC component: Vout(ac) = -(Vin1(ac) + Vin2(ac))
Gain = -1 (inverting unity sum)
```

---

## Component Values

| Component | Value | Color Code | Qty |
|-----------|-------|------------|-----|
| R_in1 | 10kΩ | Brown-Black-Orange | 1 |
| R_in2 | 10kΩ | Brown-Black-Orange | 1 |
| R_fb | 10kΩ | Brown-Black-Orange | 1 |

**Total: just 3 resistors + 1 op-amp section**

---

## IC Pin Connections — Op-amp A: pins 1,2,3

### Uses same MCP6004 as Lab 4 (op-amp A pins 1,2,3 are free)

```
        ┌──────────┐
 OUTA ──┤1        14├── (Lab4 BPF-2 out)
 INA-───┤2        13├── (Lab4)
 INA+───┤3        12├── (Lab4)
  VDD───┤4        11├── VSS
        ...
        └──────────┘
```

| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 4 | +5V | (already connected) |
| pin 11 | GND | (already connected) |
| pin 3 (IN+A) | VCM node | 2.5V bias |
| pin 2 (IN−A) | Lab4 pin8 through 10kΩ | Vin1 = Vout_bpf1 |
| pin 2 (IN−A) | Lab4 pin14 through 10kΩ | Vin2 = Vout_bpf2 |
| pin 1 (OUTA) | pin2 through 10kΩ | R_feedback |

**OUTPUT = pin 1 = Vout_adder**

---

## Wiring Diagram

```
Lab4 pin8  (bpf1) ──10kΩ──┐
                            ├──► pin2 (IN−A) ──10kΩ── pin1 (OUTA)
Lab4 pin14 (bpf2) ──10kΩ──┘

pin3 (IN+A) ──► VCM (2.5V)

OUTPUT = pin1
```

---

## ADALM Settings

| Parameter | Value |
|-----------|-------|
| CHA | sine wave |
| Frequency | 132Hz or 620Hz |
| Min | 2.1V |
| Max | 2.9V |
| CHB | probe pin1 |

---

## Test Sequence

| Step | CHA freq | CHB to | Expected |
|------|---------|--------|---------|
| 1 | 132Hz | pin 1 | sine ~0.8Vpp (bpf1 passes) ✅ |
| 2 | 620Hz | pin 1 | sine ~0.8Vpp (bpf2 passes) ✅ |
| 3 | 400Hz | pin 1 | flat/small (both reject) ✅ |

When both frequencies are present simultaneously, pin1 shows both combined.

---

## Files

- [IC Connections](./connections/connections.md) ← this file
- [LTSpice simulation](./simulation/Exp5.asc)
