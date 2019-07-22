## Input Files

z80asm reads text files in the syntax supported by the specific processor being assembled for (see -m option) and produces the corresponding object files.

An input file with a ```.o``` extension is assumed to be already in object file format and is just read by the linker. Any other extension is considered an assembly source file (conventionally ```.asm```).

## Input Lines

The assembly source code is processed line-by-line. Each line starts with an optional label and can contain assembly directives (i.e. instructions to the assembler), assembly instructions (i.e. code to be translated into object code for the specific processor) or blanks and comments. Comments start with a semi-colon (```;```) and end at the end of the line.

Differently to most other assemblers, white space is not significant, i.e. a label can be defined after white space, and an opcode can be written at column 1.

Symbols must start with a letter or underscore character (```_```), and can have letters, digits or underscores and are case-sensitive.

A label is defined by prefixing a symbol with a dot (```.```) or suffixing it with a colon (```:```), i.e. either ```.label``` or ```label:```.

[not yet released] A line can contain multiple assembly statements, separated either by the backslash (```\```) or colon (```:```) characters.
