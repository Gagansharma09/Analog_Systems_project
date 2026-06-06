# Lab 2 — PWM Modulator

## Objective
Convert single-ended audio input to differential pair (Vin_a+, Vin_a−) and generate complementary PWM signals (VPWM_P, VPWM_N).

---

## Design Equations

```
Vin_a+ = Vin_a(ac) + VCM   (buffered)
Vin_a− = −Vin_a(ac) + VCM  (inverted, R1=R2)

PWM_P: HIGH when Vin_a+ > VRAMP
PWM_N: HIGH when Vin_a− > VRAMP (complementary)
```

---

## Component Values

| Component | Value | Color Code |
|-----------|-------|------------|
| R_in (inverter) | 10kΩ | Brown-Black-Orange |
| R_fb (inverter) | 10kΩ | Brown-Black-Orange |
| R_pullup × 2 | 10kΩ each | Brown-Black-Orange |
| Cin (optional) | 10µF | electrolytic |

---

## ICs Required

| IC | Purpose |
|----|---------|
| MCP6004 IC#1 | Already from Lab 1 (VRAMP source) |
| MCP6004 IC#2 | Buffer + Inverter for differential converter |
| LM339 IC#1 | Already from Lab 1 (VRAMP source) |
| LM339 IC#2 | PWM comparators |

---

## Expected Output

| Signal | Value |
|--------|-------|
| Vin_a+ | Sine, same amplitude as input, 0° phase |
| Vin_a− | Sine, same amplitude as input, 180° phase |
| VPWM_P | PWM 5kHz, duty tracks Vin_a+ |
| VPWM_N | PWM 5kHz, inverted duty of VPWM_P |

---

## Files

- [IC Connections](./connections/connections.md)
- [LTSpice simulation](./simulation/Exp2.asc)
- [LTSpice setup](./simulation/LTSPICE_SETUP.md)
