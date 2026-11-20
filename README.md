# Waveshare ESP32-S3-Touch-AMOLED-1.43 board support

ESP-IDF drivers for the **Waveshare ESP32-S3-Touch-AMOLED-1.43 (non-C)**. Send frames or rectangular updates to the panel, read touch and acceleration, or feed accelerometer samples to the shake detector.

## Use in firmware

Run `python3 tools/setup-board.py` from this repository to fetch the vendor driver files. In an existing ESP-IDF project, include the [board registration file](board/esp-idf.cmake) before ESP-IDF’s project file. Adjust the package path to your checkout.

```cmake
include("${CMAKE_CURRENT_LIST_DIR}/third_party/waveshare-esp32-s3-touch-amoled-1.43-bsp/board/esp-idf.cmake")
include($ENV{IDF_PATH}/tools/cmake/project.cmake)
```

Add `board` to your component’s `PRIV_REQUIRES`. Your project supplies its own target, partition table and PSRAM configuration. The tested firmware used ESP-IDF 5.3.2 with an ESP32-S3 at 240 MHz, octal PSRAM at 80 MHz and 16 MiB flash.

- **Panel.** Initialize it once and draw from one task. `display_frame_region` takes a rectangle inside a full 233 × 233 premultiplied ARGB8888 frame. It rotates the image clockwise, doubles each pixel and converts it to RGB565. `display_rgb565_region` takes a tightly packed native-resolution patch in upright artwork coordinates. [Display API](board/display.h)
- **Streaming.** For procedural images, `display_scanlines` and `display_parallel_rows` take physical panel coordinates and expect RGB565 in wire byte order. The latter splits a tile across the two cores and overlaps painting with the preceding transfer. [Streaming API](board/display.h)
- **Input.** Initialize the shared I²C bus, then enable the FT3168 touch controller or QMI8658 accelerometer. Touch arrives in upright 466 × 466 artwork coordinates. Divide by two to map touch to a half-resolution framebuffer. Acceleration uses screen-relative axes in g. The standalone shake detector takes samples at 100 Hz. [Sensor API](board/sensors.h) · [Shake detector](board/shake.h)

The LCD probe selects the SH8601 command table for ID `0x86`. Other values select the CO5300 sequence and a six-column X offset, following Waveshare’s non-C example. Board pin assignments and the vendor code come from the [Waveshare resources](https://docs.waveshare.com/ESP32-S3-Touch-AMOLED-1.43/Resources-And-Documents).

## Test on a computer

The host tests exercise changed-region transfers and shake detection with CMake and a C11 compiler. The panel and sensor drivers are built in an ESP-IDF application.

```sh
cmake -S . -B build
cmake --build build
ctest --test-dir build --output-on-failure
```

The setup script accepts a local Waveshare V3 ZIP as its optional first argument and checks its SHA-256 digest before extracting the board files. [License](LICENSE) · [Vendor sources](THIRD_PARTY.md)
