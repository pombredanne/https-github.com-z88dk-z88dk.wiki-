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

2\. An overview of assembler features and related files
-------------------------------------------------------

### 2.1. The Z88 operating system definition files

You will find header files containing all operating system definitions as 
defined in the Z88 Developers' Notes V3 in the "OZdefc.zip" file. This makes 
the operating system interface programming a lot easier.

### 2.2. The supplied standard library Z80 source files

We have supplied a standard library with useful routines for both beginners and
professional machine code programmers. All source files are supplied for 
having the opportunity to study and improve the routines. However some routines
are programmed especially for the Z88 operating system and may not be of use 
for other Z80 based computers unless thoroughly rewritten. The standard library
source files may be found in the "Z80lib.zip" file.

### 2.3. Z88 module assembler application source

We have supplied the complete source of the Z88 module assembler application. 
This allows you to evaluate many aspects of programming applications on the 
Z88. Further, most features of the assembler are mirrored in these source 
files; using directives, the free format of Z80 mnemonics, library routine 
access, modular file design, labels, using expressions, comments, data 
structure manipulation and good programming design.

### 2.4. File based compilation

This assembler is completely file based, i.e. all parsing and code generation 
is manipulated via files on storage medias such as harddisks or floppy disks 
(or file based RAM-discs).

### 2.5. Modular source file design

A compilation may be split into individual source files that either can be 
linked together during assembly as a single module or assembled as separate 
source file modules. Separate source file modules saves compilation time and 
memory. Further, this design is much more straightforward and much more 
logically clear in a design phase of a large compilation project than one huge 
kludge of a source file.

### 2.6. Scope of symbols in source modules

All source modules may refer to each others symbols by using EXTERN directives.
This means that you refer to external information outside the current source 
module. The opposite of an external module reference is to declare symbols 
globally available using a PUBLIC directive, i.e. making symbols available to 
other source modules. Finally it is possible to have local symbols that are not
known to other source modules than the current. A label or constant that has 
not been declared with EXTERN, PUBLIC or GLOBAL is local to the module.

### 2.7. Using arithmetic and relational expressions

All directives that require a numerical parameter or Z80 mnemonics that use an 
integer argument may use expressions. Expressions may be formed by all standard
arithmetic operators and relational operators. Even binary operators are 
implemented. All expressions may contain external identifiers and is 
automatically resolved during the linking phase. Only certain directives 
require compilation time evaluable expressions.

### 2.8. Source file manipulation

To allow the Assembler to execute in multitasking environments such as LINUX 
and QDOS, all source input files are opened as shared I/O. This allows other 
programs to access the source files while the assembler is working. All output 
files (both text and binary files) are opened/created for exclusive use; other 
programs will have no access to those files until they have been closed.

### 2.9. Free format of assembler source files

The source files may be written in a free format. No fixed position columns as 
needed as in the COBOL programming language. All text may be typed in mixed 
case (the assembler converts all text input to uppercase). Tabulators may be 
used freely (instead of spaces which also saves source file space) to suit the 
programmers own habits of structured text layouts. However, one rule must be 
obeyed: syntax of Z80 assembler mnemonics and most directives must be completed
on individual lines. Text files using different OS dependant line feed 
standard are parsed properly; line feed types CR, LF or CRLF are automatically 
recognized. So you can easily compile your sources from Linux/UNIX on an MSDOS 
platform.

### 2.10. Specification of filenames

Specification of file names in source files are always enclosed in double 
quotes. The assembler just collects the filename string and uses this to open a
file. This way all filename standards may be used as defined on different 
operating system platforms.

### 2.11. Including other source files into the current source file

The need for header file information such as operating system constants or data
structures is often indispensable for source file modules. Instead of copying 
the contents of those files into each module, it is possible to include them at
run time (during parsing). Infinite include file levels are permitted, i.e. 
included files calling other files.

### 2.12. Conditional assembly in source file modules

Large compilation projects often need to compile the application in several 
variations. This can be achieved with enclosing parts of the source with 
conditional directives for the different variations of the application. This 
may also be useful if the assembler source is ported to several platforms, 
where inclusion of other source files (such as header files) are using 
different filename standards. The conditional directives IF, IFDEF, IFNDEF, 
ELSE, and ENDIF may be nested into infinite levels.

### 2.13. Undocumented Z80 instruction code generation

