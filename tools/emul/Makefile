TARGETS = shell zasm
KERNEL_HEADERS = shell-kernel.h zasm-kernel.h
USER_HEADERS = zasm-user.h

.PHONY: all
all: $(TARGETS)

shell-kernel.h: shell_.asm
zasm-kernel.h: zasm_glue.asm
$(KERNEL_HEADERS):
	scas -o - -I ../../parts/z80 $< | ./bin2c.sh KERNEL | tee $@ > /dev/null

zasm-user.h: zasm_user.asm
	scas -o - -I ../../apps/zasm $< | ./bin2c.sh USERSPACE | tee $@ > /dev/null

shell: shell.c libz80/libz80.o shell-kernel.h
zasm: zasm.c libz80/libz80.o zasm-kernel.h zasm-user.h
$(TARGETS):
	cc $< libz80/libz80.o -o $@

libz80/libz80.o: libz80/z80.c
	make -C libz80/codegen opcodes
	gcc -Wall -ansi -g -c -o libz80/libz80.o libz80/z80.c

.PHONY: clean
clean:
	rm -f $(TARGETS) $(KERNEL_HEADERS) $(USER_HEADERS)