# Mach3

Mach3 configuration, motor tuning values, and settings reference for the XYZ-CNC.

> Profile backup: [XYZ-CNC.XML](./XYZ-CNC.XML) — exported March 2025  
> Original profile configured by Eugene Vashenko

---

## Motor Tuning

Settings found under **Config → Motor Tuning** in Mach3.

| Axis | Steps/mm | Velocity (mm/min) | Acceleration (mm/s²) | Step Pulse (µs) | Dir Pulse (µs) |
|------|----------|-------------------|----------------------|-----------------|----------------|
| X    | 137.300  | 250               | 750                  | 2               | 2              |
| Y    | 137.231  | 250               | 750                  | 2               | 2              |
| Z    | 375.000  | 83.33             | 500                  | 2               | 2              |
| A    | 137.300  | 250               | 750                  | 2               | 2              |

> A axis is slaved to X (gantry drive). A steps/mm matches X exactly.

### Steps/mm Notes

- Motor steps/rev: 200 (standard NEMA23)  
- Microstepping (G540 fixed): 10×

**X/Y — belt and chain drive:**
- Motor (10T) → belt → large aluminium gear (48T) → 4.8:1 reduction
- Small chain sprocket (11T) × ¼" chain pitch (6.35mm) = 69.85mm/rev
- Theoretical: (200 × 10 × 4.8) ÷ 69.85 = **137.44 steps/mm**
- Configured value 137.300 — empirically fine-tuned from calculated baseline

**Z — leadscrew drive:**
- 375 steps/mm back-calculates to ~5.33mm/rev pitch (consistent with a standard T8 5mm leadscrew)

---

## Port & Pin Settings

Settings found under **Config → Ports & Pins** in Mach3.

- **Port 1 address:** 888 (0x378 — standard LPT1)

### Motor Outputs

| Axis | Step Pin | Dir Pin | Step Active Low | Dir Active Low |
|------|----------|---------|-----------------|----------------|
| X (Motor 0) | 2 | 3 | No | No |
| Y (Motor 1) | 4 | 5 | No | No |
| Z (Motor 2) | 6 | 7 | No | No |
| A (Motor 3) | 8 | 9 | No | No |

### Input Signals

| Mach3 Input # | Signal | Port | Pin | Active Low | Notes |
|---------------|--------|------|-----|------------|-------|
| 0  | E-Stop        | 1 | 10 | Yes | |
| 2  | _[confirm]_   | 1 | 10 | Yes | |
| 3  | _[confirm]_   | 1 | 10 | Yes | Multiple inputs share Pin 10 — switches likely wired in series/parallel |
| 5  | _[confirm]_   | 1 | 10 | Yes | |
| 8  | _[confirm]_   | 1 | 10 | Yes | |
| 11 | _[confirm]_   | 1 | 11 | Yes | |
| 22 | _[confirm]_   | 1 | 11 | Yes | |
| 25 | Probe (Z zero) | 1 | 15 | **No** | Active high |
| 29 | _[confirm]_   | 1 | 13 | Yes | |
| 30 | _[confirm]_   | 1 | 12 | Yes | |

> **Note:** Many inputs share Pin 10 — the G540 consolidates limit/home switches. Check Mach3 Ports & Pins screen to confirm which input number maps to which signal (E-Stop, X/Y/Z home, limits).

### Output Signals

| Output # | Mach3 Function | Port | Pin | Notes |
|----------|---------------|------|-----|-------|
| 7  | Spindle CW (M3/M5) | 1 | 17 | Same output used for CW and CCW |
| 8  | _(unused)_ | 1 | 1  | Flood output — not connected |
| 9  | _(unused)_ | 2 | 17 | Mist output — not connected, Port 2 unassigned |
| 13 | _[confirm]_        | 1 | 16 | |

---

## General Config

| Setting | Value | Notes |
|---------|-------|-------|
| Units | mm | |
| Backlash compensation | Off | |
| Soft limits | Off | Values configured but feature disabled |
| Debounce interval | 40 | |
| Look ahead | 199 lines | |
| Charge pump | Always on | |
| PWM base frequency | 900 Hz | Spindle speed control |

---

## Axis Travel & Soft Limits

Soft limits are currently **disabled** in the profile, but limits are configured:

| Axis | Min (mm) | Max (mm) |
|------|----------|----------|
| X | 0 | 2640 |
| Y | 0 | 1240 |
| Z | −150 | 0 |

---

## Homing

| Axis | Home speed (mm/min) |
|------|---------------------|
| X | 10 |
| Y | 10 |
| Z | 5 |
| A | 10 |

---

## Spindle Settings

| Setting | Value |
|---------|-------|
| Control method | PWM |
| PWM base frequency | 900 Hz |
| Max RPM (Pulley 1) | 24,000 |
| Spindle on output | Output 7 (Pin 17, Port 1) |
| Spindle start delay | 1.5 sec |

---

## Slave Axis

A axis is slaved to X for the dual-motor gantry. Motor 3 drives the second X rail. Direction is reversed (M3Rev=1) relative to Motor 0.

---

## Known Issues / Quirks

---

## Files in This Folder

| File | Description |
|------|-------------|
| `XYZ-CNC.XML` | Mach3 profile backup — exported March 2025 |
