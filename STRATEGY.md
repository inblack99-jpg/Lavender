# Project Lavender: Development Strategy

## Overview

This document outlines the strategic approach Gemini CLI will take to deliver the Project Lavender requirements. The goal is to demonstrate effective project management and technical execution, fulfilling the role of a developer lead and product manager. The project will be managed in distinct, sequential phases to ensure clarity and successful delivery.

## Phase 1: Foundational Documentation & Hardware Definition

This phase focuses on establishing a clear plan and providing the necessary information for you to procure and assemble the hardware.

1.  **Bill of Materials (BOM) Generation:**
    *   **Action:** I will research and compile a detailed list of all required hardware components (ESP32, HX711, load cell, etc.).
    *   **Details:** The BOM will include component names, specifications, and example purchase links where possible.
    *   **Output:** A `BILL_OF_MATERIALS.md` file.

2.  **Wiring Diagram Creation:**
    *   **Action:** I will generate a clear, easy-to-follow wiring diagram.
    *   **Details:** The diagram will illustrate the exact pin connections between the ESP32, the HX711 amplifier, and the load cell. I will use a simple, text-based format for clarity and version control.
    *   **Output:** A `WIRING_DIAGRAM.md` file.

## Phase 2: Software & Configuration Development

With the hardware defined, this phase focuses on creating the software and configuration needed to bring the device to life.

1.  **ESPHome Configuration:**
    *   **Action:** I will write the complete YAML configuration file for ESPHome.
    *   **Details:** The file will include sections for:
        *   Wi-Fi connectivity.
        *   Sensor definition for the HX711 load cell.
        *   Initial calibration parameters (which you will later adjust).
        *   A template sensor to convert the raw reading to grams.
        *   Deep sleep configuration to conserve power.
        *   API connection to Home Assistant.
    *   **Output:** An `esphome/lavender-scale.yaml` file.

## Phase 3: Integration & Automation

This phase focuses on integrating the data into Home Assistant and creating the desired user-facing alerts.

1.  **Home Assistant Configuration Instructions:**
    *   **Action:** I will provide detailed, step-by-step instructions for configuring Home Assistant.
    *   **Details:** This will cover:
        *   How to use the sensor data from the ESP32.
        *   Creating an automation to check the weight against a threshold.
        *   Setting up the time-based conditions (8 AM - 10 PM).
        *   Using the Alexa Media Player integration (via HACS) to trigger the voice announcement on your Echo device.
    *   **Output:** An `HOME_ASSISTANT_SETUP.md` file.

## Project Management & Communication

-   **Version Control:** All artifacts will be committed to this GitHub repository. I will create a separate commit for each major deliverable.
-   **Communication:** I will clearly state what I am working on at each step and provide the results for your review. I will rely on your feedback, especially during the physical assembly and calibration stages.
-   **File Structure:** I will maintain a clean and logical file structure within the repository.

By following this strategy, we can ensure all requirements are met systematically and the project is a successful proof of concept.
