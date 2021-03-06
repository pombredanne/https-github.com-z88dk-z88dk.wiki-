A full z80 disassembler is provided with z88dk. In addition to supporting the z80, it can also disassemble code for the other supported CPUs (8080, gbz80, z180, z80-z80n and Rabbit processors) as well as other related (but unsupported by z88dk) processors: 8085, R800, ez80.

## Usage

At its simplest:

    z88dk-dis -o [address] [binary file]

Will load binary file at the specified address and start disassembling from that point.

The diassembler understands the .map files generated by z80asm, and these will be read using the `-x` option:

    z88dk-dis -o [address] -x [map file] [binary file]

### Specifying the CPU

The following options control which CPU z88dk-dis will interpret the binary as.

| Flag | CPU | Notes |
|-|-|-|
| -mz80 | Z80 | |
| -mz180 | Z180 | |
| -mez80 | ez80 | Short mode only |
| -mz80-z80n | ZX-Next | Supports the extended opcodes in the ZX Next CPU |
| -mr2k | Rabbit 2000 | |
| -mr3k | Rabbit 3000 | |
| -mr800 | Ascii R800 | As used in the MSX Turbo |
| -mgbz80 | Gameboy z80 | |
| -m8080 | Intel 8080 | Using Z80 Opcodes |





