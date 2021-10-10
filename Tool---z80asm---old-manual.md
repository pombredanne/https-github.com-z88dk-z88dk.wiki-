THE Z80 CROSS ASSEMBLER (z88dk)
===============================
[[Top](Tool---z80asm)]

**This document is not up-to-date**

Version: v2.6.1 (October 3rd, 2014)

Thank you for purchasing a copy of this cross assembler. We have made an effort
to program an easy user interface and efficient assembler source file 
compiling. The object file and library format is an invention of our own and 
has also been included in this documentation. We hope that you will enjoy your 
Z80 machine code programming with our assembler.

We have made an effort to produce a fairly easy-to-understand documentation. If
you have any comments or corrections, please don't hesitate to contact us:

Gunther Strube  
Gl. Kongevej 37, 4.tv.  
DK-1610 Kobenhavn V  
Denmark  
e-mail [gbs@image.dk](mailto:gbs@image.dk)

6\. Using expressions
---------------------

Expressions are almost unavoidable in source files. They define and explain 
things much clearer than just using a constant. The Z80 Module Assembler allows
expressions wherever a parameter is needed. This applies to Z80 mnemonic 
instructions, directives and even in character strings. The resulting value 
from an evaluated expression is always an integer. All expressions are 
calculated internally as 32-bit signed integers. However, the parameter type 
defines the true range of the expression. E.g. you cannot store a 32-bit signed
integer at an 8-bit LD instruction like LD A, \<n\> . If a parameter is 
outside an allowed integer range an assembly error is reported. Finally, no 
floating point operations are needed by the assembler. There is no real 
standard on Z80 based computers.

Whenever an integer is stored in a Z80 file, the standard Zilog notation is 
used, i.e. storing the integer in low byte, high byte order (this also applies 
to 32-bit integers). This standard is also known as little endian notation 
(also used by INTEL processors).

Expressions may be formed as arithmetic and relational expressions. Several 
components are supported: symbol names (identifiers), constants, ASCII 
characters and various arithmetic operators.

### 6.1. Constant identifiers

Apart from specifying decimal integer numbers, you are allowed to use 
hexadecimal constants, binary constants and ASCII characters. The following 
symbols are put in front of the constant to identify the type:

    $ : hexadecimal constant, e.g. $4000 (16384).  
    @ : binary constant, e.g. @11000000 (192).  
    ' ' : ASCII character, e.g. 'a'.

### 6.2. Arithmetic operators

All basic arithmetic operators are supported: addition, subtraction, 
multiplication, division and modulus. In addition binary logical operators are 
implemented: binary AND, OR and XOR.

    + : addition, e.g. 12+13  
    - : unary minus, subtraction. e.g. -10, 12-45  
    * : multiplication, e.g. 45*2 (90)  
    / : division, e.g. 256/8 (32)  
    % : modulus, e.g. 256%8 (0)  
    ** : power, e.g. 2**7 (128)  
    & : binary AND, e.g. 255 & 7 (7)  
    | : binary OR, e.g. 128 | 64 (192)  
    ^ : binary XOR, e.g. 128 ^ 128 (0)  
    ~ : binary NOT, e.g. (~0xAA) & 0xFF (0x55)

Arithmetic operators use the standard operator precedence, shown from highest 
to lowest:

    constant identifiers  
    () ** */% +-&|^~

If you want to override the default operator precedence rules, use brackets ().

### 6.4. Relational operators

With relational operators you may form logical expressions resulting in true or
false conditions. The resulting value of a true expression is 1. The resulting
value of a false expression is 0. These operators are quite handy when you 
need to perform complex logic for conditional assembly in IF-ELSE-ENDIF 
statements. The following relational operators are available:

    = : equal to  
    <> :not equal to  
    < : less than  
    > : larger than  
    <= : less than or equal to  
    >= : larger than or equal to  
    ! : not

You may link several relational expressions with the binary operators AND, OR 
and XOR. You have all the possibilities available!

It is perfectly legal to use relational expressions in parameters requiring an 
arithmetic value. For example:

    LD A, (USING_IBM = 1) | RTMFLAGS

### 6.5. The ASMPC standard function

In occasional circumstances it may be necessary to use the current location of 
the assembler program counter in an expression e.g. calculating a relative 
distance. This may be done with the help of the ASMPC identifier. An example:

    .errmsg0 DEFB errmsg1 - ASMPC - 1 , "File open error"  
    .errmsg1 DEFB errmsg2 - ASMPC - 1 , "Syntax error"  
    .errmsg2

Here, a length byte of the following string (excluding the length byte) is 
calculated by using the current ASMPC address value.

### 6.6. Symbol identifiers in expressions

Apart from using integer constants in your expressions, names are allowed as 
well. This is frequently used for symbolical address label references (both 
external and local).

Forward referencing of symbols is not really something that is important in 
evaluating expressions. The logic is built into the assembler parser. If an 
expression cannot be resolved in pass 1 of source file parsing, it will try to 
re-evaluate the failed expression in pass 2 of the parsing. If it still fails a
symbol has not been found (XREF and LIB external symbols are handled during 
the linking phase).

