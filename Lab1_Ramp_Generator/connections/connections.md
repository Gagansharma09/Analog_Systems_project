# Lab 1 — IC Pin Connections (VERIFIED WORKING)

## MCP6004 Pinout Reference
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
     (notch/dot at top-left)
```

## LM339 Pinout Reference
```
        ┌──────────┐
 1OUT───┤1        14├── 4OUT
 2OUT───┤2        13├── 4IN-
  VCC───┤3        12├── GND
 2IN-───┤4        11├── 4IN+
 2IN+───┤5        10├── 3IN-
 1IN-───┤6         9├── 3IN+
 1IN+───┤7         8├── 3OUT
        └──────────┘
     (notch/dot at top-left)
```

---

## ADALM1000 Power

| ADALM Pin | Connect To |
|-----------|-----------|
| +5V | +5V rail (red) |
| GND | GND rail (black) |
| CHA | NOT used — Lab 1 is self-oscillating |
| CHB | probe only |

---

## VCM Divider (generates 2.5V)

| From | To | Through |
|------|----|---------|
| +5V rail | VCM node | 10kΩ (Brown-Black-Orange) |
| VCM node | GND rail | 10kΩ (Brown-Black-Orange) |

**VCM node = 2.5V**
> Verify VCM = 2.5V BEFORE connecting to any IC pin

---

## MCP6004 IC#1 — Integrator (Op-amp A: pins 1,2,3)

| Pin | Function | Connect To | Signal |
|-----|---------|-----------|--------|
| pin 4 | VDD | +5V rail | 5V |
| pin 11 | GND | GND rail | 0V |
| pin 3 | IN+A | VCM node | 2.5V DC bias |
| pin 2 | IN−A | LM339 pin1 through 22kΩ (R1) | VSQR feedback |
| pin 1 | OUTA | pin2 through 10nF (C1) | integrator capacitor |
| pin 1 | OUTA | LM339 pin7 through 10kΩ (R2) | VRAMP to Schmitt |

**VRAMP = MCP6004 pin 1**
Expected: triangle wave, ~5.3kHz, ~1.4Vpp, centered at 2.5V

---

## LM339 IC#1 — Schmitt Trigger (Comparator 1: pins 1,6,7)

| Pin | Function | Connect To | Signal |
|-----|---------|-----------|--------|
| pin 3 | VCC | +5V rail | 5V |
| pin 12 | GND | GND rail | 0V |
| pin 6 | IN1− | VCM node | 2.5V reference |
| pin 7 | IN1+ | MCP6004 pin1 through 10kΩ (R2) | VRAMP input |
| pin 7 | IN1+ | LM339 pin1 through 47kΩ (R3) | hysteresis feedback |
| pin 1 | OUT1 | MCP6004 pin2 through 22kΩ (R1) | VSQR to integrator |
| pin 1 | OUT1 | +5V through 10kΩ | open-collector pull-up |

**VSQR = LM339 pin 1**
Expected: square wave, 0V to ~4.5V, ~5.3kHz

> Note: LM339 open-collector output high = ~4.5V (not 5V) — this is normal per datasheet

---

## Wiring Summary

```
+5V ──┬── MCP6004 pin4
      ├── LM339 pin3 (VCC)
      ├── 10kΩ ──► VCM node ──10kΩ──► GND
      └── 10kΩ ──► LM339 pin1 (pull-up)

VCM ──┬── MCP6004 pin3 (IN+A)
      └── LM339 pin6 (IN1−)

MCP6004 pin1 (VRAMP)
      ├── 10nF ──► MCP6004 pin2 (integrator cap)
      └── 10kΩ ──► LM339 pin7 (IN1+)

LM339 pin1 (VSQR)
      ├── 22kΩ ──► MCP6004 pin2 (IN−A)
      └── 47kΩ ──► LM339 pin7 (IN1+) hysteresis

GND ──┬── MCP6004 pin11
      └── LM339 pin12
```

---

## Probe Points

| CHB to | Expected |
|--------|---------|
| MCP6004 pin 1 | Triangle wave ~1.4Vpp, centered 2.5V, ~5.3kHz |
| LM339 pin 1 | Square wave 0V to ~4.5V, ~5.3kHz |

---

## ADALM PixelPulse2 Settings

| Parameter | Value |
|-----------|-------|
| CHA | NOT used |
| CHB | Probe only |
| Timebase | 200µs/div |

---

## Component Values

| Component | Value | Color Code | Purpose |
|-----------|-------|------------|---------|
| R1 | 22kΩ | Red-Red-Orange | Integrator input |
| R2 | 10kΩ | Brown-Black-Orange | VRAMP to Schmitt IN+ |
| R3 | 47kΩ | Yellow-Violet-Orange | Hysteresis feedback |
| C1 | 10nF | marked 103 | Integrator capacitor |
| R_pullup | 10kΩ | Brown-Black-Orange | LM339 open-collector |
| R_VCM×2 | 10kΩ each | Brown-Black-Orange | VCM divider |

---

## Pre-build Checklist

| Check | ✓ |
|-------|---|
| MCP6004 notch facing correct direction | ☐ |
| LM339 notch facing correct direction | ☐ |
| VCM = 2.5V verified before connecting ICs | ☐ |
| 10nF between MCP6004 pin1 and pin2 | ☐ |
| 10kΩ pull-up from LM339 pin1 to +5V | ☐ |
| All GNDs common (ADALM + MCP6004 + LM339) | ☐ |
| pin3 of MCP6004 has ONLY VCM wire (nothing else) | ☐ |
