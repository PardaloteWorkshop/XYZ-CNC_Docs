# Upgrades & Modifications

Modifications, upgrades, and improvements made to the machine since acquisition.

---

## Completed / In Progress

### PC / Electronics Enclosure Fan Upgrade
- **Status:** In progress
- **Description:** Replace shorted 12V fan with two 120mm fans for enclosure
  cooling. ESP32 + DS18B20 temperature sensors control the 120mm fans via PWM.
  Existing 60mm stepper cooling fans switched via optocoupler/MOSFET circuit.
  Viewing window to be added to enclosure door.
- **Files:** Arduino sketch, schematic, BOM

---

## Planned

### UC400ETH Controller + UCCNC
- Replace Dell OptiPlex 780 parallel port with UC400ETH Ethernet motion controller
- Migrate from Windows 7 32-bit + Mach3 to Windows 10 + UCCNC
- All pin assignments and motor tuning documented in Mach3/README.md and
  Wiring/README.md — will require remapping to UC400ETH connector numbering in UCCNC

### Z-Axis Linear Rail Upgrade — HLTNC GX150
- **Status:** Planned
- Replace current Z-axis assembly (leadscrew + V-wheels) with HLTNC GX150
  ballscrew linear module

#### GX150 Specs

| Spec | Detail |
|------|--------|
| Model | HLTNC GX150 |
| Linear rail | HGR20 |
| Ballscrew | SFU1610 (16mm diameter, 10mm pitch) |
| Drive | Direct drive — stepper coupled directly to ballscrew |
| Available strokes | 100 / 150 / 200 / 300 / 400 / 500mm |
| Source | AliExpress |

#### Current Z vs New Z

| | Current | GX150 |
|-|---------|-------|
| Drive type | Leadscrew (~5.33mm pitch) | Ballscrew SFU1610 (10mm pitch) |
| Reduction | 10T → belt → 10T (1:1) | Direct drive (1:1) |
| Steps/mm | 375 | **200** |
| Velocity (mm/min) | 83.33 | TBD — expect significantly higher |
| Acceleration (mm/s²) | 500 | TBD — retuning required |
| Backlash | Present | Minimal — ballscrew preload |
| Rigidity | V-wheels | HGR20 linear rail |

#### Steps/mm Calculation (New)
```
Steps/mm = (200 steps/rev × 10 microsteps) ÷ 10mm pitch = 200 steps/mm
```

#### Changes Required in UCCNC
- Z steps/mm: 375 → 200
- Retune Z velocity and acceleration
- Verify Z soft limit (currently −150mm) matches stroke of purchased unit
- Backlash compensation for Z can likely be zeroed out

### Independent Height Dust Boot
- Self-adjusting dust boot that maintains consistent skirt height independent
  of Z position

---

## Replacement Parts & Sourcing

| Part | Source | Notes |
|------|--------|-------|
| CNC carbide bits | AliExpress — Tideway, Huhao | Yet to test |
| HLTNC GX150 Z-axis module | AliExpress — HLTNC store | Stroke TBD |
| _[ ]_ | _[ ]_ | _[ ]_ |

---

## Notes

What's worked, what hasn't...
