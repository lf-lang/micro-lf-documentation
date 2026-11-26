# Patmos

- **Template**: [ulf-patmos-template](https://github.com/lf-lang/ulf-patmos-template)
- **Patmos Documentation**: [github.com/t-crest/patmos](https://github.com/t-crest/patmos)
- **Supported Hardware**: the Patmos processor, in the `pasim` simulator or on an FPGA

Patmos is a time-predictable processor designed for hard real-time systems and worst-case execution time (WCET) analysis. The Patmos template builds micro-LF applications with `make` and the Patmos compiler.

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites):

- **Patmos toolchain** (`patmos-clang` and the `pasim` simulator): follow the instructions at [github.com/t-crest/patmos](https://github.com/t-crest/patmos)

## Setup

Clone the template:

```bash
git clone https://github.com/lf-lang/ulf-patmos-template.git my-project
cd my-project
```

## Building

```bash
make all
```

This builds the default application, `Smoke`, into the executable `Smoke`.

## Running

Run the program in the Patmos simulator:

```bash
pasim Smoke
```

To run on an FPGA, see the [Patmos documentation](https://github.com/t-crest/patmos).

## Changing the Application

Set `LF_MAIN` in the `Makefile`, or on the command line:

```bash
make LF_MAIN=MyApp all
pasim MyApp
```

## Log Level

The template's `Makefile` does not set a log level. To change it, add e.g. `-DLF_LOG_LEVEL_ALL=LF_LOG_LEVEL_DEBUG` to the `patmos-clang` command in the `Makefile`.

## Cleaning

```bash
rm -rf src-gen Smoke
```

## Network Channels

| Channel | Description |
|---------|-------------|
| S4NoC | Network-on-chip between Patmos cores (`@interface_s4noc`) |

## Notes

- The older [lf-patmos-template](https://github.com/lf-lang/lf-patmos-template) targets the Lingua Franca C target (reactor-c), not micro-LF. It uses a static scheduler and includes WCET analysis with Platin.
