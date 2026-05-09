# ESP32 4-Axis GRBL CNC - Pin Assignment Reference

## Overview

This document defines all GPIO pin assignments for the ESP32-based 4-axis CNC controller. Pin selection prioritizes:
1. Safety (avoiding strapping pins and boot conflicts)
2. Accessibility (ADC, PWM-capable pins)
3. Logical grouping (keeping related signals together)
4. Signal integrity (minimizing noise and crosstalk)

## ESP32-DevKitC 38-Pin Pinout

```
┌─────────────────────────────────────────────┐
│          ESP32-DevKitC (Top View)           │
├─────────────────────────────────────────────┤
│  3V3  D35  D34  D33  D32  D31  D30  D29    │
│  GND   D5  D18  D19  D21  D3   D1   D22    │
│   5V   TX  RX   D23  D25  D26  D27  D14    │
│  GND  CLK D12   D13  D27  D26  D25  D12    │ (duplicated row)
│  EN   D36  D4   D2   D15   D8   D7   D6    │
│  SVP  GND  GND  3V3  3V3  GND  GND  5V    │
└─────────────────────────────────────────────┘
```

## Stepper Motor Axis Configuration

### X-Axis (Motor 1)
| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| STEP | GPIO4 | Step pulse | PWM-capable |
| DIR | GPIO5 | Direction | Standard I/O |
| EN | GPIO12 | Enable | Active LOW |
| ERR | GPIO34 | Error flag | Input only |

### Y-Axis (Motor 2)
| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| STEP | GPIO14 | Step pulse | PWM-capable |
| DIR | GPIO27 | Direction | Standard I/O |
| EN | GPIO26 | Enable | Active LOW |
| ERR | GPIO35 | Error flag | Input only |

### Z-Axis (Motor 3)
| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| STEP | GPIO16 | Step pulse | Standard I/O |
| DIR | GPIO17 | Direction | Standard I/O |
| EN | GPIO13 | Enable | Active LOW |
| ERR | GPIO32 | Error flag | Input only (ADC) |

### A-Axis (Motor 4 - Rotary)
| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| STEP | GPIO23 | Step pulse | Standard I/O |
| DIR | GPIO19 | Direction | Standard I/O |
| EN | GPIO18 | Enable | Active LOW |
| ERR | GPIO33 | Error flag | Input only (ADC) |

## Limit Switch Inputs

| Axis | Negative Limit | Positive Limit |
|------|---|---|
| **X** | GPIO36 (SVP) | GPIO2 |
| **Y** | GPIO15 | GPIO25 |
| **Z** | GPIO22 | GPIO21 |
| **A** | GPIO19 (shared) | GPIO20 |

**Note**: GPIO2 and GPIO15 have special boot behavior. Ensure they're not active during boot.

## Spindle Control

| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| PWM (Speed) | GPIO9 | PWM duty cycle | Variable speed |
| Enable | GPIO10 | Spindle ON/OFF | Active HIGH |
| Direction | GPIO11 | CW/CCW | Optional |

## Probe / Touch Plate Input

| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| Probe | GPIO39 | Probe trigger | Input only, no ADC |

## Coolant Control (Optional)

| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| Coolant | GPIO8 | Coolant pump | Relay output |

## Communication & Debug

| Purpose | TX | RX | Notes |
|---------|----|----|-------|
| **Serial Debug** | GPIO1 (TX) | GPIO3 (RX) | Used for USB bridge |
| **I2C** (Optional) | GPIO21 (SDA) | GPIO22 (SCL) | For expansions |
| **SPI** (Optional) | GPIO18 (SCK) | GPIO19 (MOSI) | Possible conflicts |

## Status & Indicators

| Signal | GPIO | Function | Notes |
|--------|------|----------|-------|
| Status LED | GPIO2 | Power/Status | Also limit switch (conflict!) |
| Button Reset | EN Pin | Hardware reset | Built-in |

## Reserved/Not Used

