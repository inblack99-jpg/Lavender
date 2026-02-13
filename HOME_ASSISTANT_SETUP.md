# Project Lavender: Home Assistant Setup Instructions

This document provides step-by-step instructions for configuring Home Assistant to integrate with your ESPHome-based Lavender Scale. This includes setting up the sensor, creating an automation for watering alerts, and configuring time-based conditions.

## Prerequisites

Before you begin, ensure you have:

1.  **Home Assistant Instance:** A running Home Assistant instance.
2.  **ESPHome Integration:** The ESPHome integration configured in Home Assistant, and your `lavender-scale.yaml` device (ESP32) is added and online.
3.  **HACS (Home Assistant Community Store):** HACS installed in your Home Assistant instance.
4.  **Alexa Media Player Integration:** The "Alexa Media Player" integration installed via HACS and configured to connect to your Amazon Echo device(s).

## Step 1: Verify the ESPHome Sensor in Home Assistant

Once your ESP32 device running the `lavender-scale.yaml` configuration is connected to your network and ESPHome, Home Assistant should automatically discover the sensors.

1.  In Home Assistant, navigate to **Settings > Devices & Services**.
2.  Go to the **ESPHome** integration and click on your "Lavender Scale" device.
3.  You should see an entity named `sensor.lavender_scale_grams` (or similar, depending on your ESPHome configuration's `friendly_name`). This sensor will report the weight of your lavender plant in grams.

    *If you don't see the sensor, ensure your ESP32 is powered on, connected to Wi-Fi, and the ESPHome integration in Home Assistant is healthy.*

## Step 2: Create a Helper for the Watering Threshold

It's good practice to use a Home Assistant Helper to manage the watering threshold, making it easy to adjust without editing automations directly.

1.  Navigate to **Settings > Devices & Services > Helpers**.
2.  Click **+ CREATE HELPER**.
3.  Select **Number**.
4.  Configure the helper:
    *   **Name:** `Lavender Watering Threshold`
    *   **Icon (Optional):** `mdi:water-alert`
    *   **Minimum value:** `0`
    *   **Maximum value:** `5000` (or appropriate for your plant/pot weight)
    *   **Step:** `10`
    *   **Unit of measurement (Optional):** `g`
5.  Click **CREATE**.

    *Note the entity ID, likely `number.lavender_watering_threshold`.*

## Step 3: Create the Watering Automation

This automation will trigger an Alexa announcement when the lavender plant's weight falls below the defined threshold, within specific hours.

1.  Navigate to **Settings > Automations & Scenes > Automations**.
2.  Click **+ CREATE AUTOMATION**.
3.  Select **Start with an empty automation**.
4.  Give your automation a **Name:** `Announce Lavender Watering Needed`

### Triggers

Add two triggers:

1.  **Trigger Type: State**
    *   **Entity:** `sensor.lavender_scale_grams`
    *   **Below:** (Leave empty, we'll use a condition for the threshold)
    *   **For:** `00:05:00` (Trigger after the weight is below the threshold for 5 minutes to avoid false positives)

2.  **Trigger Type: Time Pattern**
    *   **Minutes:** `/10` (Triggers every 10 minutes. This will re-check the condition and allow repeated announcements within the time window if the plant still needs water and the first trigger didn't catch it for some reason, or if the user missed it.)

### Conditions

Add three conditions:

1.  **Condition Type: Numeric State**
    *   **Entity:** `sensor.lavender_scale_grams`
    *   **Below:** `{{ states('number.lavender_watering_threshold') | float }}` (This pulls the value from your helper)

2.  **Condition Type: Time**
    *   **After:** `08:00:00` (8 AM)
    *   **Before:** `22:00:00` (10 PM)

3.  **Condition Type: Template**
    *   **Value Template:**
        ```yaml
        {{ (as_timestamp(now()) - as_timestamp(state_attr('automation.announce_lavender_watering_needed', 'last_triggered') | default(0)) | int) > (10 * 60) }}
        ```
        *This condition ensures the automation only triggers if it hasn't triggered in the last 10 minutes, preventing excessive announcements.* (Note: Replace `automation.announce_lavender_watering_needed` with the actual entity ID of your automation if you named it differently).

### Actions

Add an action:

1.  **Action Type: Call service**
    *   **Service:** `media_player.speak`
    *   **Target:** Select your Alexa device (e.g., `media_player.your_echo_dot`)
    *   **Message:** `Your lavender plant needs water. Its current weight is {{ states('sensor.lavender_scale_grams') | round(1) }} grams.`

5.  Click **SAVE**.

## Step 4: Initial Calibration (Physical)

Once your hardware is assembled and the ESP32 is flashing the ESPHome code, you'll need to perform a physical calibration.

1.  **Access ESPHome Logs:** In the ESPHome dashboard (or Home Assistant's ESPHome integration), go to your "Lavender Scale" device and open the logs.
2.  **Note Raw Value (Empty):** With nothing on the scale, note the `Lavender Scale Raw` sensor value from the logs.
3.  **Place Known Weight:** Place a known weight (e.g., 500g, 1000g) on the scale. Note this exact weight.
4.  **Note Raw Value (With Weight):** Note the `Lavender Scale Raw` sensor value again from the logs.
5.  **Calculate Calibration Factor:**
    *   `calibration_factor = (raw_value_with_weight - raw_value_empty) / known_weight_in_grams`
6.  **Update `lavender-scale.yaml`:**
    *   Edit your `lavender-scale.yaml` file (e.g., via the ESPHome dashboard or your file editor).
    *   Find the `lambda` section for `lavender_scale_grams`.
    *   Replace `400.0` with your calculated `calibration_factor`.
        ```yaml
        lambda: |-
          return id(lavender_scale_raw).state / YOUR_CALIBRATION_FACTOR;
        ```
7.  **Upload New Configuration:** Save the file and upload the new configuration to your ESP32.

Your scale should now report accurate weights in Home Assistant!
