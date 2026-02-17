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
- `GPIO1`: General Purpose Input/Output Pin (for SCK)
- `GPIO2`: General Purpose Input/Output Pin (for DOUT)
- `GPIO4`: General Purpose Input/Output Pin (for Battery Voltage Sensing)


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
                                        |           GPIO1 |<------------------>| SCK             |
                                        |           GPIO2 |<------------------>| DT              |
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
| Solar Cell | `+` | ESP32 | `5V` or `VBUS` | Provides regulated 5V to charge battery. |
| Solar Cell | `-` | ESP32 | `GND` | Common ground reference. |
| LiPo Battery | `+` | ESP32 | `B+` | Powers the ESP32. |
| LiPo Battery | `-` | ESP32 | `B-` | Common ground reference. |

### HX711 Connections
| ESP32 Pin | Connects To | HX711 Pin | Notes |
| :---: | :---: | :---: | :--- |
| `5V` | --> | `VCC` | Powers the amplifier. |
| `GND` | --> | `GND` | Common ground reference. |
| `GPIO2` | --> | `DT` | Data line from the amplifier. |
| `GPIO1` | --> | `SCK` | Clock line to the amplifier. |

### Load Cell Connections
| HX711 Pin | Connects To | Load Cell Wire |
| :---: | :---: | :---: |
| `E+` | --> | Red |
| `E-` | --> | Black |
| `A+` | --> | Green |
| `A-` | --> | White |

## Battery Voltage Sensor

To accurately measure the LiPo battery's voltage, a voltage divider is required. This circuit scales the battery's voltage (up to 4.2V) down to a level that is safe for the ESP32's ADC (Analog-to-Digital Converter), which can only handle up to 3.3V.

### Circuit

The circuit uses two identical resistors to create a 1:2 divider. This means the voltage read by the ESP32 at `GPIO4` will be exactly half of the actual battery voltage.

```
(Battery +) --- [R1] ---+--- (ESP32 GPIO4)
                        |
                       [R2]
                        |
                       GND
```

### Parts Required

*   **R1:** 100kΩ Resistor
*   **R2:** 100kΩ Resistor

### Connections

1.  Connect the **positive terminal of the LiPo battery** to one end of the first resistor (`R1`).
2.  Connect the other end of `R1` to both **`GPIO4` on the ESP32** and one end of the second resistor (`R2`).
3.  Connect the other end of `R2` to **`GND`** on the ESP32.
