# POSIX (Linux / macOS)

- **Template**: [ulf-posix-template](https://github.com/lf-lang/ulf-posix-template) (optional)

The Native platform target compiles and runs micro-LF programs directly on POSIX-compatible operating systems. Both **Linux** and **macOS** are supported with no additional hardware or cross-compilation toolchain required.

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites), you need a standard C compiler (e.g. `gcc` or `clang`) and **CMake 3.20+**. These are typically pre-installed on macOS (via Xcode Command Line Tools) and available through your distribution's package manager on Linux.

## Setup

A template is not required: `ulfc` can compile a single `.ulf` file directly (see [Building](#building)). To use the CMake-based template:

```bash
git clone https://github.com/lf-lang/ulf-posix-template.git my-project
cd my-project
```

## Usage

Add the `@platform("Native")` attribute to your main reactor (or to the federated top-level reactor):

```lf
@platform("Native")
main reactor {
    ...
}
```

## Building

Without a template, run the compiler on your program. It compiles the generated C code for the host and places the executable in the `bin/` directory of your project:

```bash
ulfc-dev src/MyApp.ulf
bin/MyApp
```

With the template, build with CMake (default application: `HelloPosix`):

```bash
cmake -Bbuild
cmake --build build
./build/HelloPosix
```

## Changing the Application

With the template, set `LF_MAIN` in `CMakeLists.txt` or on the command line:

```bash
cmake -Bbuild -DLF_MAIN=Timer
cmake --build build
```

## Log Level

With the template:

```bash
cmake -Bbuild -DLOG_LEVEL=LF_LOG_LEVEL_DEBUG
```

Without a template, use the [`@logging`](../../documentation/annotations.md) annotation.

## Cleaning

```bash
rm -rf src-gen build bin
```

## Network Channels

| Channel | Description |
|---------|-------------|
| TCP/IP | POSIX TCP sockets |

## Federated Programs

The same attribute applies to the top-level federated reactor:

```lf
@platform("Native")
federated reactor MyFederatedApp {
    ...
}
```

With the template, build each federate separately with `FEDERATE`, then run each executable in its own terminal:

```bash
cmake -Bbuild -DLF_MAIN=FederatedConnection -DFEDERATE=r1
cmake --build build
./build/FederatedConnection_r1
```
