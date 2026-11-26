# POSIX (Linux / macOS)

- **Template**: [ulf-posix-template](https://github.com/lf-lang/ulf-posix-template) (optional)
- **Supported Systems**: Linux and macOS

The POSIX platform compiles and runs micro-LF programs directly on your computer, with no hardware or cross-compilation toolchain. It is the quickest way to try micro-LF and to develop and test programs.

## Prerequisites

In addition to the [common prerequisites](../index.md#common-prerequisites):

- **A C compiler** (e.g. `gcc` or `clang`)
- **CMake 3.20+**

These are pre-installed on macOS with the Xcode Command Line Tools, and available from your distribution's package manager on Linux.

## Setup

A template is not required: the micro-LF compiler can build a single `.ulf` file directly (see [Building](#building)). For a CMake-based project, clone the template:

```bash
git clone https://github.com/lf-lang/ulf-posix-template.git my-project
cd my-project
```

## Building

Programs for this platform declare `@platform("Native")` on the main reactor (or on the federated reactor):

```lf
@platform("Native")
main reactor {
    ...
}
```

Without a template, run the compiler on the program. It also compiles the generated C code and places the executable in `bin/`:

```bash
$REACTOR_UC_PATH/ulf/bin/ulfc-dev src/MyApp.ulf
```

With the template, build with CMake. The default application is `HelloPosix`:

```bash
cmake -Bbuild
cmake --build build
```

## Running

```bash
bin/MyApp            # without a template
./build/HelloPosix   # with the template
```

## Changing the Application

Without a template, pass a different `.ulf` file to the compiler. With the template, set `LF_MAIN` in `CMakeLists.txt`, or on the command line:

```bash
cmake -Bbuild -DLF_MAIN=Timer
cmake --build build
```

## Log Level

With the template, the default is `LF_LOG_LEVEL_WARN`:

```bash
cmake -Bbuild -DLOG_LEVEL=LF_LOG_LEVEL_DEBUG
```

Without a template, use the [`@logging`](../../documentation/annotations.md#runtime-configuration) annotation.

## Cleaning

```bash
rm -rf src-gen build bin
```

## Network Channels

| Channel | Description |
|---------|-------------|
| TCP/IP | POSIX TCP sockets |

Each federate of a federated program runs as a separate process. With the template, build each federate separately with `FEDERATE`, then run each executable in its own terminal:

```bash
cmake -Bbuild -DLF_MAIN=FederatedConnection -DFEDERATE=r1
cmake --build build
./build/FederatedConnection_r1
```
