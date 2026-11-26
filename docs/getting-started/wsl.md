# Windows (WSL)

micro-LF does not run natively on Windows. Use the
[Windows Subsystem for Linux](https://learn.microsoft.com/en-us/windows/wsl/) (WSL 2)
instead: inside WSL, follow the Linux instructions on every page of this
documentation. The only Windows-specific step is giving WSL access to your
board's USB connection, which this page explains.

## 1. Install WSL

From an administrator PowerShell:

```powershell
wsl --install
```

This installs WSL 2 with Ubuntu. Open the Ubuntu terminal and continue with
[Getting Started](index.md). Keep your projects inside the WSL file system
(e.g. `~/projects`), not under `/mnt/c`, as builds there are much slower.

## 2. Attach a Board to WSL

WSL cannot see USB devices by default. Install
[usbipd-win](https://github.com/dorssel/usbipd-win) on Windows:

```powershell
winget install usbipd
```

Then, with the board plugged in, run from an administrator PowerShell:

```powershell
usbipd list                              # find the BUSID of your board
usbipd bind --busid <BUSID>              # once per board
usbipd attach --wsl --busid <BUSID>      # every time the board is plugged in
```

While a board is attached to WSL, Windows cannot use it. Boards usually appear as:

| Board | Shows up as |
|-------|-------------|
| Raspberry Pi Pico in BOOTSEL mode | USB Mass Storage Device, RP2 Boot |
| Raspberry Pi Pico running a program | USB Serial Device |
| ESP32 | USB Serial Device or USB JTAG/serial debug unit |

Check in WSL that the board is visible:

```bash
lsusb
ls /dev/ttyACM* /dev/ttyUSB*
```

See Microsoft's [Connect USB devices](https://learn.microsoft.com/en-us/windows/wsl/connect-usb)
guide for details.

## 3. Troubleshooting

**The board disappears after flashing or a reset.** Some boards reconnect as a
different USB device, for example a Pico entering or leaving BOOTSEL mode.
Attach it again, or let usbipd re-attach it automatically:

```powershell
usbipd attach --wsl -a --busid <BUSID>
```

**Permission denied on `/dev/ttyACM0` or `/dev/ttyUSB0`.** Add your user to the
`dialout` group, then restart WSL with `wsl --shutdown` from PowerShell:

```bash
sudo usermod -aG dialout $USER
```

**`picotool` cannot access the Pico.** Run it with `sudo`, or install the udev
rules that come with picotool.

**`nix develop` fails with a permission error.** Make sure your user is in the
`nix-users` group (check with `groups`):

```bash
sudo usermod -aG nix-users $USER
```
