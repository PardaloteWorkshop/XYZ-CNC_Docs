# Manuals

Reference manuals and datasheets for all major components.

---

## Files in This Folder

| File | Description |
|------|-------------|
| [G540-MANUAL-REV8.pdf](./G540-MANUAL-REV8.pdf) | Gecko G540 4-axis stepper driver — Rev 8 |
| [HY02D223B-VFD-MANUAL.pdf](./HY02D223B-VFD-MANUAL.pdf) | Huanyang HY02D223B VFD manual |
| [UC400ETH_manual.pdf](./UC400ETH_manual.pdf) | CNCdrive UC400ETH motion controller — for planned upgrade |
| Homann Designs EN-010 — CC-01 G540 Stepper Controller Kit Wiring Rev 8.4 | The XYZ-CNC wiring follows this reference design. See https://www.homanndesigns.com |



---

## Key Reference Info

### Gecko G540

- Input voltage: 18–50V DC (running at 40.5V)
- Max current per axis: 3.5A (motors are 3A — within limit)
- Microstepping: fixed 10×
- Charge pump DIP switch: **disabled** on this machine
- Current set resistor formula: `R = 47,000 × (I ÷ 67)`
  - 3A motors → ~2.1kΩ → fitted as **2.2kΩ** (nearest standard value)
- Fault indicator: red LED on G540 front panel
- Step/dir pins: 2–9 (see [Wiring/README.md](../Wiring/))

### VFD — Huanyang HY02D223B

- Input: Single phase 240V
- Output: 3-phase 240V to spindle (U, V, W terminals)
- Control mode: External terminal (PD001=1), analog speed reference 0–10V (PD002=1)
- Max output frequency: 400Hz (PD003/004/005)
- Spindle start/stop: FOR terminal via relay driven from G540 Pin 17
- Speed reference: VI terminal (0–10V analog from G540)
- Full parameter table in [Wiring/README.md](../Wiring/)

### Spindle — 2.2kW Water-cooled

- Power: 2.2kW
- Max RPM: 24,000 (400Hz)
- Collet: ER20
- Cooling: water-cooled — YB-550 submersible pump, 600L/h
- Mounting: 80mm diameter

### Stepper Motors — Wantai 57BYGH633

- Step angle: 1.8° (200 steps/rev)
- Rated current: 3A per phase
- Phase resistance: 1Ω
- Phase inductance: 1.6mH
- Holding torque: 13.5 kg·cm
- Wiring: 6-wire, connected as 4-wire bipolar (centre taps unused)

---

## External Links

- Gecko G540 support: https://www.geckodrive.com/support
- XYZ-CNC archived site: https://web.archive.org/web/20220307203536/https://xyz-cnc.com/

---

## Notes
