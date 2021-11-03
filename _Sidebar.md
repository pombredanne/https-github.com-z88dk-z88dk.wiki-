## Getting Started

* [Home](https://github.com/z88dk/z88dk/wiki)
* [Installation](installation)
* [Docker Usage](Docker-Usage)
* [Snap usage](Snap-usage)
* [Licence](license)

## Classic Library

* [Overview](Classic-Overview)
  * [Screen output](Classic-GenericConsole)
  * [Graphics support](Classic-Monochrome-Graphics)
  * [Audio support](Classic-Audio)
  * [Memory Allocation](Classic-allocation)
  * [Maths Libraries](Classic--Maths-Libraries)
* [Platform List](Platform)
* [Unsupported Platforms](Platform---Unsupported)
* [i8080/5 Support](Classic-8080)
* [Retargetting](Classic--Retargetting)
* [Building the libraries](Classic-Building-the-libraries)
* [Pragmas](Classic--Pragmas)

## New Library

* [Introduction](https://github.com/z88dk/z88dk/wiki/Introduction)
* [Library Configuration](Configuration)
  * [clib_cfg.asm](Configuration#clib_cfgasm)
  * [clib_target_cfg.asm](Configuration#clib_target_cfgasm)
  * [Rebuilding the Library](Configuration#rebuilding-the-library)
* [CRT](CRT)
  * [crt configuration](CRT)
  * [pragma overrides](CRT)
  * [memory map](CRT)
  * [user initialization and exit code](CRT)
  * [User-Supplied Crt](CRT)
* [Header Files](Header-Files)
* [Assembly Language](Assembly-Language)
* [Library in Depth](Library-in-Depth)
* [Embedded Platform](NewLib--Platform--Embedded)

## Misc

* [Benchmarks](Benchmarks)
* [Datatypes](Datatypes)
* [Debugging](debugging)
* [Decompression](Decompression)
* [More than 64k](More-Than-64k)
* [Deficiencies](Suite-deficiencies)
* [Compiling Larger Applications](Compiling-Larger-Applications)
* [Importing routines written in 8080 assembly mnemonics](8080toz80)
* [Using CP/M libraries in REL format with z88dk](relformat)
  * [Linking external libraries](libraries)
  * [Linking Basic, Fortran, Pascal Programs with z88dk](programs)
* [Writing optimal code](WritingOptimalCode)
* [Speeding up Compilation](CompilationSpeed)

## Mixing C and Z80 Assembler

* [Calling Conventions](CallingConventions)
* [The Stack frame (Parameter passing)](The-Stack-Frame)
* [Sharing Code by Creating Libraries](creating_libraries)
* [Inline Assembler](InlineAssembler)

## Tools

* [The Compiler Frontend (*zcc*)](Tool---zcc)
* [The Assembler / Linker (*z80asm*)](Tool---z80asm)
* [Object and Library file Dumper (*z80nm*)](Tool---z80nm)
* [Object and Library file Manipulator (*zobjcopy*)](Tool---zobjcopy)
* [*copt*](Tool---copt)
* [*ticks* emulator](Tool---ticks)
* [z88dk-gdb debugger](Tool-z88dk-gdb)
* [Disassembler](Tool-z88dk-dis)
* [Various tools](tools)