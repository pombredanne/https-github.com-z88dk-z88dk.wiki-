## Input Files
[[Top](Tool---z80asm)]

**z80asm** reads text files in the syntax supported by the specific processor 
being assembled for (see -m option) and produces the corresponding object files.

## Input Lines

The assembler parses source files with any of the common end-of-line 
termination sequences (CR, LF or CR-LF). Each line starts with an optional 
label and can contain assembly directives (i.e. instructions to the assembler),
assembly instructions (i.e. code to be translated into object code for the 
specific processor) or blanks and comments.

Differently to most other assemblers, white space is not significant, i.e. a 
label can be defined after white space, and an opcode can be written at column 
1.

### Comments

Comments start with a semi-colon (```;```) and end at the end of the line.

### Symbols

All symbols in the code (labels, variables, ...) are named with unique 
identifiers. Identifiers start with a letter or underscore (```_```), and can 
contain letters, underscores or digits. Identifiers are case-sensitive.

### Labels

A label is defined at the start of a line by prefixing a symbol with a dot (
```.```) or suffixing it with a colon (```:```), i.e. either ```.label``` or 
```label:```. The symbol may be used to refer to address in the object code.

### Numbers

The assembler accepts numbers in decimal, hexadecimal and binary. 
Different syntax is allowed to simplify porting of code written for other 
assemblers. Some of the prefix characters are also used as operators; in this 
case a space may be needed after the prefix to define it as an operator:

```
   ld a, %10     ; A = 2 (10 binary)
   ld a, 12 % 10 ; A = 2 (remainder of 12 divided by 10)
```

All expressions are computed as signed integers with the host platform's integer size (32-bit or 64-bit in the most common platforms).

**Decimal** numbers are a sequence of decimal digits (```0..9```). Leading 
zeros are insignificant - note the difference from C, where a leading zero 
means octal.

```
  ld a, 99
```

**Hexadecimal** numbers are a sequence of hexadecimal digits (```0..9``` and 
```A..F```, case-insensitive), either prefixed or suffixed with an hexadecimal 
marker. If the marker is a suffix, and the number starts with a letter, then a 
leading zero has to be added.

```
  ld a, $FF
  ld a, 0xFF
  ld a, 0FFh
```

**Binary** numbers are a sequence of binary digits (```0..1```), either 
prefixed or suffixed with a binary marker. 

```
  ld a, %11
  ld a, @11
  ld a, 0b11
  ld a, 11b
```

Binary numbers can be specified as **bitmaps**, with ```#``` as ```1``` and 
```-``` as ```0```, using the binary prefix (```@``` or ```%```) immediately 
followed by a double-quoted string of hashes and dashes.

```
  defb @'---##---'
  defb @"-##--##-"
  defb %'-##-----'
  defb %'-##-----'
  defb @"-##--##-"
  defb @'---##---'
```

### Keywords

Processor registers (```BC```, ```DE```, ...) and flags (```NZ```, ```Z```, 
...), and assembly ```ASMPC```, representing the current assembly location, are
reserved keywords. They cannot be used as identifiers, and are 
case-insensitive.

### Directives and Opcodes

Assembler directives (```ORG```, ```INCLUDE```, ...) and processor opcodes (
```NOP```, ```LD```, ...) are interpreted as directives or opcodes when 
appearing at the start of the statement or after a label definition, or as 
regular identifiers otherwise. The directives and opcodes are case-insensitive.

```
  jr: jr jr  ; silly example, jr is both a label and an opcode
             ; while correct code, it's confusing
```
