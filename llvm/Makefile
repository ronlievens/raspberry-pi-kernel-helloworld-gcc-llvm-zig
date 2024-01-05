CC32=clang
CC64=$(CC32)
OBJCOPY32=llvm-objcopy
OBJCOPY64=$(OBJCOPY32)
CFLAGS=-fpic
CPPFLAGS=-O2 -Wall -Wextra -nostdlib

PLATFORM_32_BIT=-target arm-freestanding-eabi
PLATFORM_64_BIT=-target aarch64-none-elf

CPU_P1=-mcpu=arm1176jzf-s
CPU_P2=-mcpu=cortex-a7
CPU_P3=-mcpu=cortex-a53
CPU_P4=-mcpu=cortex-a72
CPU_P5=-mcpu=cortex-a76

# Specially for clion
all: build

build: compile
	$(CC32) $(CPU_P1) $(PLATFORM_32_BIT) $(CFLAGS) -T ../boot/linker-aarch32.ld -o build/kernel-pi-0-1.elf build/boot-pi-0-1.o build/kernel-pi-0-1.o $(CPPFLAGS)
	$(OBJCOPY32) build/kernel-pi-0-1.elf -O binary build/kernel-pi-0-1.img

	$(CC32) $(CPU_P2) $(PLATFORM_32_BIT) $(CFLAGS) -T ../boot/linker-aarch32.ld -o build/kernel-pi-2.elf  build/boot-pi-2.o build/kernel-pi-2.o $(CPPFLAGS)
	$(OBJCOPY32) build/kernel-pi-2.elf -O binary build/kernel-pi-2.img

	$(CC64) $(CPU_P3) $(PLATFORM_64_BIT) $(CFLAGS) -T ../boot/linker-aarch64.ld -o build/kernel-pi-3.elf -nostdlib build/aarch64-single-core.o build/kernel-pi-3.o $(CPPFLAGS)
	$(OBJCOPY64) build/kernel-pi-3.elf -O binary build/kernel-pi-3.img

	$(CC64) $(CPU_P4) $(PLATFORM_64_BIT) $(CFLAGS) -T ../boot/linker-aarch64.ld -o build/kernel-pi-4.elf -nostdlib build/aarch64-single-core.o build/kernel-pi-4.o $(CPPFLAGS)
	$(OBJCOPY64) build/kernel-pi-4.elf -O binary build/kernel-pi-4.img

	$(CC64) $(CPU_P5) $(PLATFORM_64_BIT) $(CFLAGS) -T ../boot/linker-aarch64.ld -o build/kernel-pi-5.elf -nostdlib build/aarch64-single-core.o build/kernel-pi-5.o $(CPPFLAGS)
	$(OBJCOPY64) build/kernel-pi-4.elf -O binary build/kernel-pi-5.img

compile: boot
	$(CC32) $(CPU_P1) $(PLATFORM_32_BIT) $(CFLAGS) -c ../code/kernel.c -o build/kernel-pi-0-1.o -DRASPBERRY_MODEL_VERSION=1 $(CPPFLAGS)
	$(CC32) $(CPU_P2) $(PLATFORM_32_BIT) $(CFLAGS) -c ../code/kernel.c -o build/kernel-pi-2.o -DRASPBERRY_MODEL_VERSION=2 $(CPPFLAGS)
	$(CC64) $(CPU_P3) $(PLATFORM_64_BIT) $(CFLAGS) -c ../code/kernel.c -o build/kernel-pi-3.o -DRASPBERRY_MODEL_VERSION=3 -DAARCH64 $(CPPFLAGS)
	$(CC64) $(CPU_P4) $(PLATFORM_64_BIT) $(CFLAGS) -c ../code/kernel.c -o build/kernel-pi-4.o -DRASPBERRY_MODEL_VERSION=4 -DAARCH64 $(CPPFLAGS)
	$(CC64) $(CPU_P5) $(PLATFORM_64_BIT) $(CFLAGS) -c ../code/kernel.c -o build/kernel-pi-5.o -DRASPBERRY_MODEL_VERSION=5 -DAARCH64 $(CPPFLAGS)

boot: clean
	mkdir build
	$(CC32) $(CPU_P1) $(PLATFORM_32_BIT) $(CFLAGS) -c ../boot/aarch32-multi-core.s -o build/boot-pi-0-1.o
	$(CC32) $(CPU_P2) $(PLATFORM_32_BIT) $(CFLAGS) -c ../boot/aarch32-single-core.s -o build/boot-pi-2.o
	$(CC64) $(PLATFORM_64_BIT) $(CFLAGS) -c ../boot/aarch64-single-core.s -o build/aarch64-single-core.o
	$(CC64) $(PLATFORM_64_BIT) $(CFLAGS) -c ../boot/aarch64-multi-core.s -o build/aarch64-multi-core.o

clean:
	rm -rf build

qemu-pi-0: build
	qemu-system-arm -m 512 -M raspi0 -serial stdio -kernel build/kernel-pi-0-1.elf

qemu-pi-1: build
	qemu-system-arm -m 512 -M raspi1ap -serial stdio -kernel build/kernel-pi-0-1.elf

qemu-pi-2: build
	qemu-system-arm -m 1024 -M raspi2b -serial stdio -kernel build/kernel-pi-2.elf

qemu-pi-3: build
	qemu-system-aarch64 -m 1024 -M raspi3b -serial stdio -kernel build/kernel-pi-3.elf
