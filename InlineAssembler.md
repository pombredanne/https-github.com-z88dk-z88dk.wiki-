Assembly language can be inlined in C code if it is surrounded by ”#asm” and ”#endasm” tags. Note that sdcc's back end is not used by z88dk so all assembly language instructions, inlined assembly included, is processed by z80asm and therefore use standard Zilog mnemonics.

The C compilers ignore inlined assembly code and simply generate code around it. Inlined assembly can access any variables and functions defined in the local file and can make use of PUBLIC and EXTERN to export and import names as discussed before. They can also access local variables declared on the stack as described in the last section.

Inlined assembly is often misused in the z80 community. It's meant to interject a small amount of assembly code into an otherwise C function. But quite often C functions are declared with their entire bodies written in assembly language using inlined assembler. The preferred way to implement assembly subroutines called from C was described earlier: prototypes are supplied in a header file to tell the C compiler how to call the asm functions and then a separate asm file contains the asm implementation.

* C prologue can change over time or across compilers
* cannot place code in arbitrary section
* separation of C and asm is cleaner organizationally and increases portability
* separation makes it easy to create libraries of asm routines later
* can easily supply an asm interface
* compilers may have moved data into a register