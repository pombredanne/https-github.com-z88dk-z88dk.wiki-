# The Inline Assembler

The Inline Assembler permits to easily include machine code portions in a C source:

   int one_hundred()
   {
      #asm
      ld hl,100
      #endasm
   }


### Extensions

sccz80 features several extensions to C to enable mixing of C and assembler:

    return_c                - As return but sets carry flag
    return_nc               - As return but resets carry flag
    iferror{...} else {..}  - As "if { }" but checks carry flag
                              (does else {...} if nc )


### Caveats

The optimizer is active on the inline portions of code, too.
To keep it unchanged it is suggested to export it in a pure assembler module and to link it into a library.


