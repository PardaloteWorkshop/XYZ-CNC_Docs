# Wiring

Overview of all wiring in the CNC system — control box, motion, spindle, and safety circuits.

---

## Component List

| Component | Details |
|-----------|---------|
| 48V Power Supply | 8.3A, set to 40.5V |
| DC-DC Buck Converter (x2) | LM2596HVS — one set to 12V, one set to 24V |
| Stepper Driver | Gecko G540 4-axis |
| Stepper Motors | NEMA23 x4 |
| VFD | Huanyang HY02D223B — 2.2kW, single phase 240V input |
| Spindle | 2.2kW water-cooled, 80mm, ER20 collet |
| Spindle Relay | FOTEK SSR-25 DA (solid state relay) |
| Control Relay | 2PDT 5A, 24V DC coil |
| EMI Filter | Yunpen YG10T5 |
| Mains Contactor | 240V contactor |
| Limit/Home Switches | Inductive x4 |
| Fans (enclosure) | 12V 120mm x2 |
| Fans (stepper cooling) | 12V 60mm x3 |
| E-Stop | Mushroom button x2 |
| Mains Switch | Magnetic 240V power switch |
| Power Points | Double 240V GPO x2 |
| Coolant Pump | YB-550 9W 240V submersible, 600L/h |
| Control PC | Dell OptiPlex 780, parallel port |

---

## Power Distribution Overview

```
240V Mains
├── Magnetic switch → Yunpen EMI filter → 240V contactor
│   ├── Huanyang VFD (spindle)
│   ├── Coolant pump (YB-550)
│   └── 240V GPOs (PC, accessories)
│
└── 48V PSU (set to 40.5V)
    ├── Gecko G540 (motion)
    ├── LM2596HVS → 12V (fans, switches)
    └── LM2596HVS → 24V (relay coil, control signals)
```

---

## Gecko G540

The G540 receives step/direction signals from the PC parallel port (DB25) via Mach3. The charge pump signal must be active (Mach3 running) for the drives to enable.

- Input voltage: 18–50V DC (running at 40.5V)
- Max current per axis: 3.5A
- Microstepping: fixed 10x
- Current set resistor: `R = 47kΩ × (Motor current A / 3.5)`
- Fault LED: red indicator on front panel

### Parallel Port Pin Assignments

| Axis | Step Pin | Dir Pin |
|------|----------|---------|
| X | _[ ]_ | _[ ]_ |
| Y | _[ ]_ | _[ ]_ |
| Z | _[ ]_ | _[ ]_ |
| A | _[ ]_ | _[ ]_ |

### Input Pins

| Signal | Pin | Active Low | Notes |
|--------|-----|------------|-------|
| E-Stop | _[ ]_ | Yes | NC loop — both buttons in series |
| X Home/Limit | _[ ]_ | _[ ]_ | Inductive switch |
| Y Home/Limit | _[ ]_ | _[ ]_ | Inductive switch |
| Z Home/Limit | _[ ]_ | _[ ]_ | Inductive switch |
| Charge pump | _[ ]_ | — | Required for drive enable |

### Output Pins

| Signal | Pin | Notes |
|--------|-----|-------|
| Spindle on/off | _[ ]_ | Drives FOTEK SSR-25 DA or 2PDT relay |
| Spindle PWM | _[ ]_ | 0–10V analog to VFD VI terminal |

---

## Stepper Motors

| Axis | Motor Model | Current (A) | Current Set Resistor |
|------|-------------|-------------|----------------------|
| X | _[ ]_ | _[ ]_ A | _[ ]_ Ω |
| Y | _[ ]_ | _[ ]_ A | _[ ]_ Ω |
| Z | _[ ]_ | _[ ]_ A | _[ ]_ Ω |
| A | _[ ]_ | _[ ]_ A | _[ ]_ Ω |

> 60mm fans mounted near each stepper for cooling — 12V, controlled via optocoupler/MOSFET circuit (see [Upgrades](../Upgrades/))

---

## Spindle & VFD — Huanyang HY02D223B

- Input: Single phase 240V (connected to R + S terminals — for 220V class, single phase uses any two phases of R, S, T)
- Output: Three phase 0–400Hz to spindle U, V, W terminals
- Control mode: V/F (SPWM)
- Speed reference: 0–10V analog via VI terminal

### Main Circuit Terminals

| Terminal | Function | Connected To |
|----------|----------|-------------|
| R, S | AC input (single phase 240V) | Mains via contactor |
| U, V, W | Three phase output | Spindle motor |
| E | Earth/ground | Earth bar |
| P, Pr | Braking resistor | Not connected |

### Control Circuit Terminals

| Terminal | Function | Connected To |
|----------|----------|-------------|
| FOR | Forward run (Multi-Input 1) | Mach3 spindle on signal via relay |
| DCM (COM) | Common for digital signals | GND reference |
| VI | Analog voltage speed reference (0–10V) | Mach3 PWM → 0–10V via G540 |
| ACM (GND) | Common for analog signals | Analog GND |
| +10V | Internal 10V reference | _[not used / potentiometer if fitted]_ |

### Key VFD Parameters (CNC Spindle Setup)

These are the parameters typically set for Mach3 external control of a spindle. Record your actual values in the User Set Value column.

| Parameter | Function | Recommended | Set Value |
|-----------|----------|-------------|------------|
| PD001 | Source of run commands | 1 (external terminal) | _[ ]_ |
| PD002 | Source of operating frequency | 1 (external analog VI) | _[ ]_ |
| PD003 | Main frequency | 400.00 Hz (for 24,000 RPM) | _[ ]_ |
| PD004 | Base frequency | 400.00 Hz | _[ ]_ |
| PD005 | Max operating frequency | 400.00 Hz | _[ ]_ |
| PD007 | Min frequency | _[set min spindle speed]_ | _[ ]_ |
| PD011 | Frequency lower limit | 0 | _[ ]_ |
| PD014 | Accel time 1 | _[e.g. 5–10s]_ | _[ ]_ |
| PD015 | Decel time 1 | _[e.g. 5–10s]_ | _[ ]_ |
| PD023 | Rev rotation select | 0 (disabled — spindle one direction) | _[ ]_ |
| PD070 | Analog input type | 0 (0–10V) | _[ ]_ |
| PD141 | Rated motor voltage | 220 | _[ ]_ |
| PD142 | Rated motor current | _[from spindle nameplate]_ | _[ ]_ |
| PD143 | Motor pole number | 2 (for high-speed spindle) | _[ ]_ |
| PD144 | Rated motor revolution | 24000 | _[ ]_ |

> Full parameter list in the VFD manual: [HY02D223B-VFD-MANUAL.pdf](../Manuals/)

---

## Limit & Home Switches

- Type: Inductive
- Quantity: 4
- Configuration: 
- Power: 24V DC (from LM2596HVS buck converter)

---

## Relay Functions

| Relay | Type | Function |
|-------|------|---------|
| FOTEK SSR-25 DA | Solid state, 25A |  |
| 2PDT 5A 24V coil | Mechanical |  |

---

## Cooling Fans

| Fan | Size | Voltage | Location | Control |
|-----|------|---------|----------|---------|
| Enclosure fan x2 | 120mm | 12V | Control enclosure | _[direct / thermostat]_ |
| Stepper fan x3 | 60mm | 12V | Near each stepper | Optocoupler/MOSFET circuit (upgrade in progress) |

---

## Wiring Diagrams / Photos

![Wiring](Images/XYZ-CNCWiringDiagram.png)

---

## Notes
