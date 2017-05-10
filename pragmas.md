 ====== Pragma Directives ======

### Introduction

Pragma directives allow a source file to issue directives to the compiler (and the rest of the toolchain) which affect the binary that is generated and how it can run. The results of some of the pragma options are written to the zcc_opt.def file which is used during the linking stage and the values contained within are available to the platform's startup code.

### Available Pragma Commands


*  #pragma [sccz80 option] Turns on an option with z88dk

*  #pragma asm Start an assembler block (equivalent of #asm)

*  #pragma endasm End an assembler block (equivalent of #endasm)

*  #pragma output name[=value] Defines [word] in zcc_opt.def

*  #pragma data name byte_values

*  #pragma byte name byte_value

### #pragma output

The following pragma:

	
	#pragma output JustAName
	#pragma output WithaValue=2


Generates the following output in zcc_opt.def

	
	IF !DEFINED_JustAName
	        defc    DEFINED_JustAName = 1
	ENDIF
	
	
	IF !DEFINED_WithaValue
	        defc    DEFINED_WithaValue = 1
	        defc WithaValue = 2
	ENDIF








### Pre configured parameters

 | **#pragma output nostreams** | No stdio disc files | 
 | ---------------------------- | ------------------- | 
 | **#pragma output nofileio**  | No fileio at all    | 

####  Other Target dependent parameters 

*  [Cambridge Z88](platform/z88_applications#compiler_directives)

*  [CP/M](platform/cpm#Optimization)

*  [Texas Instruments Calculators](platform/ticalc#pragma_parameters)

*  [ZX 81 High Resolution Graphics library](library/zx81#base_graphics_global_variable)

### Related arguments


*  [The ZCC command](/zcc)

