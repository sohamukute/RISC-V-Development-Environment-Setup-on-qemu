# RISC-V Development Environment Setup

This guide provides a step-by-step process for setting up a RISC-V development environment, including the GNU toolchain, QEMU, and Ubuntu 22.04.5 LTS for RISC-V. 

---

## Table of Contents
- [Prerequisites](#prerequisites)
- [Installing Dependencies](#installing-dependencies)
- [Setting up RISC-V GNU Toolchain](#setting-up-risc-v-gnu-toolchain)
- [Setting up QEMU for RISC-V](#setting-up-qemu-for-risc-v)
- [Installing Ubuntu 22.04.5 LTS for RISC-V](#installing-ubuntu-22045-lts-for-risc-v)
- [Running the Installed Ubuntu Image](#running-the-installed-ubuntu-image)
- [References](#references)

---

## Prerequisites
Ensure you have at least **25GB of free disk space** and **8GB of RAM** for smooth operation.

## Installing Dependencies
### For Debian/Ubuntu-based Systems
```sh
sudo apt update
sudo apt install git build-essential autoconf automake autotools-dev curl python3 libmpc-dev \
  libmpfr-dev libgmp-dev gawk bison flex texinfo gperf libtool patchutils bc zlib1g-dev \
  libexpat-dev ninja-build cmake libglib2.0-dev libpixman-1-dev
```

## Setting up RISC-V GNU Toolchain
### Clone the RISC-V GNU Toolchain Repository
```sh
git clone --recursive https://github.com/riscv/riscv-gnu-toolchain
```

### Configure and Build the Toolchain for Linux
```sh
cd riscv-gnu-toolchain
./configure --prefix=$HOME/riscv/toolchain --enable-multilib
make linux -j$(nproc)
```

### Add the Toolchain to Your PATH
```sh
echo 'export PATH=$PATH:$HOME/riscv/toolchain/bin' >> ~/.bashrc
source ~/.bashrc
```

---

## Setting up QEMU for RISC-V
### Clone QEMU Source Code
```sh
git clone https://github.com/qemu/qemu.git
cd qemu
```

### Configure and Build QEMU with RISC-V Support
```sh
./configure --target-list=riscv64-softmmu,riscv64-linux-user,riscv32-softmmu,riscv32-linux-user
make -j$(nproc)
sudo make install
```

### Verify QEMU Installation
```sh
qemu-system-riscv64 --version  # Should show v8.2 or newer
```

---

## Installing Ubuntu 22.04.5 LTS for RISC-V
### Installation Steps
1. **Download the Ubuntu 22.04.5 LTS (Jammy Jellyfish) image**:
   ```sh
   wget <Ubuntu-22.04.5-riscv64-image-URL>
   ```
2. **Unpack the disk image**:
   ```sh
   gzip -d ubuntu-22.04.5-live-server-riscv64.img.gz
   ```
3. **Create a Disk Image for Ubuntu Installation (16 GiB should be sufficient)**:
   ```sh
   fallocate -l 16G disk
   ```
4. **Start the Installer**:
   ```sh
   qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
      -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
      -netdev user,id=net0 \
      -device virtio-net-device,netdev=net0 \
      -drive file=ubuntu-22.04.5-live-server-riscv64.img,format=raw,if=virtio \
      -drive file=disk,format=raw,if=virtio \
      -device virtio-rng-pci
   ```
5. **Follow the standard Ubuntu Server installation tutorial**

---

## Running the Installed Ubuntu Image
```sh
qemu-system-riscv64 -machine virt -m 4G -smp cpus=2 -nographic \
    -kernel /usr/lib/u-boot/qemu-riscv64_smode/u-boot.bin \
    -netdev user,id=net0 \
    -device virtio-net-device,netdev=net0 \
    -drive file=disk,format=raw,if=virtio \
    -device virtio-rng-pci
```

---

## References
- [RISC-V GNU Toolchain](https://github.com/riscv/riscv-gnu-toolchain)
- [QEMU for RISC-V](https://www.qemu.org)
- [Ubuntu RISC-V Image](https://ubuntu.com/download/risc-v)

---


### Contributing
Pull requests are welcome! If you encounter any issues, please open an issue in this repository.

---

🚀 **Happy Coding with RISC-V!**
