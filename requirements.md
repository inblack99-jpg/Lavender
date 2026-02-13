# Project Lavender Requirements

## Project Overview

This project is to generate all of the necessary hardware and code for a project that will use a load cell to measure the weight of a lavender plant in grams and integrate that information into an instance of home assistant.

This is a proof of concept for Gemini CLI to operate as a developer Lead and product manager.

## Hardware Requirements

- Plexiglass frame for the scale
- Load cell
- Amplifier board Keyes 234 (HX711) Amplifier
- ESP 32S super mini with battery pads
- Single cell lithium battery
- 5.5 V 130 mA 72 mm x 72 mm solar cell
- Home server running ESPHome and Home Assistant

## Software Requirements

- ESP32 code to read the weight of the lavender plant and report it to the Home Assistant instance.
- Connect to Wi-fI.
- Calibrate the scale.
- Weigh the lavender plant.
- Report the weight in grams to Home Assistant.
- Deep sleep for 10 minutes.
- Ensure that the code can be debugged and updated via ESPHome.

## Integration

- Integrate with Home Assistant to set parameters for identifying when the plant needs watering.
- Utilize the HACS interface to the Amazon Echo to announce that the plant needs water.
- This announcement should only happen every 10 minutes between 8 AM and 10 PM.
- Otherwise, it should be muted.

## Documentation

- All code
- Bill of Materials (BOM)
- Wiring diagrams
- Integration instructions

## Storage
This is a GitHub project.
- All project artifacts, including requirements documents and code segments, should be stored in a GitHub project.

## Roles and Responsibilities

### Gemini CLI (My Responsibilities)
- Generate a Bill of Materials (BOM) for all hardware components.
- Create a wiring diagram for connecting the ESP32, HX711, and load cell.
- Write the ESPHome YAML configuration file for the ESP32.
- Provide step-by-step instructions for configuring Home Assistant for alerts and notifications.
- Structure all generated files in a logical project folder.

### User (Your Responsibilities)
- **Procurement:** Purchase all components listed in the BOM.
- **Physical Assembly:** Build the plexiglass frame and assemble the scale.
- **Wiring:** Connect all electronic components according to the provided wiring diagram.
- **System Setup:** 
    - Ensure your home server is running Home Assistant and ESPHome.
    - Install and configure any required Home Assistant integrations (like HACS and Alexa Media Player).
    - Create a GitHub repository for the project.
- **Deployment:** Add the generated configuration to your ESPHome instance and install it on the ESP32 device.
- **Calibration:** Perform the physical calibration of the scale by placing a known weight on it and providing the resulting value.
- **Testing & Feedback:** Test the complete system and provide feedback for any necessary adjustments.
