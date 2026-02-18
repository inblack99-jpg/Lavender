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
- `GPIO1`: Serial Clock (SCK) for HX711
- `GPIO2`: Data Out (DOUT) for HX711
- `GPIO4`: Battery Voltage Sensing (ADC)
- `GPIO5`: HX711 Power Control (Primary)
- `GPIO11`: HX711 Power Control (Secondary)
- `GPIO7`: SHT3x Power Control
- `GPIO10`: Voltage Divider Ground Control
- `GPIO8`: I2C SDA
- `GPIO9`: I2C SCL

### HX711 Amplifier
- `VCC`: Power Input (connects to controlled GPIO from ESP32)
- `GND`: Ground
- `SCK`: Serial Clock (Output)
- `DT`: Data (Input, often labeled DOUT)
- `E+`, `E-`, `A+`, `A-`: Load Cell connections

### SHT3x Temperature/Humidity Sensor
- `VCC`: Power Input (connects to controlled GPIO from ESP32)
- `GND`: Ground
- `SDA`: I2C Data
- `SCL`: I2C Clock

### Load Cell (4 Wires)
- `Red`: Excitation+ (E+)
- `Black`: Excitation- (E-)
- `White`: Output- (A-)
- `Green`: Output+ (A+)

## Power Management (Deep Sleep Optimization)

To maximize battery life, the system uses GPIO pins to control power to the sensors. These pins are turned ON only when a measurement is needed and turned OFF before entering deep sleep.

| ESP32 Pin | Target Component | Purpose |
| :---: | :--- | :--- |
| `GPIO5` | HX711 (VCC) | Primary power switch for the scale amplifier. |
| `GPIO11` | HX711 (VCC/Aux) | Secondary/Backup power switch for the scale amplifier. |
| `GPIO7` | SHT3x (VCC) | Power switch for the temperature/humidity sensor. |
| `GPIO10` | Voltage Divider | Ground reference switch. Set to LOW to enable reading, HIGH/Float to disable. |

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
| `GPIO5` | --> | `VCC` | Controlled power rail (Primary). |
| `GPIO11` | --> | `VCC` | Controlled power rail (Secondary). |
| `GND` | --> | `GND` | Common ground reference. |
| `GPIO2` | --> | `DT` | Data line from the amplifier. |
| `GPIO1` | --> | `SCK` | Clock line to the amplifier. |

### SHT3x Connections
| ESP32 Pin | Connects To | SHT3x Pin | Notes |
| :---: | :---: | :---: | :--- |
| `GPIO7` | --> | `VCC` | Controlled power rail. |
| `GND` | --> | `GND` | Common ground reference. |
| `GPIO8` | --> | `SDA` | I2C Data line. |
| `GPIO9` | --> | `SCL` | I2C Clock line. |

### Battery Voltage Sensor (with Leakage Protection)
| ESP32 Pin | Connects To | Component | Notes |
| :---: | :---: | :--- | :--- |
| `GPIO4` | --> | Voltage Divider (Middle) | ADC Input. |
| `GPIO10` | --> | Voltage Divider (Bottom) | Ground switch to prevent drain. |

## Battery Voltage Sensor Circuit

```
(Battery +) --- [100kΩ] ---+--- (ESP32 GPIO4)
                           |
                        [100kΩ]
                           |
                     (ESP32 GPIO10)
```
Set `GPIO10` to Output LOW during measurement, and High-Impedance (Input) or HIGH during sleep to stop current flow through the resistors.
