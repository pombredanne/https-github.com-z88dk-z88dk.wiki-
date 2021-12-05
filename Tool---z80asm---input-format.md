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
label can be defined after white space, and an opcode can be written at column 1.

### Continuation Lines

Lines ending with a backslash are logically continued on the next line. 
This allows breaking a long line, e.g.

    ld a,\
    1           ; -> 3e 01

### Multiple Statements

Multiple statements can be written in a physical line by separating
them with a backslash, e.g.

    ld a, 1 \ ret   ; -> 3e 01 c9

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
