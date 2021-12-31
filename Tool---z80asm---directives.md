## Directives
[[Top](Tool---z80asm)]

*z80asm* accepts the following assembly directives.

### ASSERT expression

Evaluate the constant expression and abort assembly with an assertion failure error if the expression is false. 

### ALIGN expression[, filler]

The first `ALIGN` statement in a section tells the linker to reserve space 
before the start of the section so that the section start address is a multiple 
of the constant *expression*.

A subsequent `ALIGN` statement is translated into `DEFS`.

### BINARY|INCBIN "filename"

Loads a binary file at the current location in the object file.

### CALL\_OZ expression

Calls the Z88 operating system. The *expression* is a 16-bit expression and must 
evaluate to a constant. This is assembled as follows:
```
RST $20  
DEFB x ; 8-bit parameter
```
or
```
RST $20  
DEFW x ; 16-bit parameter
```

### CALL\_PKG expression

Calls the Z88 operating system. The *expression* is a 16-bit expression and must 
evaluate to a constant and be different from zero. This is assembled as follows:
```
RST $08  
DEFW expression
```

### DEFB|DB|DEFM|DM|BYTE expression|"string"[,...]

Stores a sequence of bytes at the current location. 

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

Strings are interpreted in the host operating system character set and may 
contain C-escape sequences.

### DEFW|DW|WORD expression[,...]

Stores a sequence of 16-bit words in **low-high** byte order (little endian)
at the current location. 

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

### DEFDB|DDB expression[,...]

Stores a sequence of 16-bit words in **high-low** byte order (big endian)
at the current location. 

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

### DEFP|DP|PTR expression[,...]

Stores a sequence of 24-bit words in low-high byte order (little endian)
at the current location. 

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

### DEFQ|DQ|DWORD expression[,...]

Stores a sequence of 32-bit words in low-high byte order (little endian)
at the current location. 

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

### DEFC|DC name = expression, ...

Define a symbol variable that may either be a constant or an expression.

Expressions may be computed at the link phase, i.e. they may refer to external 
or address symbols.

### DEFGROUP { name[=expression], ... }

Define a set of symbols like the C *enum* statement. The symbol values start
at zero and are incremented for each consecutive symbol, or a new value can be
defined with the optional constant expression, e.g.

```
  DEFGROUP  
  {  
     sym_null  
     sym_ten = 10, sym_eleven, sym_twelve  
  }
```

### DEFINE name,...

Defines a set of symbols as logically true (1).

### DEFS size[, fill_byte]

Allocate a storage area of the given *size* with the given *fill_byte*, 
where *size* and *fill_byte* are constant expressions. 

If the *fill_byte* is not supplied, it defaults to zero or to the value
supplied in the command line `-fVALUE` option.

### DEFS|DS size, "string"

Create a fixed-length string, filling the remaining space, if any, with the 
filler byte (default zero or the value of the `-fVALUE` option), e.g.

```
  DEFS 10, "hello" ; => "hello", 0,0,0,0,0
```

### DEFVARS origin { [name] size_id count, ... }

Define a set of symbols like the C *struct* statement. 

*origin* is a constant expression that defines the base address of the structure
elements. If *origin* is -1, then the structure elements follow the values of the
previous `DEFVARS` statement that had an *origin* different from zero. The *origin*=0 structures are not continued with -1.

*name* is the symbol that is defined with the structure offset, i.e. *origin*
for the first symbol, and incremented by the size of each element for the
subsequent elements.

*size_id* is one of the following identifiers:

| size\_id | size
|---       |---
| DS.B     | 8-bits
| DS.W     | 16-bits
| DS.P     | 24-bits
| DS.Q     | 32-bits

*count* is the number of *size_id* elements stored at the location.

Example:
```
  DEFVARS $4000 
  {
    v1    DS.B 1          ; v1 = $4000
    v2    DS.B 3          ; v2 = $4001
    v3    DS.W 1          ; v3 = $4004
          DS.W 1          ; reserve one word
  }
  DEFVARS 0               ; stand-alone struct
  {
    prev  DS.W 1          ; prev = 0
    next  DS.W 1          ; next = 2
    data  DS.w 1          ; data = 4
  }
  DEFVARS -1              ; continue $4000 struct
  {
    v4    DS.Q 1          ; v4 = $4008
  }
```

### EXTERN|XREF|LIB name,...

Declares symbols as external to the current module. These symbols are declared
`PUBLIC` in some other module and the references are resolved during linking.

### FLOAT expresion,...

Defines a list of constant floating point expressions that are evaluated and 
sent to the output in the current format set by `SETFLOAT`.

The expressions can use:
- integer of floating point numbers
- sub-expressions in parenthesis
- the constants `pi` and `e`
- the operators + - \* / \*\* (power)
- the functions sin cos tan asin acos atan atan2 sinh cosh tanh asinh acosh atanh log log10 log2 exp exp2 pow sqrt cbrt ceil floor trunc round abs hypot fmod


### FPP expression

Calls the Z88 operating system floating point library. The *expression* is a
8-bit expression and must evaluate to a constant. This is assembled as follows:

