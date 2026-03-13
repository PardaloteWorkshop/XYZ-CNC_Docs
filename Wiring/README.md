# Wiring Overview new

## Components

- 48V 8.3A power supply (set to 40.5V)
- Huanyang HY02D223B 2.2kW VFD
- 2.2kW water-cooled spindle
- Gecko G540 4-axis stepper driver
- Wantai 57BYGH633 NEMA23 stepper motors ×4
- LM2596HVS DC-DC converter (set to 12V)
- LM2596HVS DC-DC converter (set to 24V)
- FOTEK SSR-25 DA solid state relay
- 2PDT 5A relay, 24V DC coil
- Yunpen EMI filter YG10T5
- 240V contactor
- Inductive limit/home switches ×4
- 12V 120mm fans ×2 (enclosure cooling)
- 12V 60mm fans ×3 (stepper cooling)
- E-Stop switches ×2
- Magnetic 240V power switch
- Double 240V GPO ×2
- YB-550 9W 240V submersible pump 600L/h
- Dell OptiPlex 780 with parallel port

---

## Wiring Diagram

![Wiring Diagram](../Images/XYZ-CNCWiringDiagram.png)

---				 

## Gecko G540 — Parallel Port Pin Assignment

Pin assignments confirmed from Mach3 profile
([Mach3/XYZ-CNC.XML](../Mach3/XYZ-CNC.XML)).

### Motor Step/Dir Outputs

| DB25 Pin | Signal | Notes |
|----------|--------|-------|
| 2 | X Step | |
| 3 | X Dir | |
| 4 | Y Step | |
| 5 | Y Dir | |
| 6 | Z Step | |
| 7 | Z Dir | |
| 8 | A Step | Slaved to X — second gantry motor |
| 9 | A Dir | |

### Input Signals

| DB25 Pin | Signal | Active | Notes |
|----------|--------|--------|-------|
| 10 | E-Stop / Limits | Low | G540 fault input — e-stops and limit/home switches wired in series into this line |
| 11 | _[confirm]_ | Low | |
| 12 | _[confirm]_ | Low | |
| 13 | _[confirm]_ | Low | |
| 15 | Probe (Z zero) | **High** | Touch plate — note opposite polarity to all other inputs |

> Pin 10 is the G540's combined fault input. Any break in the series circuit
> (e-stop pressed, limit triggered) halts all motion immediately.

### Output Signals

| DB25 Pin | Signal | Notes |
|----------|--------|-------|
								  
| 17 | Spindle on/off | Drives 2PDT relay → VFD FOR terminal |
| 1 | _(unused)_ | Flood output in Mach3 — not connected |
| 16 | _[confirm]_ | |
| 14 | Charge pump | G540 charge pump DIP switch **disabled** on this machine — signal output by Mach3 but ignored by G540 |

### Analog Speed Output (G540 → VFD)
																			   
										  
																	 

| G540 Terminal | VFD Terminal | Signal |
|--------------|-------------|--------|
| +10V | VR | 10V reference |
| Vout | VI | 0–10V analog speed signal |
| AGnd | ACM | Analog ground |

---

## Stepper Motors

**Model: Wantai 57BYGH633** (all four axes)

| Axis | Current (A) | G540 Current Set Resistor |
|------|-------------|--------------------------|
| X | 3A | 2.2kΩ (calc: 2.1kΩ) |
| Y | 3A | 2.2kΩ (calc: 2.1kΩ) |
| Z | 3A | 2.2kΩ (calc: 2.1kΩ) |
| A | 3A | 2.2kΩ (calc: 2.1kΩ) |

> Motors are 6-wire — connected as 4-wire bipolar. Centre tap wires unused.
> Verify fitted resistor values against G540 board.

---

## Spindle & VFD Wiring

| Spec | Detail |
|------|--------|
| VFD | Huanyang HY02D223B |
| Spindle | 2.2kW water-cooled, ER20, 24,000 RPM max |
| Speed control | 0–10V analog from G540 Vout → VFD VI terminal |
| On/off control | G540 Pin 17 → 2PDT relay → VFD FOR/DCM terminals |

### VFD Terminal Connections

| VFD Terminal | Function | Connected To |
|-------------|----------|-------------|
															   
									   
													
							   
| R, S | AC input (single phase) | 240V via contactor |
| U, V, W | 3-phase output | Spindle |
| VR | +10V reference input | G540 +10V |
| VI | Analog speed (0–10V) | G540 Vout |
| ACM | Analog GND | G540 AGnd |
| FOR | Forward run | 2PDT relay NO contact |
| DCM | Digital common | 2PDT relay common |

### Key VFD Parameters

| Parameter | Function | Setting |
|-----------|----------|---------|
| PD001 | Run command source | 1 (external terminal) |
| PD002 | Frequency source | 1 (analog VI) |
| PD003/004/005 | Main/base/max frequency | 400Hz |
| PD070 | Analog input type | 0 (0–10V) |
| PD141 | Rated motor voltage | 220 |
| PD143 | Motor poles | 2 |
| PD144 | Rated RPM | 24000 |

---

## Limit & Home Switches

- Type: Inductive — NPN/PNP _[confirm]_
- Configuration: Normally closed, wired in series into G540 Pin 10 fault line
- All switches share Pin 10 — individual axis identification not possible in hardware
- Mach3 uses homing sequence to determine position on startup

---

## Notes
