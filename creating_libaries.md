# Creating Libraries

z88dk will link a C/asm program against libraries, only pulling out functions that are actually used by the program which is exactly what you want, especially as your library grows! It is a very good idea to write all your functions in asm. Library code should be as optimal as possible so that we can get at that "90% of execution time in asm and 10% in C" thing to maximize performance). C programs will be able to call these asm subroutines no problem as long as the header file tells the compiler how to do it and the asm file collects the C parameters in the way specified in the header file.

### Rules for creating libraries

 1.  One function per file.  When a library function is referenced, the entire file gets pulled into the compiled program.
 2.  Use the 'PUBLIC' directive with the function entry point. 'PUBLIC name' means the function will be known as 'name' and if the program makes a call to "name", it pulls in this file.
 3.  Use the directive 'EXTERN name1, name2, ...' to pull in other library code, whether from your library or someone else's.  Eg, if your function uses memcpy() you can add "EXTERN memcpy" near the top of the file and call it with "call memcpy".  The linker will automatically pull in the memcpy code (if it hasn't already) when your function gets used in the user program.

### PUBLIC and EXTERN


*  PUBLIC and EXTERN are about controlling scope across user files, not libraries.  If the user program is spread across two files, 'PUBLIC name' will cause the assembler label 'name' to become global and therefore visible in the other file if an "EXTERN name' is added in the second file.

For example, the beginning of [this](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/sprites/sp1_IterateSprChar_callee.asm?view=markup) asm function looks like:

	
	    5 PUBLIC sp1_IterateSprChar_callee
	    6 PUBLIC ASMDISP_SP1_ITERATESPRCHAR_CALLEE
	    7 
	    8 EXTERN l_jpix


The library function name and entry point is PUBLIC.
An asm constant I want made globally visible is PUBLIC when this library function is used.
The library function "l_jpix" is referred to from within this function; the call to it causes its code to be included as well.
Another asm function in another file somewhere else whill use "EXTERN ASMDISP_SP1_ITERATESPRCHAR_CALLEE" (of course after including the function by specifying "EXTERN sp1_IterateSprChar_callee") to access the constant published by the XDEF directive.

### How to compile a library

To compile a library you can use this command:

	
	z80asm -d -ns -nm -Mo -xlibname @file.lst


where 'file.lst' contains a list of asm functions to include in the library.

Example code snippet from "[/z88dk/libsrc/sprites/software/sp1/spectrum.lst](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum.lst?view=markup)"

	
	spectrum/sprites/_sp1_struct_cs_prototype
	spectrum/sprites/_sp1_struct_ss_prototype
	spectrum/sprites/sp1_AddColSpr
	spectrum/sprites/sp1_AddColSpr_callee
	...


This would add the asm functions "_sp1_struct_cs_prototype.asm", etc to the library created "libname.lib".


To compile a C program using this library you'd add "-llibname" to the compile command.  You may have to add some path info if the lib is not located in the place z88dk expects to find it (z88dk/lib/clibs).


The header file is for the C compiler and indicates what functions are in the library and what linkage to use when calling them (linkage is how the parameters are passed to the function).

### Header files

A header file would look something like:

	:::c
	#ifndef _MYHEADER
	#define _MYHEADER
	
	extern void __LIB__ myfunc(int a, int b);
	extern void __LIB__ init(void);
	
	#endif



The %%__LIB__%% qaulifier will cause the C compiler to translate these header functions into sequences of "LIB myfunc", "LIB init", etc so that the linker will know those references are for library functions.  The C compiler will generate code like "call myfunc" and "call init" to call these functions.

### Linkage types

There are 3 linkage types:

*  **The standard C linkage** - This is where the C compiler collects parameters for the function call, pushes them on the stack, makes the call and then pops the parameters to restore the stack.  Inside the called function your library code is popping the parameter values and pushing them back so that the caller can pop them off again to restore the stack. Or The called function could read the parameters from the stack without popping the off.

