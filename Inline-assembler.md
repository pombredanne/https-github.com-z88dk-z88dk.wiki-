# The Inline Assembler

The inline Assembler permits to easily include machine code portions in a C source:

```
   int one_hundred()
   {
      __asm
      ld hl,100
      __endasm;
   }
```


### Caveats

The optimizer is active on the inline portions of code, too. When compiling with zsdcc including inline assembler will clobber certain rules.

It's thus recommended to isolate assembler into a separate file and include it via linking rather than inline assembly.

