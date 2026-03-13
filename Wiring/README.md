# Wiring Overview

## Components
- 48V 8.3A Power Supply (set to 40.5V)
- 2.2kW Huanyang HY02D223B VFD
- 2.2kW Water-cooled spindle
- Gecko G540 4-axis stepper driver
- NEMA23 stepper motors ×4
- LM2596HVS DC-DC converter (set to 12V)
- LM2596HVS DC-DC converter (set to 24V)
- FOTEK SSR-25 DA solid state relay
- 2PDT 5A relay, 24V DC coil
- Yunpen EMI filter YG10T5
- 240V contactor
- Inductive limit/home switches ×4
- 12V 120mm fans ×2 (enclosure)
- 12V 60mm fans ×3 (stepper cooling)
- E-Stop switches ×2
- Magnetic 240V power switch
- Double 240V GPO ×2
- YB-550 9W 240V submersible pump 600L/h
- Dell OptiPlex 780 with parallel port

---

## Gecko G540 — Parallel Port Pin Assignment

The G540 uses a DB25 parallel port connection. Pin assignments are confirmed from
the Mach3 profile ([XYZ-CNC.XML](../Mach3/XYZ-CNC.XML)).

### Motor Step/Dir Outputs (DB25 Pins 2–9)

| DB25 Pin | Signal | Notes |
|----------|--------|-------|
| 2 | X Step | |
| 3 | X Dir | |
| 4 | Y Step | |
| 5 | Y Dir | |
| 6 | Z Step | |
| 7 | Z Dir | |
| 8 | A Step | A slaved to X — second X gantry motor |
| 9 | A Dir | |

### Input Signals

| DB25 Pin | Mach3 Signal | Active | Notes |
|----------|-------------|--------|-------|
| 10 | E-Stop / Limits | Low | G540 fault input — e-stops and limit switches wired into this line |
| 11 | _[confirm]_ | Low | |
| 12 | _[confirm]_ | Low | |
| 13 | _[confirm]_ | Low | |
| 15 | Probe (Z zero) | **High** | Touch plate / tool length sensor |

> Pin 10 is the G540's combined fault input. E-stop switches and limit/home switches
> are wired in series into this single line. The G540 halts all motion on any fault.

### Output Signals

| DB25 Pin | Mach3 Output | Signal | Notes |
|----------|-------------|--------|-------|
| 16 | Output 13 | _[confirm]_ | |
| 17 | Output 7 | Spindle on/off | Drives relay → VFD FOR terminal |
| 1 | Output 8 | _(unused)_ | Flood output — not connected |

### Charge Pump
- G540 charge pump DIP switch: **disabled** — stepper outputs enabled without
  requiring a charge pump signal on Pin 14
- Mach3 profile has ChargeAlwaysOn=1 but the G540 does not require it

---

## Stepper Motors

| Axis | Motor | Current (A) | G540 Current Set Resistor |
|------|-------|-------------|--------------------------|
| X | _[model]_ | _[ ]_ | _[ ]_ Ω |
| Y | _[model]_ | _[ ]_ | _[ ]_ Ω |
| Z | _[model]_ | _[ ]_ | _[ ]_ Ω |
| A | _[model]_ | _[ ]_ | _[ ]_ Ω |

---

## Spindle & VFD Wiring

- VFD model: Huanyang HY02D223B
- Spindle: 2.2kW water-cooled, ER20 collet, 24,000 RPM max
- Speed control: PWM from G540 → 0–10V analog signal to VFD VI terminal
- Spindle on/off: G540 Pin 17 → relay → VFD FOR terminal

| VFD Terminal | Function | Connected To |
|-------------|----------|-------------|
| FOR | Forward run | 2PDT relay (coil driven by G540 Pin 17) |
| DCM | Digital common | Relay common |
| VI | Speed reference (0–10V) | G540 analog out |
| ACM | Analog GND | G540 GND |
| R, S | AC input | 240V single phase (via contactor) |
| U, V, W | 3-phase output | Spindle |

---

## Limit & Home Switches

- Type: Inductive (NPN/PNP — _[confirm]_)
- Configuration: Normally closed, wired in series into G540 Pin 10 fault line
- All four switches share Pin 10 — individual axis identification is not possible
  in hardware; Mach3 uses homing sequence to infer position

---

## Wiring Diagrams / Photos

![Wiring Diagram](../Images/XYZ-CNCWiringDiagram.png)

---

## Notes
