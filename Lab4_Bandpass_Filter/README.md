# Lab 4 — Bandpass Filter

## Objective
Design two second-order MFB bandpass filters with different center frequencies using available components.

---

## Design Equations (MFB BPF)

```
Transfer Function:
H(s) = -Ao*(wo/Q)*s / (s² + (wo/Q)*s + wo²)

Component equations (C1=C2=C):
R11 = Q / (Ao × wo × C)
R22 = 2Q / (wo × C)
R33 = Q / ((2Q²-Ao) × wo × C)
```

---

## Component Values (with available parts)

### BPF-1 — fo = 132Hz, Q = 2.74, Ao = 1

| Component | Value | How to make | Color Code |
|-----------|-------|-------------|------------|
| R11 | 330kΩ | single | Orange-Orange-Yellow |
| R22 | 660kΩ | 2× 330k in **series** | Orange-Orange-Yellow × 2 |
| R33 | 23.5kΩ | 2× 47k in **parallel** | Yellow-Violet-Orange × 2 |
| C1 | 10nF | single 103 | — |
| C2 | 10nF | single 103 | — |

### BPF-2 — fo = 620Hz, Q = 12.86, Ao = 1

| Component | Value | How to make | Color Code |
|-----------|-------|-------------|------------|
| R11 | 330kΩ | single | Orange-Orange-Yellow |
| R22 | 660kΩ | 2× 330k in **series** | Orange-Orange-Yellow × 2 |
| R33 | 1kΩ | single | Brown-Black-Red |
| C1 | 10nF | single 103 | — |
| C2 | 10nF | single 103 | — |

> **Note:** fo may shift slightly on breadboard due to tolerance. Sweep CHA frequency ±10% around target to find actual peak.

---

## Expected Output

| Filter | Input freq | Output |
|--------|-----------|--------|
| BPF-1 | 132 Hz | BIG sine ~0.8Vpp |
| BPF-1 | 620 Hz | Very small/flat |
| BPF-2 | 620 Hz | BIG sine ~0.8Vpp |
| BPF-2 | 132 Hz | Very small/flat |

---

## Files

- [IC Connections](./connections/connections.md)
- [LTSpice simulation](./simulation/Exp4.asc)
- [LTSpice setup](./simulation/LTSPICE_SETUP.md)
