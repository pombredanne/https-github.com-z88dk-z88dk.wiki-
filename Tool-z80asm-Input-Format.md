## Input Files

**z80asm** reads text files in the syntax supported by the specific processor being assembled for (see -m option) and produces the corresponding object files.

An input file with a ```.o``` extension is assumed to be already in object file format and is just read by the linker. Any other extension is considered an assembly source file (conventionally ```.asm```).

A project list file may be supplied in the command line prefixed by the at-sign (e.g. ```@project.lst```). The list file contains one input file name per line, or another project list prefixed with an at-sign, which is opened recursively. Hash signs (```#```) and semi-colons (```;```) mat be used at the start of lines in the list files to include comments.

Both the command line and the list files support wild-cards to expand all the files that match the given pattern.  
*Note* that the Operating System may do its own wildcard expansion and the pattern may need to be quoted in the command line.

A single star in a file name (```*```) expands to the list of all files/directories that match the complete pattern, where the star represents any sequence of characters. A double-star in a directory name (```**```) expands to the complete directory sub-tree, when searched recursively.

## Input Lines

The assembly source code is processed line-by-line. Each line starts with an optional label and can contain assembly directives (i.e. instructions to the assembler), assembly instructions (i.e. code to be translated into object code for the specific processor) or blanks and comments. 

Differently to most other assemblers, white space is not significant, i.e. a label can be defined after white space, and an opcode can be written at column 1.

### Comments

Comments start with a semi-colon (```;```) and end at the end of the line.

### Symbols

Symbols must start with a letter or underscore character (```_```), and can have letters, digits or underscores and are case-sensitive.

### Labels

A label is defined at the start of a line by prefixing a symbol with a dot (```.```) or suffixing it with a colon (```:```), i.e. either ```.label``` or ```label:```. The symbol may be used to refer to address in the object code.
