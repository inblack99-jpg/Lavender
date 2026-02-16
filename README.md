# Project Lavender

## Project Overview

This project is to generate all of the necessary hardware and code for a project that will use a load cell to measure the weight of a lavender plant in grams and integrate that information into an instance of home assistant.

This is a proof of concept for Gemini CLI to operate as a developer Lead and product manager.

## Hardware Changes

The status LEDs (both the red and the color LED) have been disabled in the ESPHome configuration. This was done to reduce power consumption and to make the device less conspicuous. The LEDs can be re-enabled by adding the following code to the `esphome/lavender-scale.yaml` file:

```yaml
light:
  - platform: esp32_rmt_led_strip
    id: status_led
    name: "Internal LED"
    pin: GPIO48
    chipset: WS2812
    num_leds: 1
    rgb_order: GRB
    default_transition_length: 500ms
```

and

```yaml
esphome:
  on_boot:
    priority: 600
    then:
      - light.turn_on:
          id: status_led
          red: 20%
          green: 0%
          blue: 0%
          brightness: 25%
```
