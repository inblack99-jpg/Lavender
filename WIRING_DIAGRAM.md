# Project Lavender: Wiring Diagram

This document outlines the pin connections between the ESP32, the HX711 Amplifier, the Load Cell, and the power system components. Please ensure your device is **unplugged** from power while making these connections.

## Component Pinouts

### Power System
- **Solar Cell:** Provides charging power.
  - `+`: Positive Output
  - `-`: Negative Output
- **TP4056 LiPo Charger Module:** Manages charging and provides a regulated output.
  - `IN+`, `IN-`: Input from Solar Cell
  - `B+`, `B-`: Connection to LiPo Battery
  - `OUT+`, `OUT-`: 5V Output to the ESP32
- **3.7V LiPo Battery:** Stores power for the system.
  - `+`: Positive Terminal
  - `-`: Negative Terminal

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
+------------+     +------------------------+     +------------------+                    +-----------------+
| Solar Cell |---->| TP4056 Charger         |---->| ESP32 Super Mini |                    | HX711 Amplifier |
|            |     |                        |     |                  |                    |                 |
| [+]      [+]--->| IN+              OUT+  |---->| 5V               |                    |                 |
| [-]      [-]--->| IN-              OUT-  |---->| GND              |------------------->| GND             |
|            |     |                        |     |                  |                    |                 |
+------------+     | B+                 B-  |     |           GPIO22 |<------------------>| SCK             |
                   | |                  |   |     |           GPIO21 |<------------------>| DT              |
                   | +------------------+   |     |                  |                    |                 |
                   | |                      |     +------------------+                    +-----------------+
                   | | (LiPo Battery)       |                                                 | | | |
                   +-+----------------------+                                                 | | | |
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
| Solar Cell | `+` | TP4056 | `IN+` | Provides charging power. |
| Solar Cell | `-` | TP4056 | `IN-` | Ground for charger input. |
| LiPo Battery | `+` | TP4056 | `B+` | Connects battery for charging. |
| LiPo Battery | `-` | TP4056 | `B-` | Ground for battery. |
| TP4056 | `OUT+` | ESP32 | `5V` | Powers the ESP32. |
| TP4056 | `OUT-` | ESP32 | `GND` | Common ground reference for ESP32. |


### Sensor Connections
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
