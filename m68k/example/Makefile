TARGET := example
TOOLS = $(HOME)/x-tools/m68k-megadrive-elf/bin/m68k-megadrive-elf

CC_SRCS := $(wildcard *.c)
CC_OBJS := $(CC_SRCS:%.c=%.o)

AS_FLAGS := \
	-D -mcpu=68000 -march=68000 --bitwise-or --traditional-format --register-prefix-optional -L -g -al #-alh
CC_FLAGS := \
	-m68000 -mshort -c -g -Wall -Wextra -Wundef -pedantic-errors -std=c99 \
	-Wstrict-prototypes -Wno-missing-field-initializers

all: $(TARGET).bin

$(TARGET).bin: $(TARGET).elf
	$(TOOLS)-objcopy -O binary $< $@

$(TARGET).elf: crt0.o $(CC_OBJS)
	$(TOOLS)-ld -nostdlib -Ttext=0x000000 -Tdata=0xFF0000 -Tbss=0xFF0000 -A 68000 --defsym _start=0x200 -o $@ $+

%.o: %.s
	$(TOOLS)-as $(AS_FLAGS) -o $@ $<

%.o: %.c
	$(TOOLS)-gcc $(CC_FLAGS) -o $@ $<

clean: FORCE
	rm -f *.o *.elf *.bin *.lst

FORCE:
