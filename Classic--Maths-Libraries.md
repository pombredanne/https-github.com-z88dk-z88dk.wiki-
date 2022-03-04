The classic library supports applications being compiled with different maths libraries. Selecting a different library will have implications regarding the size, performance and which machine configuration an application can run in.

## Size of floating point number

When compiling with zsdcc a float is always allocated 4 bytes in the C code. However with sccz80, the size of a float depends on which maths library is being used. Traditionally, sccz80 has always allocated a 6 byte block for representing floats, but some maths libraries support a 4 byte (32 bit) float.

## 48 bit genmath - z80/z80n - `-lm`

Genmath is z88dk's traditional maths library. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises a single index register and runs on all of classic's targets (subject to memory). It should be noted that the library makes extensive use of the ixh and ixy registers and as such cannot be used on the Z180 or clones that do not support them,

__Genmath can only be used with the sccz80 compiler.__

## 48 bit maths by Anders Hejlsberg - z80/z180/z80n - `-lmath48`

math48 has been imported into classic from the newlib. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises the alternate register set and therefore can't run on all classic targets.

math48 can be used with both sccz80 and zsdcc compilers, and is marginally faster than genmath.

## BBC Maths - (40 bit maths from BBC BASIC for z80) - z80/z180/z80n

The BBC maths library provides a 32 bit mantissa and 8 bit exponent. It's the same library as the native maths library on the z88. It can be linked with the options: `--math-bbc`. It can be used on ix/iy switched platforms with `-fp-mode=z88 -lbbc_math_iy`.

__BBC Maths can only be used with the sccz80 compiler.__


## MBF32 - (32 bit maths from Microsoft) - 8080/8085/gbz80/z80/z180/z80n

Support has been added for the (4 byte, 8 bit exponent, 24 bit mantissa) Microsoft single precision library. 

This library can be used in two ways. As a self-contained maths library that can run on any of the supported processors and as stub library that utilises the ROM code for targets that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported for this use case:

* VTech Laser 500
* Mitsubishi Multi 8 (10k mode only)
* Mattel Aquarius
* TRS80/Color Genie
* CCE MC-1000

The library can be linked with the following options: `--math-mbf32`, `--math-mbf32_8080`, `--math-mbf32_8085`, or  `--math-mbf32_GBZ80`, depending on the target CPU.

Some basic optimisation for the 8085 (using undocumented instructions) and z80 has been undertaken. Mainly this involves more effective load and store routines, and improved register shift processes.

__MBF32 can only be used with the sccz80 compiler.__


## DAI32 Maths - (32 bit maths extracted from the DAI target ROM) - 8080/z80/z180/z80n

This maths library was extracted from the DAI computer, it offers, a 4 byte, 7 bit exponent, 24 bit mantissa floating point number. It's of particular interest since the floating point format is compatible with the AM9511.

The library can be linked with the following options: `--math-dai32` or `--math-dai32_8080`

__DAI maths can only be used with the sccz80 compiler.__


## MBF64 - (64 bit maths from Microsoft) - z80

Support has been added for the (8 byte, 8 bit exponent, 56 bit mantissa) Microsoft double precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* TRS80/Color Genie


__mbf64 can only be used with the sccz80 compiler.__

## 32 bit maths using IEEE-754 format - z80/z180/z80n - `--math32`

math32 provides a 32 bit floating point format that is mostly compliant with IEEE-754, which is also the native floating point format of zsdcc. The library can be used with both sccz80 and zsdcc.

`math32` supports the z180 and ZX Spectrum Next z80n `16_8x8` hardware multiply instructions, providing accelerated performance for both these platforms. The z80 CPU is supported through `24_16x8` and `32_16x16` unrolled mantissa multiply routines which provide good performance.

The intrinsic functions are written in assembler. The higher level functions (trigonometric, exp, pow) are implemented by C functions extracted from the Cephes Maths Library and the Hi-Tech C Floating point library.

More details on the library can be found within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/math/float/math32).

## 16 bit maths using IEEE-754 format - z80/z180/z80n - `--math16`

math16 provides a 16 bit floating point format that is mostly compliant with IEEE-754. The library can be used with both sccz80 and zsdcc.

`math16` supports the z180 and ZX Spectrum Next z80n hardware multiply instructions, providing accelerated performance for both these platforms. The z80 CPU is supported through `32_16x16` unrolled mantissa multiply routines which provide good performance.

The IEEE 16 bit floating point standard supports a maximum of 3.5 significant digits of accuracy, but it is very fast.
It is useful for graphics, as per this Cambridge Z88 example comparing the IEEE16 library with the standard maths library.

![Cube example on Cambridge Z88](https://user-images.githubusercontent.com/394721/87208645-26b7f980-c307-11ea-93c5-dc111e9320c5.gif)

The intrinsic functions are written in assembler. The intrinsic type is `_Float16` for sccz80, or `half_t` for both sccz80 or sdcc.

The sccz80 compiler supports `_Float16` as a native type, and therefore arithmetic or comparison operations on variables defined as `_Float16` or `half_t` will be done without conversion. The sdcc compiler doesn't support intrinsic 16 bit floating point operations, but functions can still be called as needed.

The `math16` library is considered an adjunct library. When printing format conversion and other facilities are required, it is linked together with a main maths library (which provides the conversion routines). The invocation command will typically include `--math32 --math16`, or `-lm --math16` to provide both main and adjunct maths libraries.

More details on the library can be found within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/math/float/math16).

## Machine specific libraries

* `-lmathz88 -fp-mode=z88` 6 bytes. 8 bit exponent, 32 bit mantissa. Cambridge z88
* `-lmz` 6 bytes. 8 bit exponent, 32 bit mantissa. ZX Spectrum, ZX81 
* `-lcpcmath` 6 bytes. 8 bit exponent, 32 bit mantissa. Amstrad CPC
* `--math-mbf32`, as noted above. Various Microsoft BASIC machines.

