# Lab 4 — IC Pin Connections

## MCP6004 Pinout
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
     (notch at top-left)
```

---

## ADALM1000 Settings

| Parameter | Value |
|-----------|-------|
| CHA mode | SVMI sine wave |
| Min voltage | 2.1V |
| Max voltage | 2.9V |
| Frequency | Start at 132Hz, then 620Hz |
| CHB | probe only |

---

## Power + VCM

| Connection | Through | Signal |
|-----------|---------|--------|
| ADALM +5V → +5V rail | wire | 5V |
| ADALM GND → GND rail | wire | 0V |
| MCP6004 pin4 → +5V rail | wire | 5V |
| MCP6004 pin11 → GND rail | wire | 0V |
| +5V → VCM node | 10kΩ (Brown-Black-Orange) | 2.5V |
| VCM node → GND | 10kΩ (Brown-Black-Orange) | divider |

---

## BPF-1 (132Hz) — Op-amp C: pins 8, 9, 10

```
LAYOUT (place components BELOW IC):

CHA ──[330k]──► pin9
                 │
pin9 ──wire──► ROW-X ──[47k]──► GND
                         └──[47k]──► GND  (parallel = 23.5k)
               ROW-X ──[103cap]──► GND

pin8 ──[330k]──► MID-ROW ──[330k]──► pin9  (series = 660k)
pin8 ──[103cap]──────────────────────► pin9

pin10 ──► VCM node
```

| From | To | Component | Note |
|------|----|-----------|------|
| CHA | pin 9 | 330kΩ Orange-Orange-Yellow | R11 input |
| pin 9 | row-X | wire | junction node |
| row-X | GND | 47kΩ Yellow-Violet-Orange | R33 first |
| row-X | GND | 47kΩ Yellow-Violet-Orange | R33 second (parallel) |
| row-X | GND | 103 cap | C1 |
| pin 8 | mid-row | 330kΩ Orange-Orange-Yellow | R22 first half |
| mid-row | pin 9 | 330kΩ Orange-Orange-Yellow | R22 second half (series=660k) |
| pin 8 | pin 9 | 103 cap | C2 feedback |
| pin 10 | VCM | wire | bias |

**OUTPUT = pin 8 = Vout_bpf1**
Expected at 132Hz: sine ~0.8Vpp centered at 2.5V

---

## BPF-2 (620Hz) — Op-amp D: pins 12, 13, 14

```
LAYOUT (place components ABOVE IC):

CHA ──[330k]──► pin13
                  │
pin13 ──wire──► ROW-Y ──[1k]──► GND
               ROW-Y ──[103cap]──► GND

pin14 ──[330k]──► MID-ROW2 ──[330k]──► pin13  (series = 660k)
pin14 ──[103cap]────────────────────────► pin13

pin12 ──► VCM node
```

| From | To | Component | Note |
|------|----|-----------|------|
| CHA | pin 13 | 330kΩ Orange-Orange-Yellow | R11 input |
| pin 13 | row-Y | wire | junction node |
| row-Y | GND | 1kΩ Brown-Black-Red | R33 |
| row-Y | GND | 103 cap | C1 |
| pin 14 | mid-row2 | 330kΩ Orange-Orange-Yellow | R22 first half |
| mid-row2 | pin 13 | 330kΩ Orange-Orange-Yellow | R22 second half (series=660k) |
| pin 14 | pin 13 | 103 cap | C2 feedback |
| pin 12 | VCM | wire | bias |

**OUTPUT = pin 14 = Vout_bpf2**
Expected at 620Hz: sine ~0.8Vpp centered at 2.5V

---

## Test Sequence

| Step | CHA freq | CHB to | Expected |
|------|---------|--------|---------|
| 1 | 132 Hz | pin 8 | BIG sine ✅ |
| 2 | 132 Hz | pin 14 | flat/small ✅ |
| 3 | 620 Hz | pin 14 | BIG sine ✅ |
| 4 | 620 Hz | pin 8 | flat/small ✅ |
| 5 | sweep 100→1000Hz | pin 8 | peaks at 132Hz only ✅ |
| 6 | sweep 100→1000Hz | pin 14 | peaks at 620Hz only ✅ |

---

## Critical Notes

1. **R22 must be SERIES** — two 330k end-to-end through a middle row
2. **R33 for BPF-1 must be PARALLEL** — both 47k from row-X to GND
3. **R33 must connect via row-X** not directly to pin9 (prevents short)
4. **Mid-row for R22** must be a completely free row not touching anything else