We have included the syntax parsing and code generation of the undocumented Z80
instructions for the sake of completeness. However, IM 2 interrupts must be 
disabled before they are executed (an interrupt may otherwise occur in the 
middle of the instruction execution). Many games on the ZX Spectrum have used 
them to protect the code from prying eyes. The Z88 native debugger code uses 
some of the undocumented instructions for fast access to register variables.

### 2.14. Object file generation

The Z80 Module Assembler generates object files that contains the compressed 
version of an assembled source module. The information herein contains declared
symbols (local, global and external), expressions, address origin, module name
and machine code. The object file modules are much smaller than their source 
file counterparts (often smaller than 2K).

### 2.15. Transfer of object files across platforms

The Z80 Module Assembler is already implemented on several different computer 
platforms. You may freely transfer the object files and use them as a part of 
another cross-compilation. There is no system-dependent information in the 
object files.

### 2.16. Date stamp controlled assembly

To avoid unnecessary compilation of source file modules, it is possible to let 
the assembler compile only recently updated source file modules by comparing 
the date stamp of the source and the object file modules. Source file modules 
that are older than object file modules are ignored. This facility is 
indispensable in large compilation projects.

### 2.17. Listing files

The assembler may generate listing files that contain a copy of the source file
with additional code generation information of Z80 mnemonics dumped in 
hexadecimal format. The listing files are formatted with page headers 
containing time of assembly and the filename. Line numbers are included which 
corresponds to the source file lines.

### 2.18. Symbol information

All symbol generated values used in source modules may be dumped to the end of 
the listing file or as a separate symbol file. If the symbol table is dumped 
into the listing file, each symbol will be written with page references of all 
occurrences in the listing file. Address symbols (labels) are addressed 
relative to the start of the module. Symbol constants are written as defined in
the source. The symbol table is written in alphabetical order with 
corresponding values in hexadecimal format.

### 2.19. Linking and relocation of object modules into executable Z80 machine code

To obtain an executable Z80 machine code file it is necessary to link all 
assembled object modules and relocate them at a defined address, where the code
is to be executed at in the computers' memory. The linking & relocation is 
performed automatically after completed assembly of all specified source file 
modules. The ORG relocation address is specified in the first object module.

### 2.20. Address map files

The address map is invaluable information during a debugging session of your 
compiled program. This file contains all symbolical address labels with their 
generated address constants after a completed linking/relocation of all modules
into executable machine code. The map file is ordered in two groups; the first
list contains all symbol names ordered alphabetically with corresponding 
address constants, the second list contains all symbols ordered by their 
address value (in chronological order).

### 2.21. Symbol address definition files

As with address map files this contains information of globally declared 
symbolical address labels, relocated to their absolute position as for the 
compiled machine code file. However, the format is completely different; all 
symbols are created as constant definitions to be included as a header file 
into another source file and assembled. This is useful if you want to call 
subroutines compiled separately in another project (originated in a different 
memory setup).

### 2.22. Error files

Error files are created by the assembler during processing. If any errors 
should occur, they will be written to stderr and copied to this file containing
information of where the error occurred in the source module. If no errors 
were found, the error file is automatically closed and deleted.

### 2.23. Creating and using object file libraries for standard routines

Machine programmers often re-use their standard routines. We have implemented a
file format for generating libraries using the existing object file modules. 
Using a simple set of rules makes it very easy to create your own libraries 
from your source file modules. Documentation of the library file format is 
included in this documentation. At command line infinite number of libraries 
may be specified. All will be searched during linking of your object modules 
for referenced library routines.

5\. Compiling files
-------------------

### 5.1. The assembler compiling process

The Z80 Module Assembler uses a two stage compilation process; stage 1 parses 
source files and generates object files. Stage 2 reads the object files and 
links the object file code, completes with address patching and finishes with 
storing the executable code.

#### 5.1.1. Stage 1, parsing and code generation of all source files, object file generation

A source file is being parsed for Z80 mnemonics and directives. An object file 
is created to hold information of module name, local, global and external 
symbol identifiers, expressions and the intermediate code generation (but 
address and other constant information). During pass 1 all Z80 mnemonics are 
parsed and code is generated appropriately. All expressions are evaluated; 
expressions that contain relocatable address symbols or external symbol are 
automatically stored into the object file. Expressions that didn't evaluate are
preserved for pass 2. When a source file has been read successfully to the 
end, pass 2 is started. During pass 2 all non-evaluated expressions from pass 1
are re-evaluated and stored to the object file if necessary. Errors are 
reported if symbols are still missing in expressions. When all expressions are 
evaluated and no errors occurred, all "touched" symbols (used in expressions) 
are stored into the object file, with scope, type and value. Then, the module 
name and generated code is stored to the object file. Various file pointers to 
sub-sections of the object file is resolved. The completion of stage 1 is to 
produce the symbol table output (either appended to listing file if selected or
as a separate file).

