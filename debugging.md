# Debugging Tips

## C language


*  `<to be completed>`


*  [External article related to programming in general[http://www.cprogramming.com/debugging/debugging_strategy.html]]


## Hunting assembly code or compiler bugs

The [debug library](library/debug) features some runtime inspection tool, to inspect the stack content at a certain time, display a block of data directly on the device or a full disassembly of a code portion.

On the external side, when cross-compiling, consider that the zcc framework shows any single step of the compilation process.   The temporary files are automatically deleted, but by simply copying/pasting the same commands they can be created permanently and edited by standard text editors.
The most interesting file is the one obtained by the last optimization step (the one with the '.opt' extension): it is the optimized assembly language program just before being assembled and linked in.
Just locate the first call to 'z80asm', the last parameter is the file name we're looking for; in example:

z80asm -eopt -ns -Mo -Ic:/z88dk/lib **C:\Users\us01580\AppData\Local\Temp\saeg_2.opt**


