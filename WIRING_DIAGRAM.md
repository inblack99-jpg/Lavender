# Lavender Scale Wiring Diagram

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
