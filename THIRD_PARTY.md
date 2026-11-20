# Third-party sources

The project license covers the original code and documentation. Dependencies retain their own licenses and copyright notices.

| Source | Use in this repository |
| --- | --- |
| [Waveshare board examples](https://docs.waveshare.com/ESP32-S3-Touch-AMOLED-1.43/Resources-And-Documents) | The setup script reads the LCD probe and initialization tables from `07_LVGL_Test` and the I²C component and IMU register definitions from `03_I2C_QMI8658` in the V3 demo ZIP. Files extracted locally under `vendor/waveshare` come from Waveshare. |
| [ESP-IDF v5.3.2](https://github.com/espressif/esp-idf/tree/v5.3.2) and [Espressif's SH8601 driver](https://components.espressif.com/components/espressif/esp_lcd_sh8601) | The board component pins the display driver through the ESP-IDF component manager. |