This process is performed for all specified source modules in a project.

#### 5.1.2. Stage 2, linking object files and library modules, producing executable code

Pass 1 of the linking loads information from each object file in the project; 
the ORG address is fetched, identifiers (resolving scope, and absolute values) 
loaded, and machine code linked. During this pass all external library modules 
are fetched and linked with the object modules (if a library is specified from 
the command line). When all modules have been loaded, pass 2 begins. Pass 2 
then reads each expression section from all object modules (including library 
modules), evaluates them and patches the value into the appropriate position of
the linked machine code. When all expressions have been evaluated successfully
the executable code is stored. If selected, the address map file is produced 
from the current symbol table resided in the data structures of the assembler's
memory is stored to a text file.

### 5.2. File names

Specification of file names follows the convention used on the various 
platforms that the assembler is ported to. Please read your operating systems 
manual for more information.

### 5.3. Portability of assembler file names

If you are going to port your Z80 Module Assembler files across several 
platforms a few hints may be worth considering:

Avoid special symbols in file names like '\_', '#' and '.' . They may have 
special meaning on some operating system platforms. Use only 7-bit standard 
ASCII letters in file names ('A' to 'z'). Non English language letters are not 
always allowed, and further they may not be interpreted correctly when ported 
to another platform. Avoid too long file names. Some operating systems have 
boundaries for length of filenames in a directory path. For example MS-DOS only
allows 8 characters in a file name (followed by an extension). Others may have
no boundaries.

### 5.4. Source file structure

The composition of a source file module is completely free to the programmer. 
How he chooses to place the source code on a text line has no effect of the 
parsing process of the assembler. The linefeed interpretation is also handled 
by z80asm - it understands the following formats:

*   \<LF\> (used by QDOS/SMSQ/UNIX/AMIGA);
*   \<CR\>\<LF\> (used by MSDOS);
*   \<CR\> (used by Z88/MacIntosh).

### 5.5. Using local, global and external symbols

In larger application projects it is unavoidable to use a modular programming 
design, i.e. splitting the source into several individual files. This 
approaches the popular top - down design where you can isolate the problem 
solving into single modules. The outside world just needs to know where the 
routine is to be called by linking the modules with a few directives.

In the Z80 Module Assembler you only need two directives to accomplish just 
that: the XREF and XDEF directives.

XREF declares a symbol to be external to the current source file module. This 
tells the assembler that all expressions using that symbol is not to be 
evaluated until the compiled object modules are to linked and relocated 
together. An expression that contains this symbol is simply stored into the 
object file.

XDEF declares a symbol to be created in this module and made globally available
to other modules during the linking/relocation phase. All expressions that 
contain a globally declared symbol is automatically stored into the object file.

When a symbol is created and is neither declared external or global, it is 
implicitly defined as local to the current source module. The symbol is then 
only available to the current module during linking/relocation.

If you want to access (external) library modules from a library, use the LIB 
directive followed by the name of the routine. Several routine names may be 
specified separated by a comma.

During the linking process all external and corresponding global symbols are 
resolved. If two identical global identifiers are loaded by the linking 
process, the most recently loaded identifier is used by the linker.

### 5.6. Defining symbol names

Good programming involves a structured approach to mnemonic identification of 
names in subroutines, variables, data structures and other constants. The Z80 
Module Assembler gives you several possibilities. The easiest and frequently 
used one is DEFC (Define Constant). We have supplied a complete set of header 
files (the "OZdefc.zip" file) containing the Z88 operating system manifests as 
defined in the Developers' Notes V3 (the "devnotes.zip" file) which just 
contains DEFC directives.

Each DEFC directive is followed by an identifier name, followed by a = symbol 
and then an evaluable constant expression (usually just a constant). Constant 
definitions are usually operating system manifest or other frequently used 
items. They are put into separate source files and later inserted into main 
source files using the INCLUDE directive.

