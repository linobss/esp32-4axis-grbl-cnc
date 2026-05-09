# KiCad Design Verification Checklist

## Critical: Use This Before Ordering!

This checklist ensures your PCB is manufacturing-ready. **Skip nothing** - this is how we prevent another failed prototype.

---

## Pre-Schematic Phase

### Research & Planning
- [ ] Component datasheets reviewed (ESP32, DRV8825/A4988)
- [ ] Pin compatibility verified
- [ ] Power requirements calculated
- [ ] Current draw estimated (motors + ESP32)
- [ ] Heat dissipation planned

### Schematic Preparation
- [ ] Fritzing reference image saved and accessible
- [ ] BOM list complete with:
  - [ ] Part numbers
  - [ ] Supplier links
  - [ ] Quantities
  - [ ] Costs
- [ ] Connector pinouts verified (steppers, limit switches, spindle)
- [ ] Signal voltage levels confirmed (3.3V vs 5V)

---

## Schematic Design Phase

### Component Symbols
- [ ] ESP32-DevKitC symbol used (correct pinout)
- [ ] All 4x stepper drivers have correct symbols (DRV8825 or A4988)
- [ ] All connectors match actual hardware connectors
- [ ] Power supply symbols (VCC, GND) consistent throughout
- [ ] No mismatched voltage symbols (3.3V vs 5V confusion)

### Power Distribution
- [ ] Main power input clearly labeled (12-24V)
- [ ] 3.3V regulator for ESP32 specified
- [ ] 5V regulator for drivers specified (if needed)
- [ ] All GND connections present
- [ ] Power trace thickness noted (>1.5mm for >1A)
- [ ] Decoupling capacitors (0.1µF) on each IC VCC
- [ ] Bulk capacitors (10-47µF) on main power rail

### Signal Connections
- [ ] **X-Axis**: STEP, DIR, EN pins defined and routed
- [ ] **Y-Axis**: STEP, DIR, EN pins defined and routed
- [ ] **Z-Axis**: STEP, DIR, EN pins defined and routed
- [ ] **A-Axis**: STEP, DIR, EN pins defined and routed
- [ ] All stepper motor connections (A+, A-, B+, B-) present
- [ ] Limit switch inputs (X-, X+, Y-, Y+, Z-) defined
- [ ] Probe/touch plate input defined
- [ ] Spindle control output defined (PWM + Enable)
- [ ] Spindle speed/RPM feedback (if needed)
- [ ] Coolant control (if needed)

### ESP32 Pin Usage
- [ ] **GPIO2**: LED or indicator (verify not BOOT pin conflict)
- [ ] **GPIO4**: Motor X-STEP
- [ ] **GPIO5**: Motor X-DIR
- [ ] **GPIO12**: Motor Y-STEP (verify not strapping pin)
- [ ] **GPIO13**: Motor Y-DIR
- [ ] **GPIO14**: Motor Z-STEP
- [ ] **GPIO15**: Motor Z-DIR (verify not BOOT pin conflict)
- [ ] **GPIO16**: Motor A-STEP
- [ ] **GPIO17**: Motor A-DIR
- [ ] **GPIO18**: Limit X-
- [ ] **GPIO19**: Limit X+
- [ ] **GPIO21**: Limit Y-
- [ ] **GPIO22**: Limit Y+
- [ ] **GPIO23**: Limit Z-
- [ ] **GPIO25**: Probe input
- [ ] **GPIO26**: Spindle PWM
- [ ] **GPIO27**: Spindle Enable
- [ ] **GPIO32**: Optional (reserved)
- [ ] **GPIO33**: Optional (reserved)
- [ ] **ADC Pins**: If using analog inputs
- [ ] **UART**: TX (GPIO1), RX (GPIO3) for debugging
- [ ] **SPI**: SCK, MOSI, MISO (if using SD card, etc.)
- [ ] **I2C**: SDA (GPIO21), SCL (GPIO22) - conflicts? Check!
- [ ] **No conflicts with strapping pins** (GPIO0, GPIO2, GPIO12, GPIO15)

### Stepper Driver Configuration
- [ ] All 4 drivers have identical setup (for consistency)
- [ ] Current limiting resistor values calculated
- [ ] Heat sink requirements noted
- [ ] Enable pull-up/pull-down resistors present
- [ ] Direction control polarity verified
- [ ] Microstepping selection (MS1, MS2, MS3) documented
- [ ] Reset (RST) and Sleep (SLP) pin connections verified

### Protection & Safety
- [ ] Pull-up resistors on control signals (typically 10kΩ)
- [ ] Pull-down resistors on enable signals (if needed)
- [ ] Diodes on motor outputs (if driving external relays)
- [ ] Status LED circuit (with appropriate resistor)
- [ ] Reset button on ESP32
- [ ] Fuses on main power input (2-5A for motors, 1-2A for logic)

### Design Rule Check (Electrical)
- [ ] Run: Tools > Design Rules Check
- [ ] **All errors resolved** (no errors allowed)
- [ ] All warnings reviewed and approved
- [ ] No unconnected nets
- [ ] No floating pins
- [ ] All power nets connected
- [ ] All ground nets connected

