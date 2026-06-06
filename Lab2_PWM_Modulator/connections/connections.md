# Lab 2 — IC Pin Connections

## Reuse from Lab 1 (keep intact)
- MCP6004 IC#1 pin1 = VRAMP (~1Vpp triangle, 5.3kHz, centered at 2.5V)
- VCM node = 2.5V
- +5V and GND rails

---

## ADALM1000 Settings

| Parameter | Value |
|-----------|-------|
| CHA mode | SVMI sine wave |
| Frequency | 312.5 Hz |
| Min voltage | 2.0V |
| Max voltage | 3.0V |
| CHB | probe only |

---

## MCP6004 IC#2 — Differential Converter

```
        ┌──────────┐
 OUTA ──┤1        14├── OUTD
 INA-───┤2        13├── IND-
 INA+───┤3        12├── IND+
  VDD───┤4        11├── VSS(GND)
 INB+───┤5        10├── INC+
 INB-───┤6         9├── INC-
 OUTB───┤7         8├── OUTC
        └──────────┘
```

### Power
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 4 | +5V rail | 5V |
| pin 11 | GND rail | 0V |

### Op-amp A — Buffer (Vin_a+) pins 1,2,3
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 3 (IN+A) | ADALM CHA | sine input 312.5Hz |
| pin 2 (IN−A) | pin 1 (OUTA) | direct wire — unity buffer |
| pin 1 (OUTA) | LM339 IC#2 pin7 | Vin_a+ output |

**pin 1 = Vin_a+ = buffered sine, same phase**

### Op-amp B — Inverter (Vin_a−) pins 5,6,7
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 5 (IN+B) | VCM node | 2.5V reference |
| pin 6 (IN−B) | ADALM CHA through 10kΩ | R_input |
| pin 6 (IN−B) | pin 7 (OUTB) through 10kΩ | R_feedback |
| pin 7 (OUTB) | LM339 IC#2 pin9 | Vin_a− output |

**pin 7 = Vin_a− = inverted sine, 180° out of phase**

---

## LM339 IC#2 — PWM Comparators

```
        ┌──────────┐
 OUT1───┤1        14├── OUT4  ← VPWM_N
 IN1-───┤2        13├── IN4-
 IN1+───┤3        12├── IN4+
  VCC───┤4        11├── GND
 IN2+───┤5        10├── IN3+  ← Vin_a−
 IN2-───┤6         9├── IN3-
 OUT2───┤7         8├── OUT3
        └──────────┘
```

### Power
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 4 | +5V rail | 5V |
| pin 11 | GND rail | 0V |

### Comparator 2 — VPWM_P (pins 5,6,7)
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 6 (IN2−) | MCP6004 IC#1 pin1 | VRAMP triangle wave |
| pin 5 (IN2+) | MCP6004 IC#2 pin1 | Vin_a+ buffered sine |
| pin 7 (OUT2) | +5V through 10kΩ pull-up | open-collector |

**pin 7 = VPWM_P** — duty cycle follows Vin_a+

### Comparator 3 — VPWM_N (pins 8,9,10)
| Pin | Connect To | Signal |
|-----|-----------|--------|
| pin 9 (IN3−) | MCP6004 IC#1 pin1 | VRAMP triangle wave |
| pin 10 (IN3+) | MCP6004 IC#2 pin7 | Vin_a− inverted sine |
| pin 8 (OUT3) | +5V through 10kΩ pull-up | open-collector |

**pin 8 = VPWM_N** — complementary duty (1-D)

---

## Probe Points

| CHB to | Expected |
|--------|---------|
| MCP6004 IC#2 pin1 | Sine 312.5Hz, ~1Vpp, centered 2.5V |
| MCP6004 IC#2 pin7 | Sine 312.5Hz, ~1Vpp, 180° inverted |
| LM339 IC#2 pin7 | PWM 5kHz, varying duty cycle |
| LM339 IC#2 pin8 | PWM 5kHz, complementary duty |
| MCP6004 IC#1 pin1 | Triangle wave 5.3kHz (from Lab 1) |

---

## Verification

1. Vin_a+ and Vin_a− must be 180° out of phase ✓
2. VPWM_P duty + VPWM_N duty = 100% at all times ✓
3. Add RC filter (10kΩ + 10nF) at VPWM_P → should show sine shape ✓