All of these libraries use the floating point package located in the machine's ROM. This can result in a compact binary

__These libraries can only be used with the sccz80 compiler.__

# Maths library aliases

Aliases are provided to make usage of maths libraries straight forward. Including the alias in the zcc invocation will provide all necessary configuration options relevant for each maths library.

## classic

* __`--math-mbf32`__ is alias for `-Cc-fp-mode=mbf32-lmbf32 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_MBF32 -Ca-D__MATH_MBF32 -D__MATH_MBF32`
* __`--math-mbf32_8080`__ is alias for `-Cc-fp-mode=mbf32 -lmbf32_8080 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_MBF32 -Ca-D__MATH_MBF32 -D__MATH_MBF32`
* __`--math-mbf32_8085`__ is alias for `-Cc-fp-mode=mbf32 -lmbf32_8085 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_MBF32 -Ca-D__MATH_MBF32 -D__MATH_MBF32`
* __`--math-mbf32_gbz80`__ is alias for `-Cc-fp-mode=mbf32 -lmbf32_gbz80 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_MBF32 -Ca-D__MATH_MBF32 -D__MATH_MBF32`
* __`--math-mbf64`__ is alias for `-Cc-fp-mode=mbf64 -lmbf64`
* __`--math-bbc`__ is alias for `-Cc-fp-mode=z88 -lbbc_math`
* __`--math-dai32`__ is alias for `-Cc-fp-mode=am9511 -ldaimath32 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_DAI32 -Ca-D__MATH_DAI32 -D__MATH_DAI32`
* __`--math-dai32_8080`__ is alias for `-Cc-fp-mode=am9511 -ldaimath32_8080 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_DAI32 -Ca-D__MATH_DAI32 -D__MATH_DAI32`
* __`--math-am9511_8085`__ is alias for `-Cc-fp-mode=ieee -lam9511_8085 -pragma-define:CLIB_32BIT_FLOATS=1 -Cc-D__MATH_AM9511 -Ca-D__MATH_AM9511 -D__MATH_AM9511`

## classic + newlib

The `--math16` and `--math32` aliases are supported across z80, z180, and z80n (SpectrumNext) architectures, and automatically provides each target with the correct hardware multiply opcodes (or for z80 emulation code) to enable best performance.

The `math16` library uses 16-bit (integer) memory moves extensively, and therefore optimising code by in-lining memory get and put subroutines has a positive impact on performance, without increasing program size substantially.

* __`--math32`__ is alias for `-Cc-D__MATH_MATH32 -D__MATH_MATH32 -Cc-fp-mode=ieee -pragma-define:CLIB_32BIT_FLOATS=1 -lmath32`
* __`--math16`__ is alias for `-Cc-D__MATH_MATH16 -D__MATH_MATH16 --opt-code-speed=inlineints -lmath16`

# Benchmarks

The maths libraries have been lightly benchmarked using a couple of test programs within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks). These were tested using the classic library and the following compilation line:

`zcc +test -DPRINTF [file.c] [maths flags]`

As a result, the numbers include the time spent printing, however this isn't particularly time consuming in the overall scheme:

## [n-body](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/n-body)

Library                  | Compiler | Value 1       | Value 2       | Ticks
-|-|-|-|-
correct values           | -->      | -0.169075164  | -0.169087605
genmath                  | sccz80   | -0.169075164  | -0.169087605  | 3_652_736_949
mbf32 (gbz80)            | sccz80   | -0.169075083  | -0.169086337  | 2_555_855_304
math48                   | sccz80   | -0.169075164  | -0.169087605  | 2_377_856_525
mbf32 (z80)              | sccz80   | -0.169075083  | -0.169086337  | 1_936_249_932
daimath32                | sccz80   | -0.1690751    | -0.1690863    | 1_899_271_269
bbcmath                  | sccz80   | -0.16907516   | -0.16908760   | 1_655_789_776
math32                   | sccz80   | -0.1690752    | -0.1690867    | 0_993_265_277 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)
math32_z80n              | sccz80   | -0.1690752    | -0.1690867    | 0_576_942_516 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)
math32_z180              | sccz80   | -0.1690752    | -0.1690867    | 0_563_700_933 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)

## [spectral-norm](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/spectral-norm)

Library                  | Compiler |  Value         | Ticks
-|-|-|-
correct value            | -->      | 1.274219991
genmath                  | sccz80   | 1.274219989   | 14_817_735_124
math32                   | zsdcc    | 1.274219      | 10_003_973_822
math32                   | sccz80   | 1.274219      | 09_718_997_187
math48                   | sccz80   | 1.274219989   | 09_035_519_932
bbcmath                  | sccz80   | 1.27421999    | 08_017_859_189
mbf32 (gbz80)            | sccz80   | 1.274220      | 07_941_220_888
daimath32                | sccz80   | 1.274220      | 07_483_466_471
mbf32 (z80)              | sccz80   | 1.274220      | 06_754_491_551
math32_z80n              | sccz80   | 1.274219      | 06_396_544_633
math32_z180              | sccz80   | 1.274219      | 06_120_760_761

## [mandelbrot](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/mandelbrot)

Library                  | Compiler | Ticks
-|-|-
genmath                  | sccz80   | 3_589_992_847
math48                   | zsdcc    | 3_766_086_833
math48                   | sccz80   | 3_266_168_305
math32                   | zsdcc    | 1_410_662_416
math32                   | sccz80   | 1_142_045_217 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
math16                   | sccz80   | 1_103_113_465
math32_z80n              | sccz80   | 0_922_658_537 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
math32_z180              | sccz80   | 0_892_842_610 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
