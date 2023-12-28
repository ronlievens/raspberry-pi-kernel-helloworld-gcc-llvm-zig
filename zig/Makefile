build: compile
	zig cc -target arm-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-0-1.elf -ffreestanding -O2 -nostdlib build/boot-pi-0-1.o build/kernel-pi-0-1.o
	zig objcopy build/kernel-pi-0-1.elf -O binary build/kernel-pi-0-1.img

	zig cc -target arm-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-2.elf -ffreestanding -O2 -nostdlib build/boot-pi-2.o build/kernel-pi-2.o
	zig objcopy build/kernel-pi-2.elf -O binary build/kernel-pi-2.img

	zig cc -target aarch64-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-3.elf -ffreestanding -O2 -nostdlib build/boot-pi-3.o build/kernel-pi-3.o
	zig objcopy build/kernel-pi-3.elf -O binary build/kernel-pi-3.img

	zig cc -target aarch64-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-4.elf -ffreestanding -O2 -nostdlib build/boot-pi-4.o build/kernel-pi-4.o
	zig objcopy build/kernel-pi-4.elf -O binary build/kernel-pi-4.img

compile: boot
	zig cc -mcpu=arm1176jzf_s -target arm-freestanding-eabi -fpic -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-0-1.o -O2 -Wall -Wextra
	zig cc -mcpu=cortex_a7 -target arm-freestanding-eabi -fpic -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-2.o -O2 -Wall -Wextra
	zig cc -mcpu=cortex_a53 -target aarch64-freestanding-eabi -fpic -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-3.o -O2 -Wall -Wextra
	zig cc -mcpu=cortex_a72 -target aarch64-freestanding-eabi -fpic -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-4.o -O2 -Wall -Wextra

boot: clean
	mkdir build
	zig cc -mcpu=arm1176jzf_s -target arm-freestanding-eabi -fpic -c ../boot/pi-0-1.s -o build/boot-pi-0-1.o
	zig cc -mcpu=cortex_a7 -target arm-freestanding-eabi -fpic -c ../boot/pi-2.s -o build/boot-pi-2.o
	zig cc -mcpu=cortex_a53 -target aarch64-freestanding-eabi -fpic -c ../boot/pi-3-4.s -o build/boot-pi-3.o
	zig cc -mcpu=cortex_a72 -target aarch64-freestanding-eabi -fpic -c ../boot/pi-3-4.s -o build/boot-pi-4.o

clean:
	rm -rf build

qemu-pi-0:
	qemu-system-arm -m 512 -M raspi0 -serial stdio -kernel build/kernel-pi-0-1.elf

qemu-pi-1:
	qemu-system-arm -m 512 -M raspi1ap -serial stdio -kernel build/kernel-pi-0-1.elf

qemu-pi-2:
	qemu-system-arm -m 1024 -M raspi2b -serial stdio -kernel build/kernel-pi-2.elf

qemu-pi-3:
	qemu-system-aarch64 -m 1024 -M raspi3b -serial stdio -kernel build/kernel-pi-3.elf
