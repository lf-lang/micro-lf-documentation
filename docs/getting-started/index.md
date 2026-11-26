# Getting Started

This guide sets up micro-LF, runs a first program on your computer, and then
points you to the instructions for your hardware platform.

## Prerequisites

- **Linux** or **macOS**. On Windows, use WSL: see [Windows (WSL)](wsl.md).
- **Git**: [git-scm.com](https://git-scm.com/)
- **Java 17**: [openjdk.org](https://openjdk.org/projects/jdk/17/) (for the micro-LF compiler)
- **A C compiler** (e.g. `gcc` or `clang`) and **CMake 3.20+**

Each [platform](../platforms/index.md) page lists the additional tools it needs.

## 1. Set Up micro-LF

The micro-LF compiler and runtime live in the
[reactor-uc](https://github.com/lf-lang/reactor-uc) repository. There is no
separate installer: clone the repository and set `REACTOR_UC_PATH` to its root.

```bash
git clone https://github.com/lf-lang/reactor-uc.git --recurse-submodules
export REACTOR_UC_PATH=$(pwd)/reactor-uc
```

Add the `export` line to your shell profile (e.g. `~/.bashrc` or `~/.zshrc`) so
that `REACTOR_UC_PATH` is set in every terminal.

!!! note "`ulfc` and `ulfc-dev`"
    The micro-LF compiler is called `ulfc`. You run it with the script
    `ulf/bin/ulfc-dev` in the reactor-uc repository, which builds the compiler
    from source (using Java 17) and then runs it, so the first run takes a few
    minutes. The platform templates call this script for you.

## 2. Run Your First Program

Create a file `HelloWorld.ulf`:

```lf
@platform("Native")
main reactor {
  reaction(startup) {=
    printf("Hello World!\n");
  =}
}
```

Compile and run it:

```bash
$REACTOR_UC_PATH/ulf/bin/ulfc-dev HelloWorld.ulf
bin/HelloWorld
```

`@platform("Native")` tells the compiler to build an executable for your
computer. See [POSIX (Linux / macOS)](../platforms/posix/index.md) for more.

## 3. Choose Your Platform

To run micro-LF on a microcontroller, start from the template for your
platform. Each platform page explains how to set up the template, build, flash,
and change the application.

<div class="grid cards" markdown>

-   ![Zephyr](../assets/logos/zephyr.svg){: style="height:48px"}

    ---

    **Zephyr** · West

    [:octicons-arrow-right-24: Zephyr](../platforms/zephyr/index.md)

-   ![RIOT](../assets/logos/RIOT.png){: style="height:48px"}

    ---

    **RIOT** · Make

    [:octicons-arrow-right-24: RIOT](../platforms/riot/index.md)

-   ![Pico](../assets/logos/pico.svg){: style="height:48px"}

    ---

    **Raspberry Pi Pico** · CMake

    [:octicons-arrow-right-24: Raspberry Pi Pico](../platforms/pico/index.md)

-   ![FreeRTOS](../assets/logos/freertos.svg){: style="height:48px"}

    ---

    **FreeRTOS** · CMake

    [:octicons-arrow-right-24: FreeRTOS](../platforms/freertos/index.md)

-   ![Espressif](../assets/logos/espressif.svg#only-light){: style="height:48px"}
    ![Espressif](../assets/logos/espressif_white.svg#only-dark){: style="height:48px"}

    ---

    **ESP-IDF (ESP32)** · CMake

    [:octicons-arrow-right-24: ESP-IDF](../platforms/esp-idf/index.md)

-   :material-chip:{ style="font-size: 48px" }

    ---

    **Patmos** · Make

    [:octicons-arrow-right-24: Patmos](../platforms/patmos/index.md)

-   :fontawesome-brands-linux:{ style="font-size: 48px" }

    ---

    **POSIX (Linux / macOS)** · CMake

    [:octicons-arrow-right-24: POSIX](../platforms/posix/index.md)

</div>

## A Closer Look at a Reactor

Each template includes example applications. Here is the structure of a simple
blinking LED program:

```lf
main reactor {
  timer t(0, 500 ms)
  state led_on: bool = false

  reaction(startup) {=
    // Initialize LED GPIO
  =}

  reaction(t) {=
    self->led_on = !self->led_on;
    // Toggle LED based on self->led_on
  =}
}
```

This reactor:

1. Declares a **timer** `t` that fires immediately (`0`) and repeats every `500 ms`
2. Maintains **state** `led_on` to track the LED status
3. Has a **reaction** to `startup` for initialization
4. Has a **reaction** to timer `t` that toggles the LED

## Next Steps

<div class="grid cards" markdown>

-   **Platforms**

    ---

    Setup, build, and flashing instructions for each supported platform.

    [:octicons-arrow-right-24: Platforms](../platforms/index.md)

-   **Philosophy**

    ---

    Understand the reactor model, logical time, and deterministic concurrency.

    [:octicons-arrow-right-24: Philosophy](../design/index.md)

-   **Documentation**

    ---

    API reference, annotations, and compile flags.

    [:octicons-arrow-right-24: Documentation](../documentation/annotations.md)

</div>
