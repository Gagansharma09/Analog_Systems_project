# Lab 1 — LTSpice Setup Guide

## Step 1 — Install LTSpice
Download from: https://www.analog.com/en/design-center/design-tools-and-calculators/ltspice-simulator.html

## Step 2 — Install LM339 Model

### Mac:
```bash
echo '.SUBCKT LM339 1 2 3 4 5
F1 9 3 V1 1
IEE 3 7 DC 100.0E-6
VI1 21 1 DC .75
VI2 22 2 DC .75
Q1 9 21 7 QIN
Q2 8 22 7 QIN
Q3 9 8 4 QMO
Q4 8 8 4 QMI
.MODEL QIN PNP(IS=800.0E-18 BF=2.000E3)
.MODEL QMI NPN(IS=800.0E-18 BF=1002)
.MODEL QMO NPN(IS=800.0E-18 BF=1000 CJC=1E-15 TR=807.4E-9)
E1 10 4 9 4 1
V1 10 11 DC 0
Q5 5 11 4 QOC
.MODEL QOC NPN(IS=800.0E-18 BF=20.29E3 CJC=1E-15 TF=942.6E-12 TR=543.8E-9)
DP 4 3 DX
RP 3 4 46.3E3
.MODEL DX D(IS=800.0E-18)
.ENDS' > ~/Library/Application\ Support/LTspice/lib/sub/LM339.sub
```

### Windows:
Copy `LM339.sub` to:
```
C:\Users\[YourName]\Documents\LTspiceXVII\lib\sub\
```

## Step 3 — Open Schematic
1. Open LTSpice
2. File → Open → select `Exp1.asc`
3. Add directive: `.lib LM339.sub`

## Step 4 — Run Simulation
1. Press **R** or click green Run button
2. Set: `.tran 0 5m 0 10n startup`
3. Click wire at MCP6004 output → see triangle wave
4. Click wire at LM339 output → see square wave

## Step 5 — Expected Results
| Signal | Value |
|--------|-------|
| Frequency | ~5.3 kHz |
| VRAMP amplitude | ~1.06 Vpp |
| VRAMP center | 2.5V |

## Component Models Used
- **U1 (MCP6004)**: UniversalOpAmp2 with `Avol=100K GBW=1Meg Slew=10Meg Rail=5`
- **U2 (LM339)**: UniversalOpAmp2 with `Avol=1Meg GBW=100Meg Slew=500Meg Rail=5`
