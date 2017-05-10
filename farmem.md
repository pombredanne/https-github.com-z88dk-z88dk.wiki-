# Breaking the 64k Barrier


The Z80 processor is an old processor, and it's based on an even older one - the Intel 8080. These two processors are 8 bit accumulator based and can access an address space of 65536 bytes (or 64k).

Quite early on it was realised that this simply isn't enough address space to implement a comprehensive operating system and leave sufficient user RAM for their own programs.

The solution to this is quite straightforward, have more ROM/RAM in the system and page it in when required. This trick isn't just used on the Z80 but also for the 6500 processors.

# Abstracted Handling

z88dk supports the concept of a ''far'' pointer. This a type qualifier that can be prefixed to any pointer type, and as far as the programmer is concerned, handling of them should be transparent.

The far pointers used by z88dk are 24 bits long i.e. 3 bytes, this enables the theoretical access of 16Mb of memory. The far pointer allows for a *flat* memory model - i.e. they are treated as "normal" 24 bit number by the compiler. Far pointers are always handled in ''ehl'' where e contains bits 16-23 and hl bits 0-15 of the pointer. If e holds 0 then hl is taken to be an absolute address in the current memory configuration.

At present, an [implementation](farmemory) is only available for the [Z88](platform/z88) platform.