*  **The %%__FASTCALL__%% linkage** - %%__FASTCALL__%% only works for functions accepting a single parameter.  Rather than pushing it on the stack, the parameter is passed in HL.

*  **The %%__CALLEE__%% linkage** - %%__CALLEE__%% tells the compiler that your library function will fix the stack.  This means, to call your lib functions, the C compiler will push parameters on the stack and make the call.  Your lib function will pop the parameters off the stack and push back only the return address. Insodoing fix the stack so that the caller doesn't have to.  Amazingly this simple change can save several 100 bytes in a medium sized program!

To use these specific linkage types, in the header file you need to add  %%__FASTCALL__%% or %%__CALLEE__%% (or nothing for the standard linkage) next to the %%__LIB__%% in the function prototypes.

eg:
`extern void __LIB__ __CALLEE__ myfunc(int a, int b);`{c}

### Combineing different linkage types

One more complication regarding %%__CALLEE__%% functions: the C compiler can only call functions through function pointers using standard C linkage. There is a nice way to allow the function to be called both normally (with %%__CALLEE__%%), and as a function pointer (standard). In [this](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/spectrum-sp1.h?revision=1.1&view=markup) header file you will spot this:

	:::c
	  218 extern void               __LIB__   sp1_IterateSprChar(struct sp1_ss *s, void *hook1);
	  ...
	  242 extern void          __CALLEE__ __LIB__  sp1_IterateSprChar_callee(struct sp1_ss *s, void *hook1);
	  ...
	  257 #define sp1_IterateSprChar(a,b)        sp1_IterateSprChar_callee(a,b)


The C program will call the library function using this much nicer name as in "sp1_IterateSprChar(s,f);"  The pre-processor will change this to a call to "sp1_IterateSprChar_callee(s,f);" so that the %%__CALLEE__%% linkage function gets called by default. However, if the program tries to call the function using a function pointer:

	:::c
	function_pointer = sp1_IterateSprChar;
	(function_pointer)(s,f);


The preprocessor does not make the callee substitution because the number of parameters doesn't match.  In this case the library function "sp1_IterateSprChar" is called using normal C linkage.  You can see that this function is only a shell that calls into the real callee function: [sp1_IterateSprChar.asm](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/sprites/sp1_IterateSprChar.asm?view=markup)

For this to work the [sp1_IterateSprChar_callee](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/sprites/sp1_IterateSprChar_callee.asm?view=markup) function needs to pass the address of the code after popping off the parameters, or the displacement between this and the entry point. It makes this public by useing the XDEF directive. This also gives asm programmers the option of calling your library functions setting up parameters in registers rather than in the C-way.  This is well worth doing!

### Examples

Makefile:  [/z88dk/libsrc/sprites/software/sp1/Makefile](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/Makefile?view=markup)

The Makefile variables are defined in: [/z88dk/libsrc/Make.config](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/Make.config?view=markup)

A lst file listing all the asm files to include in the library when "make sp1-spectrum" is run:

[/z88dk/libsrc/sprites/software/sp1/spectrum.lst](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum.lst?view=markup)

The corresponding header file for inclusion in C programs showing use of %%__FASTCALL__%% and %%__CALLEE__%%:

[/z88dk/libsrc/sprites/software/sp1/spectrum/spectrum-sp1.h](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/spectrum-sp1.h?view=markup)

One of the asm implementations "sp1_IterateSprChar_callee" whose function prototype can be found in the header above:

%%extern void __CALLEE__ __LIB__  sp1_IterateSprChar_callee(struct sp1_ss *s, void *hook1);%%

[/z88dk/libsrc/sprites/software/sp1/spectrum/sprites/sp1_IterateSprChar_callee.asm](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/sprites/sp1_IterateSprChar_callee.asm?view=markup)


This page is based on a forum post by Alvin (http://www.z88dk.org/forum/viewtopic.php?id=2637)
