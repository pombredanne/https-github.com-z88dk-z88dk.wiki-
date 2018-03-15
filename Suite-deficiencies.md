## Compiler (sccz80)

### File Names

The Windows version might get in trouble with uppercase source filen names.
When in trouble try renaming your files.

Additionally, avoid program/module names beginning with numbers or symbols.

### Bitfields

Bitfields are not supported. They will compile, but will use the full datatype rather than the appropriate number of bits.

### Expression Folding

Expressions are evaluated left to right, and no folding is applied, as a result ''(1 + j + 3)'' will generate bulkier code than ''( j + 1 + 3 )''.

### Struct assignment

Copying a complete structures is not supported:

    struct s {
        char    val[10];
    };

    struct s t;
    struct s v;

    int func() {
        v = t;
    }

Will not compile, to solve use memcpy.

## Compiler (zsdcc)

### Struct assignment

As sccz80, zsdcc does not support assigning of structs, the same solution is recommended.

## Assembler (z80asm)