```
  RST $18  
  DEFB expression
```

### GLOBAL name, ...

Declares symbols `PUBLIC` if they are defined locally in the module or 
`EXTERN` otherwise.

### IF expression ... [ELIF expression] ... [ELSE] ... ENDIF

Evaluate the constant *expression* and assembles the lines up to the `ELSE`
clause if it is true (i.e. not zero), or the lines from `ELSE` to `ENDIF`
otherwise. The `ELSE` clause is optional. This structure may be nested.

Optional `ELIF` clauses may be added to check for other conditions if the 
previous conditions failed.

### IFDEF name ... [ELIFDEF name] ... [ELSE] ... ENDIF

Check if the give symbol name is defined and assembles the lines up to the `ELSE`
clause if true (i.e. defined), or the lines from `ELSE` to `ENDIF` otherwise. 
The `ELSE` clause is optional. This structure may be nested.

Optional `ELIFDEF`|`ELINDEF` clauses may be added to check for other symbols 
if the previous conditions failed.

### IFNDEF name ... [ELIFNDEF name] ... [ELSE] ... ENDIF

Check if the give symbol name is defined and assembles the lines up to the `ELSE`
clause if false (i.e. not defined), or the lines from `ELSE` to `ENDIF` 
otherwise. The `ELSE` clause is optional. This structure may be nested.

Optional `ELIFDEF`|`ELINDEF` clauses may be added to check for other symbols 
if the previous conditions failed.

### INCLUDE "filename"

Include the given file in the current assembly position. The file is searched in
the path pointed by the `-IPATH` options. Detects and reports an attempt to
recursively include the same file.

### INVOKE expression

Special CALL instruction for the TI83 calculator; it is coded as a `RST 28H` 
followed by the expression if the `-mti83plus` option is given, or as a normal 
`CALL` instruction otherwise.

### LINE|C_LINE line_num[, "filename"]

Tells the assembler that the next input line is from the given source file and
line number. This information is used for error messages.

`LINE` is used to synchronize the assembly source line number.

`C_LINE` is used by the C-compiler to point the error messages to the original
C source file.

### LSTOFF

Switches off temporarily the generation of a list file.

### LSTON

Switches on the generation of a list file, if enabled by the command line
option `-l`.

### MODULE name

Defines the object file module name, which defaults to a name derived from the
object file name. 

### ORG expression

Tells the linker that this section should start at the given constant *expression*
value. This value can be overridden with the `-rVALUE` option.

When assembling programs with multiple sections, a section without an `ORG` will 
be appended to the end of the previous section. A section with a defined `ORG` 
will generate its own binary file, e.g. `file_section.bin`.

A section may contain `ORG -1` to tell the linker to split the binary file of 
this section, but continue the addresses sequence from the previous section.

With the option `-split-bin` all sections generate their own `file_section.bin`.

### PHASE expression ... DEPHASE

Assemble a group of code at the current position, but resolve all the addresses
as if `ORG expression` had been called. Allows code to be stored at an address 
and to be copied by the runtime to another address.

### PUBLIC|XDEF|XLIB name,...

Declares symbols as exported from the current module. These symbols are 
available to be used from other modules by declaring them `EXTERN` and the 
references are resolved during linking.

### SECTION name

Starts a new section with the given `name`.

Sections allow the code to be mapped to the target architecture memory map 
by the linker. The linker lays out the sections in the order it sees them
in the object modules being linked, and joins together code from different
modules with the same section name. It considers the `ORG` and `ALIGN`
attributes of the section when laying out the code in the memory map.

A C-compiler may define a set of sections in its `crt` (e.g. `text`, `data`
and `bss`). As long as the `crt` module is first in the list of linked object
modules, then all the text, data and bss object code will be laid together
in that order. 

### SETFLOAT type

Sets the floating point format used by subsequent `FLOAT` statements. The `type` is one of:

| type  | z80asm option | zcc option    | description
|---    |---            |---            |---
|genmath|-float=genmath |-l             |Classic z80 mode 48-bit format; this is the default
|math48 |-float=math48	|-lmath48       |Same format as genmath
|ieee16 |-float=ieee16  |--math16       |16 bit IEEE-754
|ieee32 |-float=ieee32  |--math32       |32 bit IEEE-754
|ieee64 |-float=ieee64  |&nbsp;         |64 bit ieee-754
|z80    |-float=z80     |-fp-mode=z80   |Classic z80 mode with 48 bits
|zx81   |-float=zx81    |&nbsp;         |ZX-81 40 bit format
|zx     |-float=zx      |&nbsp;         |ZX-Spectrum 40 bit format
|z88    |-float=z88     |-math-z88      |40 bit format for Z88
|mbfs   |-float=mbfs    |-fp-mode=mbf32 |32 bit Microsoft single precision
|mbf40  |-float=mbf40   |-fp-mode=mbf40 |40 bit Microsoft
|mbf64  |-float=mbf64   |-fp-mode=mbf64 |64 bit Microsoft double precision
|am9511 |-float=am9511  |-fp-mode=am9511|AM9511 math processor format

### UNDEFINE name,...

Removes the definition of a set of symbols, if they exist.

