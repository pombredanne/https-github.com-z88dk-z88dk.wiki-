## z80asm File formats (v16)
[[Top](Tool---z80asm)]

This document describes the object and libary formats used by *z80asm*. 

The object and library files are stored in binary form as a set of 
contiguous sections, i.e. each section follows the previous one without 
any end-markers. 

The files start with a signature that includes a version number and is 
verified by the assembler before trying to parse the file.

A set of file pointers at the start of the object file point to the 
start of each section existing the in the file, or contain *0xFFFFFFFF* 
(-1) if that section does not exist.

The following object types exist in the file:

* *char* :	one byte containing an ASCII character;

* *byte* :	one 8-bit value;

* *word* :	one 16-bit value, stored in low byte - high byte order 
  (little endian format - Z80/Intel notation);

* *long* :	one 32-bit value, stored in low byte - high byte order;

* *string*:	one *byte* containing the string length followed by the 
characters of the string;

* *lstring*: one *word* containing the string length followed by the 
characters of the string.


Object file format
------------------

The format of the object file is as follows:

|Addr  |Type   |Object|
|---   |---    |---   |
|&nbsp;|&nbsp; |**File header**|
|   0  |8 chars|'Z80RMF16' - file signature and version|
|   8  | long  | File pointer to *Module Name*, always the last section |
|  12  | long  | File pointer to *Expressions*, may be -1               |
|  16  | long  | File pointer to *Module Names*, may be -1              |
|  20  | long  | File pointer to *External Names*, may be -1            |
|  24  | long  | File pointer to *Machine Code*, may be -1              |
|&nbsp;|&nbsp; |**Expressions**<br>expressions defined in this module
|expr+0| char  |type:<br>0 : end marker<br>'U' : 8-bit integer (0 to 255)<br>'S' : 8-bit signed integer (-128 to 127)<br>'u' : 8-bit integer (0 to 255) extended to 16 bits (appending a 0)<br>'s' : 8-bit signed integer (-128 to 127) sign-extended to 16 bits (appending a 0xff if negative or 0 otherwise)<br>'C' : 16-bit integer, little-endian (-32768 to 65535)<br>'B' : 16-bit integer, big-endian (-32768 to 65535)<br>'P' : 24-bit signed integer<br>'L' : 32-bit signed integer<br>'J' : 8-bit jump relative offset<br>'H' : byte offset to 0xFF00<br>'=' : computed name at link time
|&nbsp;|lstring|source file name where expression was defined, to be used in error messages. May be an empty string, signalling that the previous *sourcefile* of the previous expression should be used
|&nbsp;|long   |line number in source file where expression was defined
|&nbsp;|lstring|section: source file section name where expression was defined
|&nbsp;|word   |*ASMPC*: relative module code address of the start of the assembly instruction to be used as *ASMPC* during expression evaluation
|&nbsp;|word   |*patchptr*: relative module code patch pointer to store the result of evaluating the expression
|&nbsp;|lstring|*target_name*: name of the symbol that receives the result of evaluating the expression, only used for '=' type expressions, empty string for the other types
|&nbsp;|lstring|*expression*: expression text as parsed from the source file
|...|...|...|
|&nbsp; |&nbsp; |**Module Names**<br>symbols defined in this module
|names+0| char  |*scope* of the name:<br>0  : end marker<br>'L' is local<br>'G' is global
|&nbsp; | char  |*type*: defines whether it is a:<br>'A' relocatable address<br>'C' a constant<br>'=' a symbol computed at link time, the corresponding expression is in the *Expressions* section
|&nbsp; |lstring|*section*: source file section name where name was defined
|&nbsp; | long  |*value*: contains the absolute value for a constant, or the relative address to the start of the code block for a relocatable address
|&nbsp; |lstring|*name*: contains the name
|&nbsp; |lstring|*filename*: contains the source file name where the symbol was defined
|&nbsp; | long  |*line_nr*: contains the source line number where the symbols was defined
|...|...|...|
|&nbsp; |&nbsp; |**External Names**<br>external symbols names referred by this module; section ends at the *Module Name* section
|names+0|lstring|*name*: symbol contains
|...|...|...|
|&nbsp; |&nbsp; |**Module Name**<br>module name
|name+0 |lstring|*name*: module name
|&nbsp; |&nbsp; |**Macine Code**<br>contains the binary code of the module
|code+0 | long  |*length*: defines the total code length (-1 to signal the end)
|&nbsp; |lstring|*section*: source file section name
|&nbsp; | long  |*ORG Address*: contains the user defined ORG address for the start of this section,<br>-1 for no ORG address was defined, or<br>-2 to split section to a different binary file<br>If multiple sections are given with an ORG address each, the assembler generates one binary file for each section with a defined ORG, followed by all sections without one
|&nbsp; | long  |*ALIGN*: defines the address alignment of this section,<br>-1 if not defined<br>The previous section is padded to align the start address of this section
|&nbsp; |byte[length]|*code*: contains the binary code
|...|...|...|

Library file format
-------------------

The library file format is a sequence of object files with additional
structures.

|Addr  |Type   |Object|
|---   |---    |---   |
|&nbsp;|&nbsp; |**File header**
|   0  |8 chars|'Z80LMF16' - file signature and version
|&nbsp;|&nbsp; |**Object Modules**<br>object files in the library
|mod+0 | long  |*next*: contains the file pointer of the next object file in the library, or<br>*-1* if this object file is the last in the library
|&nbsp;| long  |*length*: contains the length of this object file, or<br>*0* if this object files has been marked "deleted" and will not be used
|&nbsp;|byte[length]|contains the object module
|...|...|...|

History
-------

|Version|Description
|---    |---
|*01*   |original z80asm version
|*02*   |allow expressions to use standard C operators instead of the original (legacy) z80asm specific syntax
|*03*   |include the address of the start of the assembly instruction in the object file, so that expressions with ASMPC are correctly computed at link time;<br>remove type 'X' symbols (global library), no longer used
|*04*   |include the source file location of expressions in order to give meaningful link-time error messages
|*05*   |include source code sections
|*06*   |incomplete implementation, fixed in version *07*
|*07*   |include *DEFC* symbols that are defined as an expression using other symbols and are computed at link time, after all addresses are allocated
|*08*   |include a user defined ORG address per section
|*09*   |include the file and line number where each symbol was defined
|*10*   |allow a section alignment to be defined
|*11*   |allow big-endian 16-bit expressions to be patched; these big-endian values are used in the ZXN coper unit
|*12*   |allow the target expression of relative jumps to be computed in the link phase
|*13*   |add 8-bit signed and unsigned values extended to 16-bits
|*14*   |add 24-bit pointers
|*15*   |add byte offset to 0xFF00
|*16*   |convert all *string* to *lstring* in the object file to allow for very large symbol names, needed for C debugging symbols

