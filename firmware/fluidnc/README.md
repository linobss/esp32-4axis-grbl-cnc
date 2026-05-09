# FluidNC Configuration

## Overview

FluidNC is a modern GRBL-compatible motion control firmware for CNC machines, specifically designed for ESP32.

- **Repository**: https://github.com/bdring/FluidNC
- **Documentation**: https://github.com/bdring/FluidNC/wiki

## Setup Steps

### 1. Download FluidNC
```bash
git clone https://github.com/bdring/FluidNC.git
cd FluidNC
```

### 2. Install Dependencies
- PlatformIO (recommended) or Arduino IDE
- ESP32 board support
- Required libraries

### 3. Configure for Your Hardware
Edit `config.yaml` with your pin assignments (see `../../hardware/pinout.md`)

### 4. Compile and Upload
```bash
platformio run --target upload
```

### 5. Test via Web Interface
- ESP32 creates WiFi network
- Access via browser: `http://192.168.4.1`
- Test each axis individually

## Pin Configuration

See `config.yaml` for:
- Stepper motor pins (STEP, DIR, EN)
- Limit switch inputs
- Spindle control
- Probe input

## Calibration

1. **Steps per mm**: Adjust for each axis based on mechanical setup
2. **Feed rates**: Set safe defaults for first use
3. **Max travel**: Define working envelope
4. **Tool length offset**: Calibrate with probe

## Safety Features

- Emergency stop (E-stop) support
- Limit switch triggering
- Soft limits (software boundaries)
- Probe triggering for tool length

---

**Status**: Pending hardware completion
