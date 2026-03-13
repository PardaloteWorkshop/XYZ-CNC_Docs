# Wiring overview for CNC

## Components
- 48v 8.3amp Power Supply (Set to 40.5v)
- 2.2kw Huanyang VFD
- 2.2kw Water Cooled Spindle
- Gecko G540
- NEMA 23 size Stepper Motors x4
- LM2596HVS DC-DC converter (Set to 12v)
- LM2596HVS DC-DC converter (Set to 24v)
- FOTEK SSR-25 DA relay
- 2PDT 5A Relay 24v DC Coil
- Yunpen EMI Filter YG10T5
- 240v Contactor
- Inductive limit/home switches x4
- 12v 120mm fan x2
- 12v 60mm fan x3
- E-Stop Swtich x2
- Magnetic 240v Power Switch
- Double 240v Power Point x2
- YB-550 9w 240v submersible pump 600L/h
- Dell OptiPlex 780 with Parallel port

---

## Gecko G540

| Pin | Function | Notes |
|-----|----------|-------|
| _[ ]_ | X Step | |
| _[ ]_ | X Dir | |
| _[ ]_ | Y Step | |
| _[ ]_ | Y Dir | |
| _[ ]_ | Z Step | |
| _[ ]_ | Z Dir | |
| _[ ]_ | E-stop | Normally closed |
| _[ ]_ | Limit switches | |
| _[ ]_ | Spindle relay / PWM | |

---

## Stepper Motors

| Axis | Motor | Current (A) | G540 Current Set Resistor |
|------|-------|-------------|--------------------------|
| X | _[model]_ | _[ ]_ | _[ ]_ Ω |
| Y | _[model]_ | _[ ]_ | _[ ]_ Ω |
| Z | _[model]_ | _[ ]_ | _[ ]_ Ω |

---

## Spindle & VFD Wiring

- VFD model: Huanyang
- Spindle: 2.2kW water-cooled
- Speed control:
- VFD control terminals:

| VFD Terminal | Function | Connected To |
|-------------|----------|-------------|
| _[ ]_ | Forward run | G540 / relay |
| _[ ]_ | Speed reference | Mach3 PWM / 0-10V |
| _[ ]_ | GND | |

---

## Limit Switches

- Type: _[mechanical / optical / inductive]_
- Configuration: _[normally open / normally closed, series or individual]_
- Homing order: _[X then Y then Z, or other]_

---

## Wiring Diagrams / Photos

---

## Notes
