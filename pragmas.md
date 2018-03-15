
### Introduction

Pragma directives allow a source file to issue directives to the compiler (and the rest of the toolchain) which affect the binary that is generated and how it can run. The results of some of the pragma options are written to the zcc_opt.def file which is used during the linking stage and the values contained within are available to the platform's startup code.

### Available Pragma Commands

*  #pragma output name[=value] Defines [word] in zcc_opt.def
*  #pragma data name byte_values
*  #pragma byte name byte_value

Changing the output sections:

* #pragma bssseg name
* #pragma codeseg name
* #pragma constseg name
* #pragma dataseg name
* #pragma initseg name


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



### Configuring printf and scanf converters

Both newlib and classic link in only the required converters for printf and scanf. If you compile using sccz80, then it will endeavour to configure the converters automatically, however if you use zsdcc then you will need to configure the list of converters that are required.

    #pragma printf = "<list printf converters here including l or ll for long and longlong>"
    #pragma scanf  = "<list scanf converters here including l or ll for long and longlong>"

Example:

    #pragma printf = "%s %c %u %f"     // enables %s, %c, %u, %f only 
    #pragma scanf  = "%s %lx %lld %["  // enables %s, %lx, %lld, %[ only

The converter string content is flexible. You can insert spaces, do away with quotes, include % signs and so on. ”%s%c”, “s c”, ”%0d %llx” are all valid. Digits and flag specifiers (+, -, *) have no effect on the new c library but the classic c library will distinguish between ”%0d” and ”%d” with the latter meaning use the simple %d without any formatting.

### Pre configured parameters
 
| `#pragma output nostreams` | No stdio disc files | 
| ---------------------------- | ------------------- | 
| `#pragma output nofileio` | No fileio at all    | 

####  Other Target dependent parameters 

*  [Cambridge Z88](platform/z88_applications#compiler_directives)

*  [CP/M](platform/cpm#Optimization)

*  [Texas Instruments Calculators](platform/ticalc#pragma_parameters)

*  [ZX 81 High Resolution Graphics library](library/zx81#base_graphics_global_variable)

### Related arguments


*  [The ZCC command](/zcc)
