# INTRINSIC.H

 | Include    | #include `<intrinsic.h>`                                                                                              |                                                     
 | ----------------------------------------------------------------------------------------------------------------------------------                                                     
 | Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/intrinsic.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sccz80/intrinsic.h?content-type=text%2Fplain) |
 | | [{z88dk}/include/_DEVELOPMENT/sdcc/intrinsic.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sdcc/intrinsic.h?content-type=text%2Fplain) |               

Inlined assembly interferes with sdcc's peephole optimizer.  The peepholer is not normally allowed to look into an inlined asm block so it must assume all registers leading to the block are live.  This prevents optimizations from occurring close to inlined asm blocks.

Because of this, intrinsics were introduced.  Intrinsics are a means to inline z80 instructions without impacting the peepholer.  Commonly inlined asm instructions have intrinsic equivalents but intrinsics are also used by the library to inline code that performs actions not normally guaranteed by the compilers.  For example, intrinsics are present to perform atomic 16-bit loads and stores.

Intrinsics appear like standard C function calls in source code but they are replaced by inlined asm instructions by the z88dk back end.

sccz80 does not suffer the same peepholer issues as sdcc but these functions are also available to sccz80 so that source code is portable between the two compilers.

# INTRINSICS - Z80 INSTRUCTIONS

## void intrinsic_di(void)

## void intrinsic_ei(void)

## void intrinsic_ex_de_hl(void)

## void intrinsic_exx(void)

## void intrinsic_halt(void)

## void intrinsic_im_0(void)

## void intrinsic_im_1(void)

## void intrinsic_im_2(void)

## void intrinsic_nop(void)

## void intrinsic_reti(void)

## void intrinsic_retn(void)


# INTRINSICS - ATOMICS

## unsigned int intrinsic_load16(address)

A macro that inserts an atomic 16-bit load from the address "address".  Address can be a numerical constant or an assembly language label referring to a global variable.  SDCC will generate warnings #84 and #112 both of which can be ignored when connected to use of this macro.

## unsigned int intrinsic_store16(address,value)

A macro that inserts an atomic 16-bit store of value "value" to address "address" with result equal to "value".  Both "address" and "value" can be a numerical constant or an assembly language label.  SDCC will generate warnings #84 and #112 both of which can be ignored when connected to use of this macro.


# INTRINSICS - ENDIAN

## unsigned int intrinsic_swap_endian_16(unsigned int n)

Swap the MSB and LSB of a 16-bit quantity.

## unsigned long intrinsic_swap_endian_32(unsigned long n)

Reverse the byte order of a 32-bit quantity.

## unsigned long intrinsic_swap_word_32(unsigned long n)

Swap the upper and lower words of a 32-bit quantity.  Note that this results in an inlined "ex de,hl" instruction so it can be used as an **intrinsic_ex_de_hl()** that takes an argument.


# INTRINSICS - FUNCTION RETURN VALUE

Assembly language subroutines sometimes return useful values in registers other than HL which is used as the return value by the C compiler.  These functions change the return value to another register pair so that the C program can have access to these alternate return values.

These intrinsics must be run on the rightmost side of a comma operator in order to prevent sdcc from inserting housekeeping code between the asm function and the intrinsic.  An example will appear below.

## void *intrinsic_return_bc(void)

Change the return value of a function to what is held in the BC register.

## void *intrinsic_return_de(void)

Change the return value of a function to what is held in the DE register.

	
	#include `<intrinsic.h>`
	#include `<string.h>`
	
	void *strcatz(char *dst, char *src)
	{
	   return (strcat(dst, src), intrinsic_return_de());
	}


strcat() normally returns pointer "dst" as its return value.  An investigation of the implementation of strcat() will reveal that the DE register will hold a pointer to the terminating '\0' in "dst" after the copy.  The intrinsic is used to change the return value to this pointer.  A function is used to wrap this behaviour and is aptly called strcatz() to indicate it functions like strcat but returns a pointer to the terminating 0 byte.  If sdcc is used as compiler, this function can be made inline.

Intrinsics are non-standard so if the goal is to write portable code, something like this could be hidden behind a macro.  If the code is to run on a compiler other than z88dk, an equivalent implementation can be defined "#define strcatz(d,s) (d+strlen(strcat(d,s)))".

Note that sdcc inlines a handful of string functions so if you are writing assuming a library version of one of those functions is run, be sure to invoke the library function's callee or fastcall entry point directly.  Eg, calling "strcpy_callee()" instead of "strcpy()" will select the library function rather than the inlined code that sdcc will produce for strcpy().

# INTRINSICS - LABELS

## void intrinsic_label(label_name)

Inlines an assembly label "label_name" at a point in C code that can be looked up in the map file to find its location in memory.  The label name should be chosen with care so that it doesn't collide with other library, compiler, or user code labels.


# INTRINSICS - DUMMY FUNCTION

## void intrinsic_stub(void)

To the compiler this function appears like any other function call, however the backend removes the call from the compiler output before the peephole step.

Mainly it is used to circumvent an sdcc bug concerning programs with 64-bit integers.  See [sdcc known issues](temp/front#sdcc2).

