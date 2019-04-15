## Compiler (sccz80)

### File Names

The Windows version might get in trouble with uppercase source file names.
When in trouble try renaming your files.

Additionally, avoid program/module names beginning with numbers or symbols.

### Bitfields

Bitfields are not supported. They will compile, but will use the full datatype rather than the appropriate number of bits.

### Expression Folding

Expressions are evaluated left to right, and no folding is applied, as a result ''(1 + j + 3)'' will generate bulkier code than ''( j + 1 + 3 )''.


## Compiler (zsdcc)

### Struct assignment

The current version of zsdcc used (3.8.5) does not support assigning of structs. This limitation will be removed when z88dk upgrades to 3.9.0

## Assembler (z80asm)