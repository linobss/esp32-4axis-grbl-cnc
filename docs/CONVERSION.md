# Fritzing to KiCad Conversion Guide

This document outlines the process of converting the 4-axis GRBL CNC controller from Fritzing to KiCad.

## Why Convert to KiCad?

### Fritzing Limitations (Lessons Learned)
- ❌ Unreliable Gerber export for through-hole components
- ❌ Footprints sometimes lose pad/hole definitions during export
- ❌ No proper drill file generation for custom footprints
- ❌ Limited component library for specialized parts
- ❌ Difficult to verify Gerber output before manufacturing

### KiCad Advantages
- ✅ Professional-grade PCB design tools
- ✅ Reliable Gerber export with verified drill files
- ✅ Extensive component library
- ✅ Better design rule checking (DRC)
- ✅ Can preview Gerber before ordering
- ✅ Industry standard for electronics design

## Conversion Strategy

### What We're Saving from Fritzing
1. **Schematic Logic** - Circuit connections and component relationships
2. **PCB Layout Reference** - General placement and routing ideas
3. **Component List** - Bill of Materials
4. **Design Philosophy** - How axes, power, and signals are organized

### What We're Rebuilding in KiCad
1. **Schematic** - Proper component symbols and connections
2. **Footprints** - Correct pads and holes for manufacturing
3. **PCB Layout** - Signal integrity, power distribution, DRC verified
4. **Gerber Files** - Production-ready output with verified drill files

## Step-by-Step Conversion Process

### Phase 1: Export and Archive (DONE ✅)
- [ ] Export Fritzing schematic as PNG/SVG for reference
- [ ] Export Fritzing PCB as PNG/SVG for reference
- [ ] Export Fritzing BOM for parts list
- [ ] Archive original .fz file in git

### Phase 2: KiCad Project Setup (TODO)
- [ ] Create new KiCad project: `esp32-4axis-grbl`
- [ ] Set up library paths for ESP32 and DRV8825 footprints
- [ ] Create project-specific symbol library
- [ ] Create project-specific footprint library

### Phase 3: Schematic Recreation (TODO)
- [ ] Create main schematic sheets:
  - Power distribution
  - ESP32 microcontroller
  - Motor driver X (DRV8825/A4988)
  - Motor driver Y
  - Motor driver Z
  - Motor driver A
  - Connectors (USB, limit switches, spindle, probe)
- [ ] Add decoupling capacitors (0.1µF on each driver)
- [ ] Add power supply filtering
- [ ] Verify all connections against Fritzing design
- [ ] Run Electrical Rule Check (ERC)

### Phase 4: PCB Layout (TODO)
- [ ] Place all components on PCB
- [ ] Route power distribution (ground plane recommended)
- [ ] Route signal traces (STEP, DIR, EN for each motor)
- [ ] Route SPI/I2C communication if needed
- [ ] Add vias for multi-layer connections
- [ ] Add silk screen labels
- [ ] Run Design Rule Check (DRC)
- [ ] Verify trace widths and clearances

### Phase 5: Manufacturing Prep (TODO)
- [ ] Generate Gerber files (RS-274X extended format)
- [ ] Generate drill file (.drl)
- [ ] Export BOM in CSV format
- [ ] Create assembly drawing (for reference)
- [ ] **IMPORTANT**: Verify Gerber files using online viewer
- [ ] Upload to JLSPCB for preview/quote

### Phase 6: Testing & Refinement (TODO)
- [ ] Order prototype PCB
- [ ] Test ESP32 mounting and fitment
- [ ] Test all connector alignments
- [ ] Test stepper driver operation
- [ ] Document any revisions needed

## Resources

### KiCad Learning Materials
- [KiCad Official Documentation](https://docs.kicad.org/)
- [KiCad PCB Design Tutorial](https://docs.kicad.org/8.0/en/getting_started/index.html)
- [KiCad Footprint Best Practices](https://docs.kicad.org/8.0/en/pcbnew/pcbnew.html)

### Component Libraries
- [KiCad Official Libraries](https://kicad.github.io/symbols/)
- [ESP32 Footprints](https://github.com/espressif/kicad-libraries)
- [A4988/DRV8825 Datasheets](https://www.pololu.com/product/2133)

### Gerber Verification
- [Online Gerber Viewer](https://gerber.cloudsmith.io/)
- [JLSPCB Gerber Viewer](https://jlcpcb.com/) (upload and preview before ordering)

## Common Pitfalls to Avoid

1. **ESP32 Footprint**
   - ⚠️ Use official Espressif footprints
   - ⚠️ Verify all 38 pads are present
   - ⚠️ Check pad spacing matches actual board

2. **Stepper Drivers**
   - ⚠️ Ensure proper current limiting resistor values
   - ⚠️ Add decoupling capacitors (critical!)
   - ⚠️ Keep motor power separate from logic power

3. **PCB Layout**
   - ⚠️ Use ground plane for better signal integrity
   - ⚠️ Keep stepper motor traces away from signal lines
   - ⚠️ Use proper trace widths for current (2A+ per trace)
   - ⚠️ Add vias liberally for ground connections

4. **Manufacturing**
   - ⚠️ **ALWAYS** verify Gerber files before ordering!
   - ⚠️ Check drill file separately
   - ⚠️ Use 0.1mm minimum trace width (JLSPCB standard)
   - ⚠️ Verify clearances meet manufacturer specs

## Estimated Timeline

| Phase | Task | Days |
|-------|------|------|
| 1 | Export & Archive | 1 |
| 2 | KiCad Setup | 1 |
| 3 | Schematic | 3-5 |
| 4 | PCB Layout | 3-5 |
| 5 | Manufacturing Prep | 1 |
| 6 | Testing | 7-14 |
| **Total** | | **16-27 days** |

## Questions & Notes

Use this section to track questions during conversion:
- [ ] 
- [ ] 
- [ ] 

## Author Notes

- Started: 2026-05-09
- Learning: KiCad basics while converting
- Goal: Production-ready 4-axis GRBL controller