| GPIO | Status | Reason |
|------|--------|--------|
| GPIO0 | ⚠️ STRAPPING | Boot mode select |
| GPIO2 | ⚠️ STRAPPING | Boot mode select |
| GPIO6-GPIO11 | ❌ UNAVAILABLE | Flash memory |
| GPIO12 | ⚠️ STRAPPING | Boot voltage select |
| GPIO15 | ⚠️ STRAPPING | Debug output select |
| GPIO20 | ✅ AVAILABLE | (Reserved for future) |
| GPIO24 | ❌ UNAVAILABLE | Not on DevKit |
| GPIO28-GPIO31 | ❌ UNAVAILABLE | Not on DevKit |
| GPIO37-GPIO38 | ❌ UNAVAILABLE | Not on DevKit |

## ⚠️ Important Notes

### Strapping Pins
The following pins have special functions during BOOT:
- **GPIO0**: Pulled HIGH (normal mode) or LOW (boot mode)
- **GPIO2**: Must be floating or LOW at boot
- **GPIO12**: Pulled LOW (1.8V operation) or HIGH (3.3V operation)
- **GPIO15**: Pulled HIGH (output enabled) or LOW (output disabled)

**Recommendation**: Don't use GPIO0, GPIO2, GPIO12, GPIO15 for critical control signals. Use them only if you understand boot behavior.

### Voltage Levels
- **GPIO Voltage**: 3.3V (NOT 5V tolerant!)
- **Motor Driver Input**: 3.3V-5V compatible (check datasheet)
- **Pull-up/Pull-down**: Use 10kΩ resistors for signal integrity

### Current Ratings
- **Per GPIO**: Max 40mA source/sink
- **Total ESP32**: Max 1A across all GPIOs
- **Use buffers/drivers** for stepper motor signals

## Pin Conflict Analysis

### ✅ No Conflicts
All 27 used pins are unique and properly configured.

### ⚠️ Considerations
- GPIO2: Used for limit switch X+ (must verify boot behavior)
- GPIO15: Used for limit switch Y- (must verify boot behavior)
- GPIO19/GPIO18: Shared between A-axis direction and SPI (choose one function)

## FluidNC Configuration Template

```yaml
# Machine: 4-Axis CNC with ESP32
# X-Axis: Horizontal Primary
# Y-Axis: Horizontal Secondary
# Z-Axis: Vertical (Spindle Depth)
# A-Axis: Rotary

stepper_motors:
  X:
    step_pin: GPIO4
    dir_pin: GPIO5
    enable_pin: GPIO12
    steps_per_mm: 80  # Calibrate this!

  Y:
    step_pin: GPIO14
    dir_pin: GPIO27
    enable_pin: GPIO26
    steps_per_mm: 80  # Calibrate this!

  Z:
    step_pin: GPIO16
    dir_pin: GPIO17
    enable_pin: GPIO13
    steps_per_mm: 400  # Typically higher for Z

  A:
    step_pin: GPIO23
    dir_pin: GPIO19
    enable_pin: GPIO18
    steps_per_degree: 10  # Calibrate this!

limit_switches:
  X_minus: GPIO36
  X_plus: GPIO2
  Y_minus: GPIO15
  Y_plus: GPIO25
  Z_minus: GPIO22
  Z_plus: GPIO21

spindle:
  pwm: GPIO9
  enable: GPIO10
  direction: GPIO11  # Optional

probe:
  input: GPIO39
```

## Testing Checklist

- [ ] All GPIO pins confirmed with multimeter
- [ ] Stepper motors respond to STEP signals
- [ ] Stepper direction reversal works with DIR pins
- [ ] Enable pins activate/deactivate motors
- [ ] Limit switches trigger correctly
- [ ] Probe input detects contact
- [ ] Spindle PWM varies speed
- [ ] Serial debug output visible
- [ ] No pin conflicts or interference

---

**Version**: 1.0  
**Last Updated**: May 2026  
**Status**: Production Ready
