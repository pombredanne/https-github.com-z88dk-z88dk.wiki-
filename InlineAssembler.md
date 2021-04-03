If so desired assembly language can be inlined into functions. On the whole, doing so isn't recommended due to the following reasons:

* The C function prologue varies between compilers and calling conventions
* Embedding assembler in the middle of a function can harm optimisation
* Compilers may have moved variables into registers

The preferred way to implement assembly subroutines called from C is that prototypes are supplied in a header file to tell the C compiler how to call the asm functions and then a separate asm file contains the asm implementation. This has the additional advantage that it's easy to produce assembler entry points with parameters passed in the most convenient registers rather that on the stack.

Despite all of that should you wish to inline assembler then you should surround it with `#asm`, `#endasm`. zcc/zpragma will take care of translating the common syntax into a syntax suitable for use with either backend compiler.

Embedding assembler in macros needs some care, using the pseudo function `__asm__("")` is better solution than using #asm/#endasm. 
