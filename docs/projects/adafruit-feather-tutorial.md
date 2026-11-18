# micro-LF Adafruit Feather Tutorial

A hands-on tutorial for writing reactor programs, and finally a federated program, on the Adafruit Feather Sense using micro-LF and RIOT OS.

## What you will learn

- Set up and build for the **Adafruit Feather Sense** (nRF52840-based board)
- Write and compose reactors, and build and flash them with RIOT's `make`
- Read the on-board **LSM6DS33** accelerometer and gyroscope over I2C
- Use [annotations](../documentation/annotations.md) such as `@timeout`, `@max_pending_events` and `@buffer`
- Write a **federated program** where two boards communicate over **CoAP** (IPv6 over IEEE 802.15.4)

## Setup

The tutorial uses RIOT OS as the underlying RTOS. The micro-LF runtime is integrated as a RIOT module, so the standard RIOT build system (`make`) handles compilation and flashing. Set up micro-LF as described in [Getting Started](../getting-started/index.md#1-clone-micro-lf), then clone the tutorial and its RIOT submodule:

```bash
git clone --recurse-submodules https://github.com/lf-lang/micro-lf-tutorial
cd micro-lf-tutorial
```

The repository's README walks through each part step by step. The exercises are in `src/` and the solutions in `reference/`. The exercises are designed to be completed in order, but you can also jump to any part.

## Runtime Configuration

micro-LF programs (`.ulf` files) have no `target` block. The board is selected by
the RIOT build in the tutorial's `Makefile`:

```makefile
BOARD ?= adafruit-feather-nrf52840-sense
```

Runtime options are set with [annotations](../documentation/annotations.md) on the
main or federated reactor, for example:

```lf
@logging("WARN")
federated reactor {
    ...
}
```

Build, flash and open a serial terminal with:

```bash
make LF_MAIN=HelloUc all flash term
```

## Reading the Sensor

The `LSM6DS33` reactor talks to the sensor over I2C with RIOT's `periph_i2c` driver. At startup it configures the sensor, which is then read by a periodic timer. The reactor outputs the orientation as a `Vector` (x, y, z):

```lf
reactor LSM6DS33 {
  output orientation: Vector
  timer t(50msec, 150msec)

  reaction(startup) {=
    // Find the sensor on the I2C bus (WHO_AM_I), reset it, and configure
    // the accelerometer and gyroscope.
  =}

  reaction(t) -> orientation {=
    // Read the gyroscope registers, integrate the angles and
    // lf_set() the new orientation.
  =}
}
```

I2C support has to be compiled into RIOT by adding these modules to the `Makefile`:

```makefile
USEMODULE += periph_i2c
USEMODULE += printf_float
USEMODULE += ztimer_sec
```

## Federated Program over CoAP

Two Feather Sense boards run as separate federates and communicate over CoAP, using their IPv6 link-local addresses (the tutorial's `Ipv6LinkLocal.ulf` program prints a board's address). In the final exercise, one board reads its orientation and the other blinks its LED faster when the first board is turned:

```lf
federated reactor {
  @interface_coap(name="if1", address="fe80::e46b:69f7:d00d:2a31")
  sensor = new SensorReactor()

  @interface_coap(name="if1", address="fe80::6481:c8fa:8443:8e02")
  blinking = new BlinkingReactor()

  @link(left="if1", right="if1")
  sensor.orientation -> blinking.orientation after 10ms;
}
```

Each `@interface_coap` annotation declares a CoAP interface with the address of the board that runs that federate, and `@link` selects the interfaces that carry the connection. `after 10ms` adds a logical delay that covers the network latency. Replace the addresses with those of your boards.

Build and flash each federate onto its own board with `LF_FED`:

```bash
make LF_MAIN=FederatedBlinking LF_FED=sensor all flash term
make LF_MAIN=FederatedBlinking LF_FED=blinking all flash term
```

## Repository

[:octicons-mark-github-16: lf-lang/micro-lf-tutorial](https://github.com/lf-lang/micro-lf-tutorial)
