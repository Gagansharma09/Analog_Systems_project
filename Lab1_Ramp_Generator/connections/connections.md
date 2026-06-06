# Lab 1 — IC Pin Connections

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
 OUT1───┤1        14├── OUT4
 IN1-───┤2        13├── IN4-
 IN1+───┤3        12├── IN4+
  VCC───┤4        11├── GND
 IN2+───┤5        10├── IN3+
 IN2-───┤6         9├── IN3-
 OUT2───┤7         8├── OUT3
        └──────────┘
     (notch/dot at top-left)
```

---

## ADALM1000 Power

| ADALM Pin | Connect To |
|-----------|-----------|
| +5V | +5V rail (red) |
| GND | GND rail (black) |
| CHB | probe only — no output |

---

## VCM Divider (generates 2.5V)

| From | To | Through |
|------|----|---------|
| +5V rail | VCM node | 10kΩ (Brown-Black-Orange) |
| VCM node | GND rail | 10kΩ (Brown-Black-Orange) |

**VCM node = 2.5V**

---

## MCP6004 IC#1 — Integrator (Op-amp A: pins 1,2,3)

| Pin | Function | Connect To | Signal |
|-----|---------|-----------|--------|
| pin 4 | VDD | +5V rail | 5V |
| pin 11 | GND | GND rail | 0V |
| pin 3 | IN+A | VCM node | 2.5V DC |
| pin 2 | IN−A | LM339 pin1 through R1 (22kΩ) | VSQR via R1 |
| pin 1 | OUTA | pin2 through C1 (10nF) | VRAMP output |
| pin 1 | OUTA | LM339 pin3 through R2 (10kΩ) | VRAMP to Schmitt |

**VRAMP = MCP6004 pin 1**
Expected: triangle wave, ~5.3kHz, ~1.06Vpp, centered at 2.5V

---

## LM339 IC#1 — Schmitt Trigger (Comparator 1: pins 1,2,3)

| Pin | Function | Connect To | Signal |
|-----|---------|-----------|--------|
| pin 4 | VCC | +5V rail | 5V |
| pin 11 | GND | GND rail | 0V |
| pin 2 | IN1− | VCM node | 2.5V reference |
| pin 3 | IN1+ | MCP6004 pin1 through R2 (10kΩ) | VRAMP |
| pin 3 | IN1+ | LM339 pin1 through R3 (47kΩ) | feedback |
| pin 1 | OUT1 | MCP6004 pin2 through R1 (22kΩ) | VSQR to integrator |
| pin 1 | OUT1 | +5V through 10kΩ pull-up | open-collector pull-up |

**VSQR = LM339 pin 1**
Expected: square wave, 0V to 5V, ~5.3kHz

---

## Wiring Summary Diagram

```
+5V ──┬── MCP6004 pin4
      ├── LM339 pin4 (VCC)
      ├── 10kΩ ──► VCM node ──10kΩ──► GND
      └── 10kΩ ──► LM339 pin1 (pull-up)

VCM ──┬── MCP6004 pin3 (IN+)
      └── LM339 pin2 (IN1-)

MCP6004 pin1 (VRAMP)
      ├──10nF──► MCP6004 pin2
      └──10kΩ──► LM339 pin3 (IN1+)

LM339 pin1 (VSQR)
      ├──22kΩ──► MCP6004 pin2 (IN-)
      └──47kΩ──► LM339 pin3 (IN1+)

GND ──┬── MCP6004 pin11
      └── LM339 pin11
```

---

## Probe Points

| CHB to | Expected |
|--------|---------|
| MCP6004 pin 1 | Triangle wave ~1Vpp at 2.5V, 5.3kHz |
| LM339 pin 1 | Square wave 0-5V, 5.3kHz |

---

## ADALM Settings (Lab 1 — No input needed, self-oscillating)

| Parameter | Value |
|-----------|-------|
| CHA | NOT used (no waveform needed) |
| CHB | Probe only |
| Timebase | 50µs/div or 100µs/div |