---

## PCB Layout Phase

### Board Setup
- [ ] Board dimensions set (100mm x 80mm approx)
- [ ] Layer stack defined (2-layer standard)
- [ ] Min trace width: 0.15mm (6mil) for signals
- [ ] Min trace width: 0.3mm (12mil) for power
- [ ] Min clearance: 0.15mm (6mil) between traces
- [ ] Min via diameter: 0.3mm (12mil) pad
- [ ] Design rules loaded from JLSPCB (if available)

### Component Placement
- [ ] **Power**: Power connector and regulators placed first
- [ ] **Main IC**: ESP32 placed centrally
- [ ] **Drivers**: 4x stepper drivers grouped logically (X, Y, Z, A)
- [ ] **Motor connectors**: Close to their respective drivers
- [ ] **Limit switch connectors**: Grouped, away from power
- [ ] **Spindle control**: Separated from stepper signals
- [ ] **Decoupling capacitors**: Placed directly next to IC VCC pins
- [ ] **Heat sinks**: Space allocated for stepper driver heatsinks
- [ ] **Mounting holes**: 4x M2 or M3 holes at corners
- [ ] **Rotation**: All text readable when board is oriented correctly

### Power Distribution
- [ ] Main power trace: Direct from connector to regulators (thick, >1.5mm)
- [ ] 3.3V distribution: From regulator to ESP32 and stepper drivers
- [ ] 5V distribution (if used): Separate from 3.3V
- [ ] Ground distribution: Multiple vias to ground plane
- [ ] No long power traces (keep <50mm if possible)
- [ ] Ground plane on bottom layer (if 2-layer board)

### Signal Routing
- [ ] **STEP signals**: Tight traces, minimal length
- [ ] **DIR signals**: Routed near STEP signals
- [ ] **Motor phase wires**: Thicker traces (>0.25mm)
- [ ] **Limit switch signals**: Shielded or routed carefully
- [ ] **Spindle signals**: Separated from stepper signals
- [ ] **No parallel signal traces** (unless differential pairs)
- [ ] Signals routed on top/bottom, not mixed

### Via Placement
- [ ] Vias used efficiently for signal transitions
- [ ] Ground vias placed frequently (every 10-15mm)
- [ ] No vias under components
- [ ] Via sizes appropriate (0.3mm pad typical)

### Mechanical Features
- [ ] **Board edge**: Clean, defined edge cut
- [ ] **Mounting holes**: 4x holes, M2 or M3 sized
- [ ] **ESP32 holes**: ALL 38 MOUNTING HOLES PRESENT AND CORRECT SIZE
  - [ ] Hole diameter: 1.0-1.1mm
  - [ ] Holes positioned correctly per datasheet
  - [ ] Holes through entire board
  - [ ] No solder mask on holes
- [ ] **Connector cutouts**: Verified for physical fit
- [ ] **Clearances**: 2mm min from board edge to traces
- [ ] **Test points**: Added for debugging signals (optional but helpful)

### Silk Screen (Top Layer)
- [ ] Component designators labeled (R1, C1, U1, etc.)
- [ ] Voltage labels (3.3V, 5V, GND areas)
- [ ] Connector labels (Motor X, Motor Y, Motor Z, Motor A)
- [ ] Limit switch labels (X-, X+, Y-, Y+, Z-)
- [ ] Spindle connector labeled
- [ ] Power input labeled with polarity
- [ ] "Top", "Bottom", or orientation marker
- [ ] Company/project name
- [ ] Date of design
- [ ] Version number

### Solder Mask
- [ ] No solder mask on via pads (for soldering)
- [ ] Solder mask clearance: 0.1mm from traces
- [ ] Solder mask coverage on planes

### Design Rule Check (Physical)
- [ ] Run: Tools > Design Rules Check
- [ ] **All errors resolved** (no errors allowed)
- [ ] All warnings reviewed
- [ ] Min trace width verified
- [ ] Min clearance verified
- [ ] Via sizes verified
- [ ] Hole sizes verified

---

## Gerber File Generation & Verification

### Pre-Export
- [ ] All design checks passed
- [ ] No unrouted nets
- [ ] No design rule violations
- [ ] All layers assigned correctly

### Export Settings
- [ ] File > Plot
- [ ] Output directory: `hardware/gerber/`
- [ ] Gerber format: RS-274X (extended Gerber)
- [ ] Layers selected:
  - [ ] F.Cu (Front Copper)
  - [ ] B.Cu (Back Copper)
  - [ ] F.Silkscreen
  - [ ] B.Silkscreen
  - [ ] F.Mask (Front Solder Mask)
  - [ ] B.Mask (Back Solder Mask)
  - [ ] Edge.Cuts (Board Outline)
  - [ ] Generate Drill File (.drl)
  - [ ] Generate NC Drill file

### Post-Export Verification
- [ ] All files generated successfully:
  - [ ] .kicad_pcb-F.Cu.gbr
  - [ ] .kicad_pcb-B.Cu.gbr
  - [ ] .kicad_pcb-F.SilkS.gbr
  - [ ] .kicad_pcb-B.SilkS.gbr
  - [ ] .kicad_pcb-F.Mask.gbr
  - [ ] .kicad_pcb-B.Mask.gbr
  - [ ] .kicad_pcb-Edge.Cuts.gbr
  - [ ] .drl (drill file)
  - [ ] .txt (drill file alternative format)
