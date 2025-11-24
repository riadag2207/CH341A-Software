# CH341A Software Repository Instructions

This repository is a collection of drivers, software, and tools for the CH341A USB programmer chip, organized by operating system. It contains both binary distributions (Windows/Android) and C source code (Linux).

## 📂 Project Structure & Navigation

The codebase is segmented by target operating system. Identify the user's target OS immediately.

- **`Programas/Linux/`**: The primary area for C source code development.
  - `ch341eeprom-master24xx/`: Tools for 24Cxx I2C EEPROMs.
  - `ch341prog-master 25xx/`: Tools for 25xx SPI Flash chips.
  - `flashrom-master/`: Source for the universal flashrom utility.
- **`Programas/Windows/`**: Contains pre-compiled binaries (AsProgrammer, NeoProgrammer) and tools.
- **`Drivers/`**: Kernel drivers for various OSs. Distinguish between *drivers* (kernel modules) and *programs* (userspace tools).

## 🐧 Linux Development (`Programas/Linux/`)

This is the active development area. All tools rely on `libusb-1.0`.

### Build Systems
Each subdirectory has its own independent build system. There is no root-level Makefile.

1.  **ch341eeprom** (`Programas/Linux/ch341eeprom-master24xx/`)
    -   **Compiler**: Defaults to `clang`.
    -   **Build**: Run `make` inside the directory.
    -   **Key Files**: `ch341eeprom.c` (main logic), `ch341funcs.c` (USB communication).

2.  **ch341prog** (`Programas/Linux/ch341prog-master 25xx/`)
    -   **Compiler**: Defaults to `gcc` with `-std=gnu99`.
    -   **Build**: Run `make` inside the directory.
    -   **Key Files**: `main.c`, `ch341a.c`.

### Testing & Verification
-   **ch341eeprom**: The Makefile includes integration tests that generate random data, write to the chip, read it back, and compare.
    -   Run: `make test02` (for 24c02), `make test64` (for 24c64), etc.
    -   *Note*: These tests require a physical device connected.

## ⚠️ Critical Conventions & Gotchas

-   **Path Handling**: Directory names contain spaces (e.g., `ch341prog-master 25xx`). **ALWAYS** quote paths in shell commands.
    -   *Bad*: `cd Programas/Linux/ch341prog-master 25xx`
    -   *Good*: `cd "Programas/Linux/ch341prog-master 25xx"`
-   **Dependencies**: Ensure `libusb-1.0-0-dev` (or equivalent) is installed before building.
-   **Permissions**: Linux tools often require udev rules to access the USB device without root.
    -   Refer to `99-CH341.rules` or `99-ch341a-prog.rules` in the respective directories.
    -   Install command: `sudo make install-udev-rule` (available in `ch341prog`).

## 💻 Code Style (C)
-   **APIs**: The tools interact directly with USB via `libusb`. Look for `usb_control_msg` and `usb_bulk_msg` for low-level protocol implementation.
-   **Error Handling**: Check return codes from `libusb` functions. Negative values usually indicate errors.
