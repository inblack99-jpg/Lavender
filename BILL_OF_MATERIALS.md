# Project Lavender: Bill of Materials (BOM)

This document lists all the necessary hardware components for building the smart lavender scale. Example links are provided for reference, but you can source these components from any reputable electronics supplier.

| Quantity | Component | Description | Notes / Example Link |
| :---: | :--- | :--- | :--- |
| 1 | ESP32 Development Board | The microcontroller that will run our code. The "ESP32S Super Mini" is specified, but any ESP32 board with at least 4 available GPIO pins will work. | [Example on Amazon](https://www.amazon.com/dp/B086MLNH7N) |
| 1 | HX711 Load Cell Amplifier | A breakout board that reads the data from the load cell and communicates it to the ESP32. The "Keyes 234" board is a common variant. | [Example on Amazon](https://www.amazon.com/dp/B07Y52626G) |
| 1 | Straight Bar Load Cell (5kg) | The sensor that measures weight. A 5kg capacity is more than enough for a potted plant and provides good resolution for this use case. | [Example on Amazon](https://www.amazon.com/dp/B075595F4X) |
| 1 | Single Cell Lithium Battery | To power the device. A standard 3.7V LiPo or Li-Ion battery with a JST connector is recommended. Capacity of ~1200mAh is a good starting point. | [Example on Adafruit](https://www.adafruit.com/product/258) |
| 1 | Solar Cell | To recharge the battery. A 5.5V or 6V cell is needed to provide a high enough voltage to charge a 3.7V battery. The ESP32 board must have a compatible charging circuit. | [Example on Amazon](https://www.amazon.com/dp/B0CF5H9Y4X) <!-- This component was already listed, formally addressing issue #5 --> |
| - | Plexiglass/Acrylic Sheet | To build the physical frame for the scale. Dimensions depend on your plant pot size. 1/4" thickness is a good starting point for rigidity. | Available at local hardware stores or online plastic suppliers. |
| - | Jumper Wires | For connecting the electronic components. A standard male-to-female or male-to-male jumper wire kit is useful. | [Example on Amazon](https://www.amazon.com/dp/B01EV70C78) |
| - | M4/M5 Screws & Spacers | To mount the load cell to the plexiglass frame. The exact size will depend on the load cell you purchase. | Available at local hardware stores. |

---

### Important Notes:

*   **ESP32 Charging Circuit:** Please ensure the specific ESP32 board you purchase has a built-in charge controller for single-cell lithium batteries if you plan to use the solar cell. The "ESP32S Super Mini" often includes this.
*   **Physical Assembly:** The plexiglass frame will require cutting and drilling to mount the load cell and create the scale platform.
