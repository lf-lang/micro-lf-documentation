# Raspberry Pi Pico

- **Template**: [ulf-pico-template](https://github.com/lf-lang/ulf-pico-template)
- **Pico SDK Documentation**: [raspberrypi.com/documentation/pico-sdk](https://www.raspberrypi.com/documentation/pico-sdk/)
- **Supported Boards**: [Pico Series](https://www.raspberrypi.com/documentation/microcontrollers/pico-series.html)

The Pico template builds micro-LF applications bare-metal with the [Pico SDK](https://github.com/raspberrypi/pico-sdk/), for RP2040 and RP2350 boards such as the Pico, Pico W, and Pico 2.

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites):

- **CMake 3.20+**
- **ARM cross-compiler**: [Install ARM Cross-Compiler](https://www.lf-lang.org/embedded-lab/Non-Nix.html#install-cmake-standard-c-library-arm-cross-compiler)
- **picotool**: [Install Picotool](https://www.lf-lang.org/embedded-lab/Non-Nix.html#install-picotool)

If you use [Nix](https://nixos.org), run `nix develop` in the project directory to get these tools instead.

## Setup

Clone the template and initialize the Pico SDK submodule:

```bash
git clone https://github.com/lf-lang/ulf-pico-template.git my-project
cd my-project
git submodule update --init --recursive
```

## Building

```bash
cmake -Bbuild
cmake --build build
```

This builds the default application, `Blink`, into `build/Blink.elf`. For a board other than the Pico, set `PICO_BOARD` when configuring, e.g. `-DPICO_BOARD=pico2` (RP2350) or `-DPICO_BOARD=pico_w`.

## Flashing

Put the Pico into BOOTSEL mode (hold the BOOTSEL button while connecting USB), then:

```bash
picotool load -x build/Blink.elf
```

The program's output is sent over USB serial and over UART (GPIO 0 and 1). On Windows, first attach the board to WSL: see [Windows (WSL)](../../getting-started/wsl.md).

## Changing the Application

Set `LF_MAIN` in `CMakeLists.txt`, or on the command line:

```bash
cmake -Bbuild -DLF_MAIN=Timer
cmake --build build
```

## Log Level

The default is `LF_LOG_LEVEL_WARN`:

```bash
cmake -Bbuild -DLOG_LEVEL=LF_LOG_LEVEL_DEBUG
```

## Cleaning

```bash
rm -rf src-gen build
```

## Network Channels

| Channel | Description |
|---------|-------------|
| UART | Serial point-to-point link |
