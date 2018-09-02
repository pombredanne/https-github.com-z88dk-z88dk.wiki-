
The TI calculators port has been thought to get a common compatibility layer.
The VT/ANSI library features the same text resolution for all the platforms, permitting the applications to be easily ported.


{{platform:ticalc.jpg|}}


# Quick start

zcc +ti82 -lm -o adv_a -create-app adv_a.c

zcc +ti83 -lm -o adv_a -create-app adv_a.c

zcc +ti8x -lm -o adv_a -create-app adv_a.c

zcc +ti85 -lm -o adv_a -create-app adv_a.c

zcc +ti86 -lm -o adv_a -create-app adv_a.c


or, for special text support...


zcc +ti82 -lndos -O3 -create-app -pragma-need=ansiterminal  -pragma-define:ansicolumns=24 -o adv_a adv_a.c


..being 'ansicolums' an optional parameter (normally set to 32) which for the TI82..TI84 models can be 12, 13, 16, 19, 24 or 32.

On the TI85..TI86 models you can choose between 16, 18, 21, 25, 32 and 42.


# Supported Calculators

Every calculator has specific hardware and software requirement.

Some calculator is able to run machine code directly, some is not; however we suggest to use always a shell for your calculator.
It is a sort of "operating system" making your calculator being capable to run machine code programs.

You can download a suitable shell at ["ticalc.org"](http://www.ticalc.org).



## TI82

To compile programs for this platform you need to specify the "+ti82" parameter.
The only supported shell is "**CrASH**".



## TI83

To compile programs for this platform you need to specify the "+ti83" parameter.
The default supported shell is "**ION**", but you can change this behaviour by passing "-startup=X" parameter, where 'X' can be:

   1 - Ion; Ti-Explorer (default)
   2 - Venus;
   3 - ZES;
   4 - Anova; SOS
   5 - Ti-Explorer, AShell; SOS, Anova (same as 6)
   6 - AShell, Ti-Explorer; SOS, Anova (same as 5)
   7 - SOS; Anova
   8 - Venus Explorer; Venus
   9 - Ion, Ti-Explorer; ZASMLOAD, plain TIOS
    10 - Plain TIOS, ZASMLOAD

Types can be specified also with the "-subtype=" command line option.

Valid values are (referred to the table above):
	1 - venus
	2 - zes
	3 - anova
	4 - tiexp
	5 - ashell
	6 - sos
	7 - venusexp
	8 - ion
	9 - asm

### External resources

Assembly programming notes on [WikiTI](http://wikiti.denglend.net/index.php?title=Calculator_Documentation)




## TI83 Plus

To compile programs for this platform you need to specify the "+ti8x" parameter.
The default supported shell is "**ION**", but you can change this behaviour by passing "-startup=X" parameter, where 'X' can be:

   1  - Ion (default)
   2  - MirageOS without quit key
   3  - (reserved)
   4  - TSE Kernel
   10 - asm( executable

Types can be specified also with the "-subtype=" command line option.

Valid values are (referred to the table above):
	mirage
	tse
	asm

### External resources

Assembly programming notes on [WikiTI](http://wikiti.denglend.net/index.php?title=Calculator_Documentation)



## TI85

To compile programs for this platform you need to specify the "+ti85" parameter.
The default supported shell is "**Rigel**", but you can change this behaviour by passing "-startup=X" parameter, where 'X' can be:

   1  - Rigel (default)
   3  - PhatOS (unstable)

Note that some TI85 shell is able to run some program in TI83 format, but it won't take benefit of the wider screen.


## TI86

To compile programs for this platform you need to specify the "+ti86" parameter.

The default supported shell is "ION", but you can change this behaviour by passing "-startup=X" parameter, where 'X' can be:

   1 - LASM (default)
   2 - ASE, Rascal, emanon, etc.
   3 - zap2000
   4 - emanon
   5 - Embedded LargeLd - !!!EXPERIMENTAL!!!
    10 - asm() executable


Types can be specified also with the "-subtype=" command line option.

Valid values are (referred to the table above):
	ase
	zap2000
	emanon
	largeld
	asm


# #pragma parameters

Depending on the Shell being used it might be possible to export further "special" informations, such as an icon or an application title.
This can be done placing #pragma directives directly in the C source program.




## Application name

To make the shell display a specific name:

    #pragma string name `<application name>`

This will work on:

*  Anova

*  AShell

*  ASE

*  CrASH

*  Emanon

*  Ion

*  MirageOS

*  PhatOS

*  Rascal

*  Rigel

*  SOS

*  Ti-Explorer

*  TSE Kernel

*  Venus

*  Venus Explorer

*  Zap2000





## Icon

Some shell displays an Icon associated to the application, normally near its name; when required Z88DK shows a default picture, a small "C+" logo, but it can be replaced with a custom one.

Icons might be handled in slightly different ways depending on the Shell.
Some of them require 8x8 icons, but sometimes the resolution is different.

To make the shell display the 8x8 smiley icon:

    #pragma data icon 0x3C, 0x42, 0xA5, 0xA5, 0x81, 0xA5, 0x5A, 0x3C ;


This will work on: 


*  AShell (8x8)

*  Anova (8x5)

*  Emanon (7x7)

*  SOS (8x8)

*  MirageOS (15x15 - see below)

*  TI-Explorer (8x8)

*  Venus (7x7)

*  Venus Explorer (7x7)

*  Zap2000 (8x8)



On TI83 Plus, an icon for MirageOS can be defined as follows:

    #pragma data mirage_icon `<30 bytes: data for 15x15 pixels>` ;

## Links

[Video:Programando en C para la TI-82 STATS (TI-83)](http://www.youtube.com/watch?v=KkY55kO59BM)

[Compiler un programme en C pour ti83](http://www.yaronet.com/posts.php?s=138500)

[Ti84 plus - compile and run an hello world c program](http://ti-84-plus.com/blog/ti-84-plus-compile-and-run-an-hello-world-c-program/)

