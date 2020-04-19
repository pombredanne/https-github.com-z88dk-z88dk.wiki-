## Compiler (sccz80)

### Designated initialisers

So far, sccz80 does not support designated initialisers.

### File Names

The Windows version might get in trouble with uppercase source file names.
When in trouble try renaming your files.

Additionally, avoid program/module names beginning with numbers or symbols.

### sccz80 will sometimes demote longs to ints in mixed expressions

To avoid this problem use explicit casting rather than rely on expected implicit conversions.

```
long a, c;
int b;

c = a + b;          // may result in improper demotion of long to int before the addition
c = a + (long)(b);  // will always work
```

It's a good coding practice to explicitly cast types where type conversion is required.

### Expression Folding

Expressions are evaluated left to right, and no folding is applied, as a result ''(1 + j + 3)'' will generate bulkier code than ''( j + 1 + 3 )''.


## Compiler (zsdcc)

### Struct assignment

The current version of zsdcc used (3.8.5) does not support assigning of structs. This limitation will be removed when z88dk upgrades to 3.9.0

## Assembler (z80asm)