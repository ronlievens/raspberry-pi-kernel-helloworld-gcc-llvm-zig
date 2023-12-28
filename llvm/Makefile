build: compile
	clang -target arm-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-0-1.elf -O2 -nostdlib build/boot-pi-0-1.o build/kernel-pi-0-1.o
	llvm-objcopy build/kernel-pi-0-1.elf -O binary build/kernel-pi-0-1.img

	clang -target arm-freestanding-eabi -T ../boot/linker.ld -o build/kernel-pi-2.elf -O2 -nostdlib build/boot-pi-2.o build/kernel-pi-2.o
	llvm-objcopy build/kernel-pi-2.elf -O binary build/kernel-pi-2.img

	clang -target aarch64-none-elf -T ../boot/linker.ld -o build/kernel-pi-3.elf -O2 -nostdlib build/boot-pi-3.o build/kernel-pi-3.o
	llvm-objcopy build/kernel-pi-3.elf -O binary build/kernel-pi-3.img

	clang -target aarch64-none-elf -T ../boot/linker.ld -o build/kernel-pi-4.elf -O2 -nostdlib build/boot-pi-4.o build/kernel-pi-4.o
	llvm-objcopy build/kernel-pi-4.elf -O binary build/kernel-pi-4.img

compile: boot
	clang -mcpu=arm1176jzf-s -fpic -target arm-freestanding-eabi -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-0-1.o -O2 -Wall -Wextra
	clang -mcpu=cortex-a7 -fpic -target arm-freestanding-eabi -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-2.o -O2 -Wall -Wextra
	clang -mcpu=cortex-a53 -fpic -target aarch64-none-elf -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-3.o -O2 -Wall -Wextra
	clang -mcpu=cortex-a72 -fpic -target aarch64-none-elf -std=gnu99 -c ../code/kernel.c -o build/kernel-pi-4.o -O2 -Wall -Wextra

boot: clean
	mkdir build
	clang -mcpu=arm1176jzf-s -fpic -target arm-freestanding-eabi -fpic -c ../boot/pi-0-1.s -o build/boot-pi-0-1.o
	clang -mcpu=cortex-a7 -fpic -target arm-freestanding-eabi -c ../boot/pi-2.s -o build/boot-pi-2.o
	clang -mcpu=cortex-a53 -fpic -target aarch64-none-elf -c ../boot/pi-3-4.s -o build/boot-pi-3.o
	clang -mcpu=cortex-a72 -fpic -target aarch64-none-elf -c ../boot/pi-3-4.s -o build/boot-pi-4.o

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
