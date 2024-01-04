build: compile
	arm-none-eabi-gcc -T ../boot/linker.ld -o build/kernel-pi-0-1.elf -ffreestanding -O2 -nostdlib build/boot-pi-0-1.o build/kernel-pi-0-1.o -lgcc
	arm-none-eabi-objcopy build/kernel-pi-0-1.elf -O binary build/kernel-pi-0-1.img

	arm-none-eabi-gcc -T ../boot/linker.ld -o build/kernel-pi-2.elf -ffreestanding -O2 -nostdlib build/boot-pi-2.o build/kernel-pi-2.o -lgcc
	arm-none-eabi-objcopy build/kernel-pi-2.elf -O binary build/kernel-pi-2.img
ifneq ($(OS),Windows_NT)
	aarch64-elf-gcc -T ../boot/linker.ld -o build/kernel-pi-3.elf -ffreestanding -O2 -nostdlib build/boot-pi-3.o build/kernel-pi-3.o -lgcc
	aarch64-elf-objcopy build/kernel-pi-3.elf -O binary build/kernel-pi-3.img

	aarch64-elf-gcc -T ../boot/linker.ld -o build/kernel-pi-4.elf -ffreestanding -O2 -nostdlib build/boot-pi-4.o build/kernel-pi-4.o -lgcc
	aarch64-elf-objcopy build/kernel-pi-4.elf -O binary build/kernel-pi-4.img
else
	@echo Please use llvm or zig for aarch64 on Windows
endif

compile: boot
	arm-none-eabi-gcc -mcpu=arm1176jzf-s -fpic -ffreestanding -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-0-1.o -O2 -Wall -Wextra -DRASPBERRY_MODEL_VERSION=1
	arm-none-eabi-gcc -mcpu=cortex-a7 -fpic -ffreestanding -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-2.o -O2 -Wall -Wextra -DRASPBERRY_MODEL_VERSION=2
ifneq ($(OS),Windows_NT)
	aarch64-elf-gcc -mcpu=cortex-a53 -fpic -ffreestanding -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-3.o -O2 -Wall -Wextra -DRASPBERRY_MODEL_VERSION=3
	aarch64-elf-gcc -mcpu=cortex-a72 -fpic -ffreestanding -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-4.o -O2 -Wall -Wextra -DRASPBERRY_MODEL_VERSION=4
else
	@echo Please use llvm or zig for aarch64 on Windows
endif

boot: clean
	mkdir build
	arm-none-eabi-gcc -mcpu=arm1176jzf-s -fpic -ffreestanding -c ../boot/pi-0-1.s -o build/boot-pi-0-1.o
	arm-none-eabi-gcc -mcpu=cortex-a7 -fpic -ffreestanding -c ../boot/pi-2.s -o build/boot-pi-2.o
ifneq ($(OS),Windows_NT)
	aarch64-elf-gcc -mcpu=cortex-a53 -fpic -ffreestanding -c ../boot/pi-3-4.s -o build/boot-pi-3.o
	aarch64-elf-gcc -mcpu=cortex-a72 -fpic -ffreestanding -c ../boot/pi-3-4.s -o build/boot-pi-4.o
else
	@echo Please use llvm or zig for aarch64 on Windows
endif

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
