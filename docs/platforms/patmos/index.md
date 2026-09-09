# Patmos

- **Git:** <https://github.com/t-crest/patmos>
- **Documentation:** <https://github.com/t-crest/patmos/wiki>
- **micro-LF Docs:** <https://micro-lf.org>

______

This is a template for micro-LF applications targeting the [Patmos](https://github.com/t-crest/patmos) processor. It is designed to be cloned as a sibling folder alongside reactor-uc.

## 1. Prerequisites

### 1.1. Basic

You must use one of the following operating systems:

- `Linux` — Officially supported are Debian & Ubuntu
- `macOS`

Your system must have the following software packages (you likely have at least some of these already):

- `git` — [a distributed version control system](https://git-scm.com/)
- `java` — [Java 17](https://openjdk.org/projects/jdk/17)

Install Patmos by following the instructions at <https://github.com/t-crest/patmos>.

### 1.2. micro-LF

This template is for running micro-LF applications on the Patmos processor. It uses [reactor-uc](https://github.com/lf-lang/reactor-uc), the runtime that facilitates the execution.

## 2. Choose a Directory

You can either use an existing directory, create a new one, or use the `~` (home) directory to store the files. Then navigate to it.

## 3. Clone reactor-uc

Clone reactor-uc and set `REACTOR_UC_PATH`:

#### Clone via HTTPS

```bash
git clone https://github.com/lf-lang/reactor-uc.git --recursive
cd reactor-uc
export REACTOR_UC_PATH=$(pwd)
```

#### Or Clone via SSH

```bash
git clone git@github.com:lf-lang/reactor-uc.git --recursive
cd reactor-uc
export REACTOR_UC_PATH=$(pwd)
```

## 4. Clone this Repository

Clone this template repository as a sibling folder to reactor-uc:

```shell
cd ..
git clone --depth=1 https://github.com/lf-lang/ulf-patmos-template.git ulf-patmos-template
cd ulf-patmos-template
```

## 5. Build

```bash
make all
```

To build a different micro-LF application, set the `LF_MAIN` variable:

```bash
make LF_MAIN=Smoke all
```