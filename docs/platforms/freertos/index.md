# FreeRTOS

- **Template**: [ulf-freertos-template](https://github.com/lf-lang/ulf-freertos-template)
- **FreeRTOS Documentation**: [freertos.org](https://www.freertos.org/Documentation/02-Kernel/07-Books-and-manual/01-RTOS_book)
- **Supported Boards**: Raspberry Pi Pico (RP2040)

The FreeRTOS template runs micro-LF applications on the FreeRTOS kernel. Its hardware support is organized per platform; it currently supports the Raspberry Pi Pico (RP2040).

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites):

- **CMake 3.20+**
- **ARM cross-compiler**: [Install ARM Cross-Compiler](https://www.lf-lang.org/embedded-lab/Non-Nix.html#install-cmake-standard-c-library-arm-cross-compiler)
- **picotool**: [Install Picotool](https://www.lf-lang.org/embedded-lab/Non-Nix.html#install-picotool)
- **Pico SDK**: the template does not include it, so point `PICO_SDK_PATH` to a [pico-sdk](https://github.com/raspberrypi/pico-sdk) checkout:

```bash
export PICO_SDK_PATH=/path/to/pico-sdk
```

## Setup

Clone the template and initialize the FreeRTOS submodule:

```bash
git clone https://github.com/lf-lang/ulf-freertos-template.git my-project
cd my-project
git submodule update --init --recursive
```

## Building

Select the hardware platform with `PLATFORM_TARGET` (required):

```bash
cmake -Bbuild -DPLATFORM_TARGET=pico
cmake --build build
```

This builds the default application, `HelloFreeRTOS`, into `build/HelloFreeRTOS.elf`. For a Pico W, also pass `-DPICO_BOARD=pico_w`.

## Flashing

Put the Pico into BOOTSEL mode (hold the BOOTSEL button while connecting USB), then:

```bash
picotool load -x build/HelloFreeRTOS.elf
```

The program's output is sent over USB serial and over UART (GPIO 0 and 1). On Windows, first attach the board to WSL: see [Windows (WSL)](../../getting-started/wsl.md).

## Changing the Application

Set `LF_MAIN` in `CMakeLists.txt`, or on the command line:

```bash
cmake -Bbuild -DPLATFORM_TARGET=pico -DLF_MAIN=Blink
cmake --build build
```

## Log Level

The default is `LF_LOG_LEVEL_WARN`:

```bash
cmake -Bbuild -DPLATFORM_TARGET=pico -DLOG_LEVEL=LF_LOG_LEVEL_DEBUG
```

## Cleaning

```bash
rm -rf src-gen build
```

## Network Channels

No network channels are available on FreeRTOS yet, so federated programs are not supported on this platform.

## Notes

- **Timer precision**: the Pico's `FreeRTOSConfig.h` sets a 100 Hz tick, so timers are precise to 10 ms. Increase `configTICK_RATE_HZ` for finer timing.
- **Other hardware**: each platform lives in `platforms/<name>/` with its own `platform.cmake` and `FreeRTOSConfig.h`; see the template's README to add one.
