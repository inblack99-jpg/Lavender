# Running ESPHome with Docker Compose

This document explains how to run ESPHome as a separate container using the provided `docker-compose.yaml` file. This setup is ideal when running Home Assistant in a separate Docker container.

## Prerequisites

-   Docker and Docker Compose installed on your system.
-   Your Home Assistant container is running and accessible on the network.

## Setup and Usage

1.  **Place Configuration:** Ensure your `lavender-scale.yaml` file is inside the `esphome` directory, which is in the same directory as the `docker-compose.yaml` file.

2.  **Start the Container:** From the root of the project directory, run the following command:
    ```bash
    docker-compose up -d
    ```
    This will start the ESPHome container in detached mode.

3.  **Access ESPHome Dashboard:** You can now access the ESPHome dashboard by navigating to `http://<your-docker-host-ip>:6052` in your web browser.

4.  **Adopt the Device:**
    -   Once you have compiled and uploaded the configuration to your ESP32 device for the first time, ESPHome will see the new device on the network.
    -   You may need to manually add it by clicking "New Device" and then "Adopt" and providing the device's IP address.

5.  **Connect to Home Assistant:**
    -   In your Home Assistant instance, go to **Settings > Devices & Services**.
    -   The ESPHome integration should automatically discover the Lavender Scale device.
    -   If not, you can add it manually by clicking "Add Integration", searching for "ESPHome", and entering the IP address of the ESP32 device.

## File Structure

```
Lavender/
├── docker-compose.yaml
├── esphome/
│   └── lavender-scale.yaml
└── docker/
    └── README.md
```
