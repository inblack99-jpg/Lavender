# Project Lavender: Wiring Diagram

This document outlines the pin connections between the ESP32, the HX711 Amplifier, the Load Cell, and the power system components. Please ensure your device is **unplugged** from power while making these connections.

## Component Pinouts

### Power System
- **Solar Cell (5V Regulated):** Provides charging power. *Assumes a regulated 5V output.* 
  - `+`: Positive Output
  - `-`: Negative Output
- **3.7V LiPo Battery:** Stores power for the system.
  - `+`: Positive Terminal (Connects to ESP32 B+)
  - `-`: Negative Terminal (Connects to ESP32 B-)

### ESP32 (Typical "ESP32S Super Mini" with Built-in LiPo Charger)
- `5V` or `VBUS`: 5V Power Input (for Solar Cell input)
- `GND`: Ground (for Solar Cell input)
- `B+`: LiPo Battery Positive Terminal
- `B-`: LiPo Battery Negative Terminal
- `GPIO22`: General Purpose Input/Output Pin 22 (for SCK)
- `GPIO21`: General Purpose Input/Output Pin 21 (for DOUT)

### HX711 Amplifier
- `VCC`: Power Input (connects to 5V from ESP32)
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
+------------+                          +------------------+                    +-----------------+
| Solar Cell |------------------------->| ESP32 Super Mini |                    | HX711 Amplifier |
| (5V Reg.)  |                          | (with LiPo Chrgr)|                    |                 |
| [+]        |------------------------->| 5V / VBUS        |------------------->| VCC             |
| [-]        |------------------------->| GND              |------------------->| GND             |
|            |                          |                  |                    |                 |
+------------+                          | B+               |<-------------------| + (LiPo Battery) |
                                        | B-               |<-------------------| - (LiPo Battery) |
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

### Power Connections
| From Component | From Pin | To Component | To Pin | Notes |
| :--- | :---: | :--- | :---: | :--- |
| Solar Cell | `+` | ESP32 | `5V` or `VBUS` | Provides regulated 5V power to the ESP32. |
| Solar Cell | `-` | ESP32 | `GND` | Common ground for solar input. |
| LiPo Battery | `+` | ESP32 | `B+` | Connects to ESP32's internal charger. |
| LiPo Battery | `-` | ESP32 | `B-` | Ground for LiPo battery. |


### Sensor Connections
| ESP32 Pin | Connects To | HX711 Pin | Notes |
| :---: | :---: | :---: | :--- |
| `5V` | --> | `VCC` | Powers the amplifier (from ESP32's regulated 5V). |
| `GND` | --> | `GND` | Common ground reference. |
| `GPIO21` | --> | `DT` | Data line from the amplifier. |
| `GPIO22` | --> | `SCK` | Clock line to the amplifier. |

| HX711 Pin | Connects To | Load Cell Wire |
| :---: | :---: | :---: |
| `E+` | --> | Red |
| `E-` | --> | Black |
| `A+` | --> | Green |
| `A-` | --> | White |
