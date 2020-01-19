The entire library is accessible from assembly language.  The assembly entry points for functions are prefixed with "asm_" and the register interface for each function is documented in the source code rooted in {z88dk}/libsrc/_DEVELOPMENT.  Here is a brief example that makes use of stdlib's dtoa() function to convert a double to ascii text:

<code>
; assumes math48 is the math library linked

SECTION code_user

EXTERN asm_dtoa

exx
ld bc,$490F      ; AC' = pi
ld de,$DAA2      ; math48 uses BCDEHL to hold a double
ld hl,$2182
exx

ld c,0           ; no flags
ld de,-1         ; max precision
ld hl,buffer     ; destination buffer

call asm_dtoa    ; write double in ddd.ddd format to buffer

...

SECTION bss_user

buffer:   defs 32
</code>

asm_dtoa is located in {z88dk}/libsrc/_DEVELOPMENT/stdlib/z80.  The comments detail input parameters, output parameters and registers modified.  The C documentation can also be consulted for more details.

Vararg functions such as printf expect their parameters to be pushed onto the stack.  In these sorts of cases, the asm caller must use standard C linkage to call the function.  Here is a brief example that uses printf:

<code>
; assumes sccz80 is the compiler
; L->R parameter order, varargs require A to be loaded with num words pushed onto stack

SECTION code_user

EXTERN asm_printf

ld hl,fmt       ; format string
push hl
ld hl,100       ; 100 dollars (16-bit integer)
push hl
ld a,2          ; sccz80 only, number of words pushed
call asm_printf
pop af
pop af          ; clear stack

...

SECTION rodata_user

fmt:  defm "You win %d dollars.\n"
      defb 0
</code>

The equivalent C is:

<code c>
printf("You win %d dollars.\n", 100);
</code>

If this is a project compiled with sccz80 or if this is an assembly language project linked against the sccz80 library, then printf is expecting its parameters to be pushed in left-to-right order and the 'A' register must be loaded with the number of 16-bit words pushed.

On the other hand if the project is compiled with sdcc or if this is an assembly language project linked against the sdcc library, then printf is expecting its parameters to be pushed in right-to-left order and nothing needs to be loaded into 'A'.

More details can be found in the [[front#mixing_c_and_assembly_language|Mixing C and Assembly]] topic.  Most library functions are not vararg and asm parameters will be passed via register rather than stack.

You must also be aware that the new c library employs **sections**.  Sections are destination containers that hold code and/or data and can have an ORG address associated with them.  All assembly language written should be assigned to a section so that the linker can know where to place it in memory.

The crts previously discussed create three basic sections: CODE, DATA and BSS.  These large sections hold many small ones, including some sections designated for user code:

  * **code_user**  assign executable code to this section
  * **rodata_user**  assign read-only data to this section
  * **smc_user**  assign self-modifying code to this section
  * **data_user**  assign non-zero initial data to this section
  * **bss_user**  assign zero initial data to this section

By assigning your assembly code to the correct sections, the linker will be able to create ROMable software with your code.

You can also create your own sections.  There's no magic incantation, simply start using it and assign a name as in:

<code>
SECTION my_section

start:

    ld hl,2
    ....
    ret
</code>

Since this section is not in the crts' memory map, all data or code assigned to it will be output as a separate binary when the project is assembled.  The name of the binary will be "outputname_my_section.bin".  If no ORG is assigned to the section anywhere in your project, it is assigned an ORG of 0.  (Note that un-ORGed sections may be appended to previously defined sections in the same source file; this is how memory maps are built with z80asm).

These details are best described in the [[front#mixing_c_and_assembly_language|Mixing C and Assembly]] topic.