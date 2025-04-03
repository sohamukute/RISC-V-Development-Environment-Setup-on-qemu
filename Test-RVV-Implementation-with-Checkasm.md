# Testing RISC-V Vector (RVV) Implementation with Checkasm

This guide walks you through testing the RISC-V Vector (RVV) implementation in the dav1d AV1 decoder using Checkasm inside a QEMU-based Ubuntu environment.

---

## Table of Contents
- [Prerequisites](#prerequisites)
- [Cloning the dav1d Repository](#cloning-the-dav1d-repository)
- [Installing Dependencies](#installing-dependencies)
- [Building the Project](#building-the-project)
- [Running Checkasm Tests](#running-checkasm-tests)
- [Benchmarking Functions](#benchmarking-functions)
- [References](#references)

---

## Prerequisites
Ensure that you have:
- **QEMU with Ubuntu 22.04.5 LTS installed**
- **RISC-V toolchain and environment set up**

If you haven't set up QEMU yet, refer to the [RISC-V Development Setup Guide](../Riscv_Dev_Setup.md).

---

## Cloning the dav1d Repository
```sh
git clone https://code.videolan.org/videolan/dav1d
cd dav1d
```

---

## Installing Dependencies
```sh
sudo apt update
sudo apt install meson ninja-build nasm clang
```

---

## Building the Project
```sh
meson setup build --buildtype=debug -Dtrim_dsp=false
cd build
ninja test
```

---

## Running Checkasm Tests
### View help information
```sh
./tests/checkasm --help
```

### List available tests
```sh
./tests/checkasm --list-tests
```

### List available functions
```sh
./tests/checkasm --list-functions
```

- [List of all functions and tests](https://drive.google.com/file/d/16Z9wxOMKBxf1Aw2IV51sbbngWhwbu4Ru/view?usp=sharing)
---

## Benchmarking Functions
To benchmark all functions, run:
```sh
./tests/checkasm -bench -b
```
- [Benchamrk](https://drive.google.com/file/d/1FALLUAq5gsI8FIZXoY9vQ76AtN_7WL9u/view?usp=sharing)

## References
- [dav1d Repository](https://code.videolan.org/videolan/dav1d)
- [Checkasm Documentation](https://code.videolan.org/videolan/dav1d/-/blob/master/tests/checkasm.c)

---