Though DEFC resolves most needs, it may be necessary to define variable areas 
or templates containing names with an appropriate size tag (byte, word or 
double word). This is possible using the DEFVARS directive. Here you may 
specify as many names as needed in the group. Then, it is easy to add, 
rearrange or delete any of the variable names - only a few modifications and 
then just re-compiling the necessary source files that use the templates. This 
would be a nightmare with DEFC, since you have to keep track of the previous 
and next name in the group in addition to count the size of all names. All this
is managed by DEFVARS automatically. Have a look at the syntax in the 
Directive Reference section.

With advanced Z80 programming you cannot avoid dynamic data structures like 
linked lists or binary trees. The fundamentals for this are known as records in
PASCAL or structures in C. DEFVARS is well suited for this purpose. Defining 
each DEFVARS group with 0 automatically generates offset variables. The last 
name then automatically defines the size of the data structure. Again, refer to
the directive reference for a good example.

A third possibility for an easy definition of symbols is to use the DEFGROUP 
directive. Here you're able to define a set of symbols equal to an enumeration.
It follows the same principles as for C's ENUM facility. The default starts at
0 and increases by 1. If you choose, a specific identifier may be set to a 
value, which then can set the next enumeration sequence. Again, this directive 
has been made to implement an easy way of defining symbols and providing a 
simple method to alter the identifier group. Please refer to the directive 
reference for an example.

### 5.7. Comments in source files

As always, good programming requires good documentation. Without comments your 
programs lose overview and logic. Machine code is especially hard to follow - 
have you tried to look at a piece of code 2 years after implementation AND 
without any comments? HORRIBLE! There is never too many comments in machine 
code - we especially like to use high level language as comments - it avoids 
unnecessary text and logic is much more clear.

Comments in Z80 source files are possible using a semicolon. When the assembler
meets a semicolon the rest of the current source line is ignored until the 
linefeed. Parsing will then commence from the beginning of the line. The 
semicolon may be placed anywhere in a source line. As stated you cannot place 
mnemonics after the semicolon - they will be ignored. The Z80 parser will in 
many places accept comments without a semicolon has been set - but don't rely 
on it. Better use a semicolon. The context is much clearer. The following is an
example on how to use comments in Z80 source files:

    ; **********************  
    ; main menu  
    ;  
    .mainmenu   call window   ; display menu  
                call getkey   ; read keyboard  
                ret           ; action in register A

### 5.8. Defining symbolic address labels

The main reason for using an assembler is to be able to determine symbolical 
addresses and use them as reference in the code. These are defined by a name 
preceded with a full stop, or followed by a colon. It is allowed to place a 
mnemonic or directive after an address label. An address label may be left as a
single statement on a line - you may of course use comment after the label. 
The following is a label definition:

    ; *****************  
    ; routine definition  
    .mainmenu call xxx   ; a label is preceded with '.'
    endmain:  ret        ; or followed by ':'

It is not allowed to position two labels on the same line. However, you may 
place as many label after each other - even though no code is between them. 
They just receive the same assembler address.

It is not allowed to specify two identical address labels in the same source 
file.

If you want to declare an address globally accessible to other modules, then 
use PUBLIC for the address label definition, otherwise the label definition 
will be interpreted as a local address label.

    PUBLIC mainmenu  
    ...  
    .mainmenu ; label accessible from other modules through PUBLIC directive

You may use it before or after the label - z80asm automatically handles the 
scope resolution as long as you use PUBLIC to define it as globally accessible.

### 5.9. Writing Z80 mnemonic instructions

All Z80 instructions may be written in mixed case, lower or upper case - you 
decide! How you separate opcode words, register names and operands is your 
choice. Only a few rules must be obeyed:

1.  Each instruction mnemonic must be completed on a single line.
2.  The instruction identifier must be a word, i.e. don't use space between CALL.
3.  Register identifiers must be a word, ie. HL not H L.

A few examples which all are legal syntax:

    Ld HL   , 0       ; comment  
    ld       hl, $fFFF;comment  
    caLL ttttt

### 5.10. Optional Z80 mnemonic instruction syntax

The instructions below allow additional specification of the accumulator 
register. Zilog standard convention is to only specify the source operand. 
Sometimes it is better to specify the accumulator to make the instruction look 
more clear, and to avoid confusion. After all, they specified it for "add", 
"adc" and "sbc".

    sub a,r  
    sub a,n  
    sub a,(hl)  
    sub a,(ix+0)  
    sub a,(iy+0)

