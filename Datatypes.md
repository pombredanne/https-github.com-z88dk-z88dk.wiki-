# Data Types

The most common surprise to C programmers used to programming 32- and 64-bit machines is the reduced bit width of many types.  The z80 processor is only 8-bit with some 16-bit characteristics so the bit widths of the built in data types are correspondingly reduced for best performance.  Always keep this in mind while writing programs targetting small processors.

|          |  char  |  int  |  long  |  long long  |  _Float16 | float<sup>**(2,4)**</sup>  |  double<sup>**(2,4)**</sup>  | void *  |
|--|--|--|--|--|--|--|--|--|
| SCCZ80   |  8     |  16   |  32    |  n/a        |  16       | 32/48/64                   |  48                          |  16<sup>**(5)**</sup>  |
| SDCC     |  8     |  16   |  32    |  64         |  n/a      | 32<sup>**(3)**</sup>       |  32<sup>**(3)**</sup>        |  16  |


<sup>**2**</sup> sccz80 does not distinguish between float and double and sdcc only supports the float type, however the library aliases float and double for sdcc so that source code is compatible between the two compilers.  In user code, use of types "float_t" and "double_t" as defined in "math.h" and "stdlib.h" will eliminate compiler warnings and allow seemless compatibility of source between both compilers.

<sup>**3**</sup> The floating point libraries supplied by z88dk operate on 48-bit floats which are converted at the compiler / library interface.

<sup>**4**</sup> Targets may supply a native float library with variable characteristics.  If a native library is selected on the compile line, conversions are inserted at the compiler / native interface to match the bit-width of float types expected by the compiler.

<sup>**5**</sup> Pointer types are two bytes for addressing 64k of memory.  sccz80 is capable of supporting three-byte pointer types for bankswitched systems but this feature is not generally taken advantage of by z88dk at this time.

**Notes:**

1. The C standard allows the implementation to decide whether char is signed or unsigned.  Always make that decision explicitly in your code by using a "signed char" or an "unsigned char" if the type will be used as an arithmetic type.  sdcc defaults to unsigned char which may be unexpected for many (--fsigned-char will change the default to signed).

2. Always prefer to use unsigned types whenever possible.  Careless use if signed types can lead to superfluous sign extension code inserted by the compiler.  The z80's instruction set is better suited to unsigned types.

New C code intended to be portable should use the data types defined in `<stdint.h>`.  These data types explicitly define the bit width in their names.  See int8_t, int16_t, int32_t, int64_t, uint8_t, uint16_t, uint32_t, uint64_t in particular.
