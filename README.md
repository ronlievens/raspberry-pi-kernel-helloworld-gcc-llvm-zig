# The Raspberry Pi Hello world kernel in gcc, llvm or zig

In this project we compile, build and emulate a hello world kernel for the Raspberry Pi models:

- Raspberry Pi Zero (revision 1.2)
- Raspberry Pi A+ (revision 1.1)
- Raspberry Pi 2B (revision 1.1)
- Raspberry Pi 3A+ (revision 1.0)
- Raspberry Pi 3B (revision 1.2)

This project is primary based on the guide from [osdev - Raspberry Pi Bare Bones](https://wiki.osdev.org/Raspberry_Pi_Bare_Bones)

The Raspberry Pi 4 and 5 are not yet supported by QEMU emulator version 8.2.0.

## Setup the toolchain

We use can use arm-gcc, llvm or the zig compiler and we emulate the Raspberry Pi with qemu.

To install the toolchain on Windows we will use [scoop](https://scoop.sh/):

```shell
scoop install gcc-arm-none-eabi llvm zig zls extras/vcredist2022 qemu bmake
```

To install the toolchain on Macos we will use [brew](https://brew.sh/):

```shell
brew install gcc-arm-embedded aarch64-elf-gcc aarch64-elf-binutils llvm zig zls qemu bmake
```

## Building and running the kernel
As the title suggest we can compile the code with gcc `cd gcc && bmake`,
run the code with `bmake qemu-pi-2` (or `qemu-pi-0`, `qemu-pi-1`, `qemu-pi-3`)

Or you can compile the code using llvm `cd llvm && bmake`,
run the code with `bmake qemu-pi-2` (or `qemu-pi-0`, `qemu-pi-1`, `qemu-pi-3`)

And as last compile the code using zig `cd zig && bmake`,
run the code with `bmake qemu-pi-2` (or `qemu-pi-0`, `qemu-pi-1`, `qemu-pi-3`)

## Issues

The current issues in the project are:
- qemu doesn't support the raspberry pi 4 (is coming soon)