this syntax applies also to "and", "or", "xor" & "cp"

### 5.11. The undocumented Z80 instructions

We have included the parsing and code generation of the undocumented Z80 
instructions. They are as follows:

    LD   r,IXL  ; r = A,B,C,D,E,IXL,IXH  
    LD   r,IXH  
    LD   IXL,n  ; n = 8-bit operand  
    LD   IXH,n  
  
    ADC  A,IXL  
    ADC  A,IXH  
    ADD, AND, CP, DEC, INC, OR, SBC, SUB, XOR ...  
  
    SLL  r   ; r = A,B,C,D,E,H,L  
    SLL  (HL)
    SLL  (IX+d)
    SLL  (IY+d)

SLL (Shift Logical Left)

SLL does shift leftwards but insert a '1' in bit 0 instead of a '0'.

Except for the SLL instruction all have bugs related to an interrupt being able
to occur while the instructions are decoded by the processor. They are 
implemented on the chip, but are reported to be unreliable. We have used some 
of them in our debugger software for the Z88. Until now the code has been 
running successfully on all our Z88 computers.

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

7\. Directive reference
-----------------------

The Z80 Module Assembler directives are used to manipulate the Z80 assembler 
mnemonics and to generate data structures, variables and constants. You are 
even permitted to include binary files while code generation is performed.

As the name imply they direct the assembler to perform other tasks than just 
parsing and compiling Z80 instruction mnemonics. All directives are treated as 
mnemonics by the assembler, i.e. it is necessary that they appear as the first 
command identifier on the source line (NOT necessarily the first character). 
Only one directive is allowed at a single source line. Even though they are 
written as CAPITALS in this documentation they may be written in mixed case 
letters in your source files.

Since the directives cover very different topics of assembler processing, each 
directive will be explained in detail, identified with a header description for
each text section. The following syntax is used:

    <> : defines an entity, i.e. a number, character or string.  
    {} : defines a an optional repetition of an entity.  
    [] : defines an option that may be left out.

### 7.1. BINARY "filename"

BINARY loads a binary file at the current location. This could for example be a
static data structure or an executable machine code routine. The loaded binary
file information is included into the object file code section. The assembler 
PC is updated to the end of the loaded binary code.

### 7.2. CALL\_OZ \<expression\>

The \<expression\> may be a 16-bit expression and must evaluate to a constant. 
This is an easy interface call to the Z88 operating system. This is an advanced
RST 20H instruction which automatically allocates space for the size of the 
specified parameter (either 8-bit or 16-bit). Code is internally generated as 
follows:

    RST $20  
    DEFB x ; 8-bit parameter

or

    RST $20  
    DEFW x ; 16-bit parameter

### 7.3. DEFB \<8-bit expr\>,{<8-bit expr>} (-128; 255)

Stores a sequence of bytes (8-bits) at the current location. Expressions may be
used to calculate the values.

### 7.4. DEFW \<16-bit expr\>,{<16-bit expr>} (-32768; 65535)

Stores a sequence of words (16-bits) in low byte - high byte order (little 
endian) at the current location. Expressions may be used to calculate the 
values.

### 7.5. DEFL \<32-bit expr\>,{<32-bit expr>} (-2147483647; 4294967295)

Stores a sequence of double-words (32-bits) in low byte - high byte order 
(little endian) at the current location. Expressions may be used to calculate 
the values.

### 7.6. DEFC name=\<32-bit expression\>{, name=<32-bit expression>}

Define a symbol variable, that may either be a constant or an expression 
evaluated at link time. The allowed range is a signed 32-bit integer value. All
standard Z88 operating system header files use DEFC

### 7.7. DEFM \<string expression\>|\<8-bit expr\>,...

DEFM stores a string constant at the current location. Strings are enclosed in 
double quotes, e.g. "abcdefgh". Strings may be concatenated with byte constants
using commas. This is useful if control characters need to be a part of the 
string and cannot be typed from the keyboard. Several strings and byte 
expressions may be concatenated, e.g.

    DEFM "string_a" , "string_b" , 'X' , CR , LF , 0

### 7.8. DEFGROUP '{' name {',' name \['=' <8-bit expression>\]} '}'

DEFGROUP defines a group of identifier symbols with implicit values. This is 
similar to the enumeration principles used in C and PASCAL. The initial symbol 
value is 0, increased by 1 for each new symbol in the list. You may include a 
\<name = expression\> which breaks the linear enumeration from that constant. 
The DEFGROUP directive may be spanned across several lines and MUST be enclosed
with { and }. DEFGROUP is just a more easy way than: DEFC name0 = 0, name1 = 
name0, ...

