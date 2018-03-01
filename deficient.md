# Suite Deficiencies

## Compiler (sccz80)

### File Names

The Windows version might get in trouble with uppercase source filen names.
When in trouble try renaming your files.
Also avoid program/module names beginning with numbers or symbols.


### Bitfields

Bitfields are not supported.

### Expression Folding

Expressions are evaluated left to right, and no folding is applied, as a result ''(1 + j + 3)'' will generate bulkier code than ''( j + 1 + 3 )''.


### Copy of struct elements in an array

Copying a whole 'struct' node in an array in another position (or another array) won't work, only the first 2 bytes will be moved.
This won't work correctly:

    struct segment {
      int x;
      int y;
    } data[10];
    
    data[2].x=6;
    data[2].y=7;
    
    data[1]=data[2];

To solve for small structs, copy the elements one by one.

    data[1].x=data[2].x;
    data[1].y=data[2].y;

For larger structs, it may be more efficient to copy using memcpy().

    memcpy(&data[1], &data[2], sizeof(struct segment));

## Assembler (z80asm)
