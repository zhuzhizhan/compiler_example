objcopy -I binary -O elf32-i386 -B i386 image.jpg image.o
objdump -ht image.o
