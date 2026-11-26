# ESP-IDF

- **Template**: [ulf-esp-idf-template](https://github.com/lf-lang/ulf-esp-idf-template)
- **ESP-IDF Documentation**: [docs.espressif.com](https://docs.espressif.com/projects/esp-idf/en/latest/)
- **Supported Boards**: ESP32, ESP32-C3, ESP32-C6, ESP32-S2, ESP32-S3, ESP32-H2, and other targets supported by ESP-IDF

The ESP-IDF template builds micro-LF applications for Espressif's ESP32 family with the ESP-IDF framework (v5.5.1, included as a submodule). Any ESP-IDF API, such as GPIO, Wi-Fi, or drivers, can be called directly from reactions.

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites):

- **Python 3.8+**
- **CMake 3.20+**
- **Ninja**
- The [ESP-IDF prerequisites](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#manual-installation) for your system

If you use [Nix](https://nixos.org), run `nix develop` in the project directory instead: it provides all tools, installs the ESP-IDF toolchain and sets its environment variables, so you can skip the toolchain step in [Setup](#setup). For a board other than the ESP32, use e.g. `nix develop .#esp32c6`.

## Setup

Clone the template and initialize its submodules (ESP-IDF and the extra components used by the examples):

```bash
git clone https://github.com/lf-lang/ulf-esp-idf-template.git my-project
cd my-project
git submodule update --init --recursive
```

Install the ESP-IDF toolchain for your board (here the ESP32-C6) and set up its environment:

```bash
cd esp-idf
./install.sh esp32c6
source ./export.sh
cd ..
```

Run `source ./esp-idf/export.sh` again in every new terminal.

## Building

Set your board and configure the project, then build:

```bash
export ESP_BOARD=esp32c6

cmake -Bbuild \
      -DESP_IDF_BOARD=$ESP_BOARD \
      -DCMAKE_TOOLCHAIN_FILE=$IDF_PATH/tools/cmake/toolchain-$ESP_BOARD.cmake \
      -GNinja
cmake --build build
```

This builds the default application, `HelloEsp`.

## Flashing

Flash the board and show the program's output:

```bash
cd build
ninja flash
ninja monitor
```

Press `Ctrl+]` to exit the monitor. If the serial port is not detected, pass it explicitly, e.g. `ninja flash -p /dev/ttyUSB0`. On Windows, first attach the board to WSL: see [Windows (WSL)](../../getting-started/wsl.md).

## Changing the Application

Set `LF_MAIN` in `CMakeLists.txt`, or on the command line:

```bash
cmake -Bbuild \
      -DLF_MAIN=Timer \
      -DESP_IDF_BOARD=$ESP_BOARD \
      -DCMAKE_TOOLCHAIN_FILE=$IDF_PATH/tools/cmake/toolchain-$ESP_BOARD.cmake \
      -GNinja
cmake --build build
```

## Log Level

The default is `LF_LOG_LEVEL_WARN`. Add `-DLOG_LEVEL=LF_LOG_LEVEL_DEBUG` to the configure command above.

## Cleaning

```bash
rm -rf src-gen build
```

## Network Channels

No network channels are available on ESP-IDF yet, so federated programs are not supported on this platform.

## Notes

- **Timer precision**: the template's `sdkconfig` sets a 100 Hz FreeRTOS tick (`CONFIG_FREERTOS_HZ=100`), so timers are precise to 10 ms. Increase it (e.g. to `1000`) for finer timing.
- **Blink example**: the `Led` reactor drives an addressable LED on GPIO 8, as on the ESP32-C6 and ESP32-C3 DevKitC boards. For other boards, change `BLINK_GPIO` in `src/lib/Led.ulf`.
- **More ESP-IDF components**: add them to `IDF_COMPONENTS` in `CMakeLists.txt` and link them to the application target.
