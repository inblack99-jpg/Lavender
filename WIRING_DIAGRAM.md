# Project Lavender: Wiring Diagram

This document outlines the pin connections between the ESP32, the HX711 Amplifier, and the Load Cell. Please ensure your device is **unplugged** from power while making these connections.

## Component Pinouts

### ESP32 (Typical "ESP32S Super Mini")
- `5V` or `VBUS`: 5V Power Input
- `GND`: Ground
- `GPIO22`: General Purpose Input/Output Pin 22 (for SCK)
- `GPIO21`: General Purpose Input/Output Pin 21 (for DOUT)

### HX711 Amplifier
- `VCC`: Power Input (connects to 5V)
- `GND`: Ground
- `SCK`: Serial Clock (Output)
- `DT`: Data (Input, often labeled DOUT)
- `E+`, `E-`, `A+`, `A-`: Load Cell connections

### Load Cell (4 Wires)
- `Red`: Excitation+ (E+)
- `Black`: Excitation- (E-)
- `White`: Output- (A-)
- `Green`: Output+ (A+)
  *(Note: Wire colors can vary. Always refer to the datasheet for your specific load cell if available.)*

## Wiring Connections

This diagram shows the direct connections to be made between the components.

```
+------------------+                    +-----------------+
| ESP32 Super Mini |                    | HX711 Amplifier |
|                  |                    |                 |
|              GND |<------------------>| GND             |
|               5V |<------------------>| VCC             |
|                  |                    |                 |
|           GPIO22 |<------------------>| SCK             |
|           GPIO21 |<------------------>| DT              |
|                  |                    |                 |
+------------------+                    +-----------------+
                                              | | | |
                                              | | | |
                                              | | | +------> E+ (Red)
                                              | | +------> E- (Black)
                                              | +------> A+ (Green)
                                              +------> A- (White)
                                                   |
                                            +-------------+
                                            |  Load Cell  |
                                            +-------------+
```

## Connection Summary Table

| ESP32 Pin | Connects To | HX711 Pin | Notes |
| :---: | :---: | :---: | :--- |
| `5V` | --> | `VCC` | Powers the amplifier. |
| `GND` | --> | `GND` | Common ground reference. |
| `GPIO21` | --> | `DT` | Data line from the amplifier. |
| `GPIO22` | --> | `SCK` | Clock line to the amplifier. |

| HX711 Pin | Connects To | Load Cell Wire |
| :---: | :---: | :---: |
| `E+` | --> | Red |
| `E-` | --> | Black |
| `A+` | --> | Green |
| `A-` | --> | White |
