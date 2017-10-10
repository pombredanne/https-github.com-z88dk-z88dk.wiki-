## Getting Started

* [Installation](installation)
* [Licence](license)

## Platforms


## Classic Library

* [Overview](Classic-Overview)
* [Platform List](Platform)
* [Memory Allocation](Classic-allocation)
* [Retargetting](Classic--Retargetting)
* [Building the libraries](libsrc)

## New Library

* [Introduction](front)

## Misc

* [Debugging](debugging)
* [Far Memory](farmem)
* [Deficiencies](deficient)
* [Importing routines written in 8080 assembly mnemonics](8080toz80)
* [Using CP/M libraries in REL format with z88dk](relformat)
  * [Linking external libraries](libraries)
  * [Linking Basic, Fortran, Pascal Programs with z88dk](programs)

## Mixing C and Z80 Assembler

* [Inline Assembler](inlineasm)
* [The Stack frame (Parameter passing)](stackframe)
* [Pragma Directives](pragmas)
* [Sharing Code by Creating Libraries](creating_libraries)

## Tools

* [The Compiler Frontend (zcc)](Tool-zcc)
* [The Assembler / Linker (z80asm)](Tool-z80asm)
* [copt](Tool--copt)
* [Various tools](tools)




### The VT/ANSI console driver

The ANSI driver provides a VT100 emulation, accepting escape codes that are compatible across the platforms that support the ANSI driver and of course unix terminals.
The conio.h variant provided by z88dk is tightly interconnected to this driver and provides useful extra features like cursor positioning and detection of the current txt resolution.

#### How to change the font size in the VT/ANSI emulation library

The selection of columns is now a link time option, for example with ansi test you can compile as follows:

    zcc +msx ansitest.c -create-app -lndos -pragma-need=ansiterminal -pragma-define:ansicolumns=64

Valid columns are:

     24, 28, 32, 36, 40, 42, 51, 64, 80, 85, 128, ROM24, ROM28, ROM32 and ROM36.

#### How to change the font that is used

You can switch to the ROM font by specifying the following options to the compile line:

    -pragma-define:ansifont=7359 -pragma-define:ansifont_is_packed=0 -pragma-define:ansicolumns=32
