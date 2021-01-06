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


### stdarg.h

The default calling convention for sccz80 is `__smallc`. This makes dealing with variadic arguments tricky. For more details see https://github.com/z88dk/z88dk/issues/330#issuecomment-324791397 or alternatively define variadic functions as `__stdc` and define `__Z88DK_R2L_CALLING_CONVENTION` prior to inclusion of `<stdarg.h>`

## Compiler (zsdcc)

### --reserve-regs-iy__

Use of this option can sometimes lead to buggy code generation when accessing global or static variables.  Some peephole rules have been added at -SO2 level to correct this issue and this looks to have been successful.

The option will be ignored and the compiler will use iy if the stack frame is greater than 128 bytes in size.  The size of the stack frame is determine by the size of local variables declared.  One key difference is when this flag is active, the compiler will not expect iy's value to be preserved across function calls.

### compiler sometimes fails when generating code for 64-bit integers

The issue can arise when the compiler tries to keep a 64-bit value in registers when it is reused in a following statement.

<code>
z = llabs(x);
lltoa(z, buffer, 10);
</code>

The compiler may fail with a fatal error when it tries to reuse "z" from the assignment in the following call to lltoa().

When this occurs, the error can be circumvented by inserting a dummy function call between the two statements.  The dummy function "intrinsic_stub()" from <intrinsic.h> is ideal for this purpose as it results in no code being inserted between the statements.

### sdcc critical language extension bugged for nmos z80s

sdcc implements a %%__critical { ... }%% block that will disable interrupts while the enclosed code runs.  The interrupts are not simply disabled; instead the current interrupt state is read and then after the critical block, interrupts are only re-enabled if they were previously enabled.  In order to learn the current interrupt state, the instruction "ld a,i" is executed.  On cmos z80s this is a reliable way to find out if interrupts are enabled but on nmos z80s it isn't.  So these critical sections are in fact unreliable on nmos z80s.

This issue has been fixed at the -SO2 optimization level.  sdcc's inlined code to implement critical sections are replaced with library calls.  Since the library knows whether the target is a cmos or nmos z80, it can provide the correct code to implement the desired behaviour.

### Clumsy access of static variables

Code generated to access globals and static variables can often be sub-optimal.  Peephole rules have been added at -SO3 level to help mitigate this.


## Assembler (z80asm)