The following example illustrates a useful example of defining symbol values:

    DEFGROUP  
    {  
       sym_null  
       sym_ten = 10, sym_eleven, sym_twelve  
    }

### 7.9. DEFINE name,{name}

Defines a symbol identifier as logically true (integer <> 0). The symbol will 
be created as a local variable and disappears when assembly is finished on the 
current source file module.

### 7.10. DEFS \<size\>{, \<fill-byte\>}

DEFS allocates a storage area of the given size with the given fill-byte. The 
fill-byte defaults to zero if not supplied. Both expressions need to be 
constants. The default can be changed with the command line option "-f{value}".

### 7.10a. DEFS \<size\>, "string"

Create a fixed-length string, filling the remaining space, if any, with the 
filler byte (default zero).

e.g.
	DEFS 10, "hello" => "hello", 0,0,0,0,0


### 7.11. DEFVARS \<16-bit expression\> '{' \[<name>\] \[\<storage\_size\> \<size\_multiplier\>\] '}'

DEFVARS defines variable address area or offsets. First you define the origin 
of a variable area. This may be defined using an evaluable 16-bit positive 
expression. Each variable name is followed by a size specifier which can be  
'ds.b' (byte), 'ds.w' (word), 'ds.p' (3-byte pointer) or 'ds.l' (double-word). 
This is particularly useful for defining dynamic data structures in linked 
lists and binary search trees. Defining variable areas are only template 
definitions not allocations. An example:

    DEFVARS Z80asm_vars  
    {  
       RuntimeFlags1 ds.b 1     ; reserve 1 byte  
       RuntimeFlags2 ds.b 1  
       RuntimeFlags3 ds.b 1  
                     ds.w 1     ; space not named  
       explicitORIG  ds.w 1     ; reserve 2 bytes  
       asmtime       ds.b 3     ; reserve 3 bytes  
       datestamp_src ds.b 6     ; reserve 6 bytes  
       datestamp_obj ds.b 6  
       TOTALERRORS   ds.l 1     ; reserve 4 bytes  
    }

the following is useful for defining dynamic data structures:

    DEFVARS 0                    ; 'PfixStack' structure  
    {  
       pfixstack_const     ds.l 1    ; stack item value  
       pfixstack_previtem  ds.p 1    ; pointer to previous  
       SIZEOF_pfixstack              ; size of structure  
    }

This type of variable declaration makes it very easy for modifications, e.g. 
deleting or inserting variable definitions.

A special logic is available too which can be used throughout individual source
files during compilation. If you specify -1 as the starting address, the last 
offset from the previous DEFVARS which was not specified as 0 will be used.

This enables you to gradually build up a list of identifier name offsets across
DEFVARS areas in different source files. The following example explains 
everything:

    defvars $4000  
    {  
       aaa ds.b 1  
       bbb ds.b 100  
    }  
    defvars -1  
    {  
       ccc ds.b 100  
       ddd ds.b 1  
       eee ds.b 10  
    }  
    defvars 0  
    {  
       fff ds.p 1  
       ggg ds.b 1  
       hhh ds.w 1  
       iii ds.p 1  
    }  
    defvars -1  
    {  
       jjj ds.b 100

    }

Some of the symbols will look like this:

    BBB = $4001  
    CCC = $4065  
    GGG = $0003  
    JJJ = $40D4

### 7.12. EXTERN name {, name}

This declares symbols as external to the current module. Such a symbol must 
have been defined as PUBLIC in another module for the current module to be able
to use the symbol (it will be linked during the linking phase).

### 7.12. FPP \<8-bit expression\>

Interface call to the Z88 operating systems' floating point library. This is 
easier than writing:

    RST $18  
    DEFB mnemonic

This is an advanced RST 18H instruction which automatically allocates space for
the specified parameter. All Z88 floating point call mnemonics are defined in 
the "fpp.def" file.

### 7.14. GLOBAL name {, name}

The GLOBAL directive defines a symbol PUBLIC if it has been defined locally or 
EXTERN otherwise.

### 7.13. IF \<logical expression\> ... \[ELSE\] ... ENDIF