- [ ] ZIP file created with all Gerber files
- [ ] File sizes reasonable (not 0 bytes)

### Gerber Viewer Inspection
- [ ] Open Gerber files in GerbView (File > Plot > View > Display in GerbView)
- [ ] **Check ESP32 mounting holes**: Are all 38 holes visible?
  - [ ] Compare hole count with ESP32 datasheet (should be 38)
  - [ ] Verify hole positions match schematic
  - [ ] Verify hole diameters (1.0-1.1mm)
- [ ] Check PCB outline (Edge.Cuts layer)
  - [ ] Board is complete rectangle/shape
  - [ ] Mounting holes visible
  - [ ] No open gaps in outline
- [ ] Check front copper layer
  - [ ] All traces present
  - [ ] No broken traces
  - [ ] Power distribution visible
  - [ ] All connections intact
- [ ] Check back copper layer
  - [ ] Ground plane complete (if present)
  - [ ] Via connections visible
  - [ ] No isolated copper islands
- [ ] Check silk screen
  - [ ] All labels readable
  - [ ] No text overlapping traces
  - [ ] Orientation marker present
- [ ] Check solder mask
  - [ ] Via pads exposed
  - [ ] Component pads exposed
  - [ ] Proper clearances maintained

### JLSPCB Design Rules Verification
- [ ] Min trace width: 0.127mm (5mil) - use 0.15mm (6mil) for safety
- [ ] Min trace spacing: 0.127mm (5mil) - use 0.15mm (6mil) for safety
- [ ] Via hole size: Min 0.2mm, prefer 0.3mm
- [ ] Via annular ring: Min 0.15mm
- [ ] BGA pad size: N/A (no BGA on this design)
- [ ] Copper to edge: Min 0.3mm
- [ ] Solder mask clearance: 0.05-0.1mm (check JLSPCB settings)
- [ ] Silk screen text: Min 1mm height

---

## JLSPCB Upload Phase

### Pre-Upload
- [ ] Gerber ZIP file ready
- [ ] BOM file prepared (if ordering with components)
- [ ] Gerber viewer inspection completed
- [ ] **ESP32 holes confirmed present in Gerber files**

### Upload & Review
- [ ] Go to [jlspcb.com](https://jlspcb.com)
- [ ] Click "Quote Now"
- [ ] Upload Gerber ZIP file
- [ ] Select options:
  - [ ] PCB Qty: 2-5 (recommend 3-5 for testing)
  - [ ] Layers: 2
  - [ ] Thickness: 1.6mm (standard)
  - [ ] Copper weight: 1oz (standard)
  - [ ] Surface finish: HASL or LeadFree HASL
  - [ ] Solder mask color: Green (standard, cheapest)
  - [ ] Silk screen color: White (standard)
  - [ ] Vias: TENTED (covered with solder mask) or NOT COVERED (exposed)
    - Recommendation: NOT COVERED (easier to debug)

### Visual Inspection on JLSPCB
- [ ] Use their online Gerber viewer
- [ ] **Check AGAIN for ESP32 holes** (last chance!)
- [ ] Compare preview with expected design
- [ ] Verify mounting hole positions
- [ ] Check that all traces are present
- [ ] If ANY concerns, CANCEL and re-export Gerber files

### Final Review Before Payment
- [ ] Design specifications correct
- [ ] Price acceptable
- [ ] Estimated delivery date noted
- [ ] Shipping method selected
- [ ] **Gerber viewer shows all expected features**
- [ ] **ESP32 holes 100% VISIBLE in preview**

---

## Post-Order

### Documentation
- [ ] Order number saved
- [ ] Confirmation email saved
- [ ] Tracking number saved
- [ ] Expected delivery date noted
- [ ] Gerber files archived with design
- [ ] BOM saved with order
- [ ] KiCad project files backed up

### Learning Notes
- [ ] Document design decisions
- [ ] Note any difficult parts
- [ ] List improvements for next version
- [ ] Add to project README

---

## ⚠️ CRITICAL REMINDERS

**Before ordering ANYTHING:**
1. ✅ Generate Gerber files
2. ✅ Open in Gerber viewer
3. ✅ **MANUALLY COUNT ESP32 HOLES** (should be 38)
4. ✅ Verify hole positions
5. ✅ Check for any missing features
6. ✅ Review on JLSPCB website preview
7. ✅ **ONLY THEN click "Order"**

**This one check prevents expensive failures!** 🎯

---

## Sign-Off

Once all items checked:

- [ ] I have verified this PCB is manufacturing-ready
- [ ] All design rules are met
- [ ] ESP32 mounting holes are present and correct
- [ ] Gerber files have been inspected visually
- [ ] JLSPCB preview has been reviewed
- [ ] I understand the risks and accept the design

**Date Completed**: _______________

**Designer Name**: _______________

---

**Good luck with your prototype!** 🚀
