A full z80 disassembler is provided with z88dk. In addition to supporting the z80, it can also disassemble code for the other supported CPUs (z180, z80-zxn and Rabbit processors) as well as other related (but unsupported by z88dk) processors: ez80, R800, gbz80, 8080.

## Usage

At its simplest:

    z88dk-dis -o [address] [binary file]

Will load binary file at the specified address and start disassembling from that point.

The diassembler understands the .map files generated by z80asm, and these will be read using the `-x` option:

    z88dk-dis -o [address] -x [map file] [binary file]


