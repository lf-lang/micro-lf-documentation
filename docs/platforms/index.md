# Platforms

micro-LF supports multiple embedded platforms through dedicated **template repositories**. Each template provides a pre-configured project structure with build system integration, example applications, and platform-specific setup.

## Template Repositories

| Platform | Template | Build System |
|----------|----------|--------------|
| [Zephyr](zephyr/index.md) | [ulf-zephyr-template](https://github.com/lf-lang/ulf-zephyr-template) | West |
| [RIOT](riot/index.md) | [ulf-riot-template](https://github.com/lf-lang/ulf-riot-template) | Make |
| [Raspberry Pi Pico](pico/index.md) | [ulf-pico-template](https://github.com/lf-lang/ulf-pico-template) | CMake |
| [FreeRTOS](freertos/index.md) | [ulf-freertos-template](https://github.com/lf-lang/ulf-freertos-template) | CMake |
| [ESP-IDF](esp-idf/index.md) | [ulf-esp-idf-template](https://github.com/lf-lang/ulf-esp-idf-template) | CMake |
| [Patmos](patmos/index.md) | [ulf-patmos-template](https://github.com/lf-lang/ulf-patmos-template) | Make |
| [POSIX (Linux / macOS)](posix/index.md) | [ulf-posix-template](https://github.com/lf-lang/ulf-posix-template) (optional) | CMake |

To start a project, create a repository from the template on GitHub (**Use this template**) or clone the template directly, then follow its platform page.

## Common Prerequisites

All platforms require:

- **Linux** or **macOS**. On Windows, use WSL: see [Windows (WSL)](../getting-started/wsl.md).
- **Git**
- **Java 17** for the micro-LF compiler
- **micro-LF** cloned, with the `REACTOR_UC_PATH` environment variable pointing to it:

```bash
git clone https://github.com/lf-lang/reactor-uc.git --recurse-submodules
export REACTOR_UC_PATH=$(pwd)/reactor-uc
```

See [Getting Started](../getting-started/index.md#1-set-up-micro-lf) for details.