This structure evaluates the logical expression as a constant, and compiles the
lines up to the ELSE clause if the expression is true (i.e. not zero), or the 
lines from ELSE to ENDIF if is is false (i.e. zero). The ELSE clause is 
optional. This structure may be nested.

### 7.14. IFDEF \<name\> ... \[ELSE\] ... ENDIF

This structure checks if the give symbol name is defined, and compiles the 
lines up to the ELSE clause if true (i.e. defined), or the lines from ELSE to 
ENDIF if false (i.e. not defined). The ELSE clause is optional. This structure 
may be nested.

### 7.15. IFNDEF \<name\> ... \[ELSE\] ... ENDIF

This structure checks if the give symbol name is not defined, and compiles the 
lines up to the ELSE clause if true (i.e. not defined), or the lines from ELSE 
to ENDIF if false (i.e. defined). The ELSE clause is optional. This structure 
may be nested.

### 7.16. INCLUDE "filename"

Another component that is frequently used is to 'link' an additional source 
file together with the current source file. Usually this contains variable 
definitions that are commonly used by several modules in a project. This makes 
sense since there is no idea in copying the same information into several files
- it simply uses redundant space of your storage media. This is certainly 
important on the Z88 which not always has huge amounts of installed user/system
RAM (usually 128K). The external source file will be included at the position 
of the INCLUDE directive.

The format of the filename depends on the operating system platform. As with 
the current source file, you may also insert files in include files. There is 
no limit of how many levels (of files) you specify of include files. Recursive 
or mutually recursive INCLUDE files (an INCLUDE file calling itself) is not 
possible - the assembler program will immediately return an error message back 
to you!

Include files are usually put at the start of the source file module but may be
placed anywhere in the source text. The current source file will be continued 
after the INCLUDE directive when the included file has been parsed to the end 
of file.

### 7.17. INVOKE \<16-bit expression\>

Special CALL instruction for the Ti83 calculator; it is coded as a RST 28H 
followed by the 16-bit expression, if the -plus option is passed on the command
line (for the Ti83Plus), or as a normal CALL instruction if the option is not 
passed.

### 7.18. LIB name {,name}

This directive is obsolete. It has been replaced by the EXTERN directive (See 
changelog.txt at the root of the z88dk project).

### 7.19. LINE \<32-bit expr\>

Used when the assembler is used as the back-end of a compiler to synchronize 
the line numbers in error messages to the lines from the compiled source. Needs
the -C option to enable.

### 7.20. LSTOFF

Switches listing output to file off temporarily. The listing file is not closed.

### 7.21. LSTON

Enables listing output (usually from a previous LSTOFF). Both directives may be
useful when information from INCLUDE files is redundant in the listing file, 
e.g. operating system definitions.

### 7.22. MODULE name

This defines the name of the current module. This may be defined only once for 
a module. All source file modules contain a module name. This name is used by 
the assembler when creating address map files and for searching routines in 
libraries. Further, it allows the programmer to choose a well-defined name for 
the source file module. The position of the module name is of no importance; it
may be placed at the end or the start of the source file. However, it has more
sense to put it at the top. The syntax is simple - specify a legal identifier 
name after the MODULE directive, e.g. MODULE main\_module

### 7.23. ORG \<16-bit expression\>

Define address origin of compiled machine code - the position in memory where 
the machine is to be loaded and executed. The expression must be evaluable 
(containing no forward or external references). All address references will be 
calculated from the defined ORG value. The ORG address will be placed in the 
current module that is being compiled. However, during linking only the first 
object module is being read for an ORG address. The ORG is ignored during 
linking if you have specified an -r option on the command line.

When assembling programs with multiple sections, a section without an ORG will 
be appended to the end of the previous section. A section with a defined ORG 
will generate its own binary file, e.g. file\_CODE.asm.

A section may contain ORG -1 to tell the linker to split the binary file of 
this section, but continue the addresses sequence from the previous section.

### 7.26. PUBLIC name {, name}

This directive declares symbols publicly available for other modules during the
linking phase of the compilation process.

### 7.24. XDEF name {, name}

This directive is obsolete. It has been replaced by the PUBLIC directive (See 
changelog.txt at the root of the z88dk project).

### 7.25. XLIB name

This directive is obsolete. It has been replaced by the PUBLIC directive (See 
changelog.txt at the root of the z88dk project).

### 7.26. XREF name {, name}

This directive is obsolete. It has been replaced by the EXTERN directive (See 
changelog.txt at the root of the z88dk project). 
