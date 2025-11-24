# CH341A Softwares
CH341A Softwares (Windows, Linux, Mac and Android)
 <p align="center">
  <img src="https://raw.githubusercontent.com/YTEC-info/CH341A-Softwares/main/Manual/pic-ch341a-black_.jpg" />
</p>
# CH341A Softwares - AI Agent Instructions

This repository is a collection of drivers, software, and tools for the CH341A USB programmer chip, organized by operating system.

## 📂 Project Structure & Navigation

The codebase is segmented by target operating system. When answering user queries, first identify the target OS.

- **`Programas/Linux/`**: Contains C source code for Linux tools. This is the primary area for code analysis and modification.
- **`Programas/Windows/`**: Primarily contains pre-compiled binaries and tools (e.g., AsProgrammer, NeoProgrammer).
- **`Drivers/`**: Contains driver files for Android, Linux, Mac, and Windows.

## 🐧 Linux Development (`Programas/Linux/`)

The Linux section contains three main projects. Assume `libusb-1.0` is a required dependency for all.

### 1. ch341eeprom (`Programas/Linux/ch341eeprom-master24xx/`)
- **Purpose**: Read/Write 24Cxx serial EEPROMs.
- **Build**: Uses `clang` by default.
  ```bash
  cd "Programas/Linux/ch341eeprom-master24xx"
  make
  ```
- **Testing**: The Makefile includes test targets (e.g., `make test02`, `make test64`) that generate random data and verify read/write operations.

### 2. ch341prog (`Programas/Linux/ch341prog-master 25xx/`)
- **Purpose**: SPI/IIC programmer for 25xx series chips.
- **Build**: Uses `gcc`.
  ```bash
  cd "Programas/Linux/ch341prog-master 25xx"
  make
  ```
- **Installation**: Includes a udev rule for device permissions.
  ```bash
  sudo make install-udev-rule
  ```

### 3. flashrom (`Programas/Linux/flashrom-master/`)
- **Purpose**: Universal flash programming utility.
- **Build**: Standard flashrom build system (check `Makefile` or `meson.build`).

## 🛠️ Common Workflows & Patterns

- **Device Access**: Linux tools use `libusb` to communicate directly with the CH341A chip. Look for `usb_open`, `usb_control_msg`, and `usb_bulk_msg` patterns in `ch341funcs.c` or `ch341a.c`.
- **EEPROM Addressing**: When working with EEPROM logic, pay attention to the specific addressing modes (8-bit vs 16-bit) depending on the chip size (e.g., 24c01 vs 24c64).
- **Binary Handling**: Windows tools are mostly "black boxes". If asked about them, refer to their specific folder names and READMEs if available, but note that source code is generally not available unless specified (e.g., inside a zip).

## ⚠️ Important Notes

- **Path Handling**: Directory names may contain spaces (e.g., `ch341prog-master 25xx`). Always quote paths in shell commands.
- **Driver vs. Tool**: Distinguish between installing the *driver* (kernel module) and using the *tool* (userspace program). The `Drivers/` folder is for the former, `Programas/` for the latter.
