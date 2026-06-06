# Lab 1 — Ramp Generator

## Objective
Generate a triangle wave (VRAMP) at ~5kHz and square wave (VSQR) using an op-amp integrator and Schmitt trigger comparator.

---

## Design Equations

```
Frequency:  Fsw = R3 / (4 × R2 × R1 × C1) = 5.34 kHz
Amplitude:  VM  = 2 × (R2/R3) × VCM       = 1.06 Vpp
```

---

## Component Values

| Component | Value | Color Code |
|-----------|-------|------------|
| R1 | 22kΩ | Red-Red-Orange |
| R2 | 10kΩ | Brown-Black-Orange |
| R3 | 47kΩ | Yellow-Violet-Orange |
| C1 | 10nF | marked 103 |
| R_pullup | 10kΩ | Brown-Black-Orange |
| R_VCM × 2 | 10kΩ each | Brown-Black-Orange |

---

## Expected Output

| Signal | Value |
|--------|-------|
| VRAMP frequency | ~5.3 kHz |
| VRAMP amplitude | ~1.06 Vpp |
| VRAMP center | 2.5V (VCM) |
| VSQR | 0V to 5V square wave |

---

## Files

- [IC Connections](./connections/connections.md) — exact pin-by-pin wiring
- [LTSpice simulation](./simulation/Exp1.asc) — open in LTSpice
- [LTSpice setup guide](./simulation/LTSPICE_SETUP.md)
