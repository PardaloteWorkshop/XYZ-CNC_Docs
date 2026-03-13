# Mach3

Mach3 configuration, motor tuning values, and settings reference for the XYZ-CNC.

---

## Motor Tuning

Settings found under **Config → Motor Tuning** in Mach3.

| Axis | Steps/mm | Velocity (mm/min) | Acceleration (mm/s²) | Step Pulse (µs) | Dir Pulse (µs) |
|------|----------|-------------------|----------------------|-----------------|----------------|
| X | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |
| Y | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |
| Z | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |

### Steps/mm Calculation

```
Steps/mm = (Motor steps/rev × microstepping) ÷ (leadscrew pitch mm)
```

- Motor steps/rev:
- Microstepping (G540 default): 10x
- Leadscrew pitch: mm/rev

---

## Port & Pin Settings

Settings found under **Config → Ports & Pins** in Mach3.

### Motor Outputs

| Axis | Step Pin | Dir Pin | Step Low | Dir Low |
|------|----------|---------|----------|---------|
| X | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |
| Y | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |
| Z | _[ ]_ | _[ ]_ | _[ ]_ | _[ ]_ |

### Input Signals

| Signal | Port | Pin | Active Low |
|--------|------|-----|------------|
| E-Stop | 1 | _[ ]_ | _[ ]_ |
| X Home | 1 | _[ ]_ | _[ ]_ |
| Y Home | 1 | _[ ]_ | _[ ]_ |
| Z Home | 1 | _[ ]_ | _[ ]_ |
| X Limit | 1 | _[ ]_ | _[ ]_ |
| Y Limit | 1 | _[ ]_ | _[ ]_ |
| Z Limit | 1 | _[ ]_ | _[ ]_ |

### Spindle / Output Signals

| Signal | Port | Pin | Notes |
|--------|------|-----|-------|
| Spindle on/off | _[ ]_ | _[ ]_ | _[ ]_ |
| Spindle PWM | _[ ]_ | _[ ]_ | _[ ]_ |

---

## General Config

Settings found under **Config → General Config** in Mach3.

| Setting | Value |
|---------|-------|
| Kernel speed |  |
| Units | mm |
| Backlash compensation |  |
| Soft limits |  |
| _[ ]_ | _[ ]_ |

---

## Homing & Soft Limits

| Axis | Home direction | Soft limit min (mm) | Soft limit max (mm) |
|------|---------------|---------------------|---------------------|
| X | _[ ]_ | _[ ]_ | _[ ]_ |
| Y | _[ ]_ | _[ ]_ | _[ ]_ |
| Z | _[ ]_ | _[ ]_ | _[ ]_ |

---

## Spindle Settings

- Spindle control method: 
- Min RPM: 
- Max RPM: 
- PWM base frequency:  Hz


---

## Known Issues / Quirks
