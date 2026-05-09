# ESP32 4-Axis GRBL CNC Controller

A complete CNC controller project using ESP32 microcontroller with 4 stepper motor drivers (DRV8825/A4988) for X, Y, Z, and A (rotary) axes.

## Project Status

**Phase 1: Hardware Design** 🔄  
- ✅ Initial design completed in Fritzing
- ⚠️ First prototype had manufacturing issue (missing ESP32 mounting holes)
- 🚀 **Currently: Converting to KiCad for production-ready design**

## Project Overview

This project aims to create a professional-grade CNC controller with:
- **4-Axis Control**: X, Y, Z, and A (rotary) axes
- **Stepper Drivers**: DRV8825/A4988 drivers with current limiting
- **ESP32 Microcontroller**: WiFi + Bluetooth capable
- **FluidNC Firmware**: Open-source GRBL-compatible motion control
- **Safety Features**: Limit switches, probe input, spindle control
- **Power Management**: Regulated 3.3V and motor supply

## Repository Structure

```
esp32-4axis-grbl-cnc/
├── README.md                          # This file
├── CHANGELOG.md                       # Version history
├── hardware/
│   ├── fritzing/                      # Original Fritzing design (reference)
│   │   └── exports/
│   ├── kicad/                         # KiCad project (in progress)
│   ├── gerber/                        # Production-ready Gerber files
│   ├── bom.csv                        # Bill of Materials
│   └── pinout.md                      # Detailed pin assignments
├── firmware/
│   ├── fluidnc/                       # FluidNC configuration
│   └── notes/
├── docs/
│   ├── CONVERSION.md                  # Fritzing to KiCad conversion guide
│   ├── KICAD-CHECKLIST.md             # KiCad design verification checklist
│   ├── ASSEMBLY.md                    # PCB assembly instructions
│   ├── TROUBLESHOOTING.md             # Common issues and solutions
│   └── images/                        # Photos, diagrams, schematics
└── .gitignore
```

## Hardware Specifications

### Components
- **Microcontroller**: ESP32-DevKitC (38-pin, AZ-Delivery)
- **Stepper Drivers**: 4x DRV8825 or A4988
- **Stepper Motors**: NEMA17/23 (4x)
- **Power Supply**: 12-24V for motors, regulated to 5V/3.3V
- **Connectors**: JST, 2.54mm headers
- **PCB**: 2-layer, production-ready

### Axes Configuration
- **X-Axis**: Primary horizontal
- **Y-Axis**: Secondary horizontal
- **Z-Axis**: Vertical (spindle depth)
- **A-Axis**: Rotary (optional)

## Next Steps

1. **Read Documentation**: Start with CONVERSION.md and KICAD-CHECKLIST.md
2. **Learn KiCad**: Follow the learning resources below
3. **Design Hardware**: Create schematic and PCB layout
4. **Configure Firmware**: Set up FluidNC with pin mappings
5. **Test & Deploy**: Assembly, calibration, and operation

## Learning Resources

### KiCad
- [KiCad Documentation](https://docs.kicad.org/)
- [KiCad Getting Started](https://docs.kicad.org/en/getting_started/)

### FluidNC
- [FluidNC GitHub](https://github.com/bdring/FluidNC)
- [FluidNC Wiki](https://github.com/bdring/FluidNC/wiki)

### General References
- [ESP32 Pinout](https://randomnerdtutorials.com/esp32-pinout-reference/)
- [GRBL Project](https://github.com/grbl/grbl)

## Author

**linobss** - [GitHub Profile](https://github.com/linobss)

---

**Status**: In Active Development (May 2026)  
**Phase**: Hardware Design → KiCad Implementation