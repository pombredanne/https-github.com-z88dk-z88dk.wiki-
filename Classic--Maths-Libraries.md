The classic library supports applications being compiled with different maths libraries. Selecting a different library will have implications regarding the size, performance and which machine configuration an application can run in.

## Size of floating point number

When compiling with zsdcc a float is always allocated 4 bytes in the C code. However with sccz80, the size of a float depends on which maths library is being used. Traditionally, sccz80 has always allocated a 6 byte block for representing floats, but some maths libraries support a 4 byte (32 bit) float.

## `-lm` - (48 bit genmath)

Genmath is z88dk's traditional maths library. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises a single index register and runs on all of classic's targets (subject to memory).

Genmath can only be used with the sccz80 compiler.

## `-lmath48` - (48 bit math by Anders Hejlsberg)

math48 has been imported into classic from the newlib. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises the alternate register set and therefore can't run on all classic targets.

math48 can be used with both sccz80 and zsdcc and is marginally faster than genmath.

## `-lbbc_math` - (40 bit math from BBC BASIC for z80)

The BBC maths library provides a 32 bit mantissa and 8 bit exponent. It's the same library as the native maths library on the z88. It can be linked with the options: `-fp-mode=z88 -lbbc_math`. It can be used on ix/iy switched platforms with `-fp-mode=z88 -lbbc_math_iy`.

## `-lmbf32` - (32 bit math from Microsoft)

Support has been added for the (4 byte, 8 bit exponent, 24 bit mantissa) Microsoft single precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* Mitsubishi Multi 8 (10k mode only)
* Mattel Aquarius
* TRS80/Color Genie
* CCE MC-1000

The library can be linked with the following options: `-fp-mode=mbf32 -lmbf32`

Typically using the mbf32 will result in a roughly 20% decrease in floating point performance when compared to genmath/math48. However, the size of your application will be greatly reduced.

## `-lmbf64` - (64 bit math from Microsoft)

Support has been added for the (8 byte, 8 bit exponent, 56 bit mantissa) Microsoft double precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* TRS80/Color Genie

The library can be linked with the following options: `-fp-mode=mbf64 -lmbf64`

mbf64 can be used with sccz80.

## `-lmath32` - (32 bit math using IEEE-754 format)

The IEEE 32 bit provides a 32 bit float format that is mostly compliant with IEEE-754. The library can be used with both sccz80 and zsdcc using the following options:

`-fp-mode=ieee -lmath32 -pragma-define:CLIB_32BIT_FLOAT=1`

or to link the fast (table) multiply for z80: 

`-fp-mode=ieee -lmath32_fast -pragma-define:CLIB_32BIT_FLOAT=1`

math32 supports the z180 (`-lmath32_z180`) and ZX Spectrum Next z80n (`-lmath32_z80n`) hardware multiply instructions, providing accelerated performance for these platforms. The z80 CPU is also supported through emulation of the hardware multiply format `16_8x8`, and also provides good performance.

The intrinsic functions are written in assembler. The higher level functions (trigonometric, exp, pow) are implemented by C functions extracted from the Hi-Tech C Floating point library, and the Cephes Math Library.

More details on the library can be found within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/math/float/math32).

## Math Library Aliases

Aliases are provided to make usage of math libraries straight forward. Including the alias in the zcc invocation will provide all necessary configuration instructions for each math library.

### classic

* __`--math-mbf32`__ is alias for `-Cc-fp-mode=mbf32 -lmbf32`
* __`--math-mbf32_8080`__ is alias for `-Cc-fp-mode=mbf32 -lmbf32_8080`
* __`--math-mbf64`__ is alias for `-Cc-fp-mode=mbf64 -lmbf64`
* __`--math-bbc`__ is alias for `-Cc-fp-mode=z88 -lbbc_math`

### classic + newlib

* __`--math32`__ is alias for `-Cc-fp-mode=ieee -Cc-D__MATH_MATH32 -D__MATH_MATH32 -lmath32 -pragma-define:CLIB_32BIT_FLOAT=1`
* __`--math32_fast`__ is alias for `-Cc-fp-mode=ieee -Cc-D__MATH_MATH32 -D__MATH_MATH32 -lmath32_fast -pragma-define:CLIB_32BIT_FLOAT=1`
* __`--math32_z180`__ is alias for `-mz180  -Cc-fp-mode=ieee -Cc-D__MATH_MATH32 -D__MATH_MATH32 -lmath32_z180 -pragma-define:CLIB_32BIT_FLOAT=1`
* __`--math32_z80n`__ is alias for `-mz80n  -Cc-fp-mode=ieee -Cc-D__MATH_MATH32 -D__MATH_MATH32 -lmath32_z80n -pragma-define:CLIB_32BIT_FLOAT=1`

## Machine specific libraries

* `-lmathz88 -fp-mode=z88` 6 bytes. 8 bit exponent, 32 bit mantissa. Cambridge z88
* `-lmz` 6 bytes. 8 bit exponent, 32 bit mantissa. ZX Spectrum, ZX81 
* `-lcpcmath` 6 bytes. 8 bit exponent, 32 bit mantissa. Amstrad CPC

All of these libraries use the floating point package located in the machine's ROM. This can result in a compact binary

# Benchmarks

The maths libraries have been lightly benchmarked using a couple of test programs within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks). These were tested using the classic library and the following compilation line:

`zcc +test -DPRINTF [file.c] [maths flags]`

As a result, the numbers include the time spent printing, however this isn't particularly time consuming in the overall scheme:

## [n-body](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/n-body)

Library         | Compiler | Value 1       | Value 2       | Ticks
-|-|-|-|-
correct values  | -->      | -0.169075164  | -0.169087605
genmath         | sccz80   | -0.169075164  | -0.169087605  | 3_652_736_949
math48          | sccz80   | -0.169075164  | -0.169087605  | 2_402_023_498
mbf32           | sccz80   | -0.1699168    | -0.1699168    | 1_939_334_701
bbcmath         | sccz80   | -0.16907516   | -0.16908760   | 1_655_789_776
math32          | sccz80   | -0.1690752    | -0.1690867    | 1_398_993_950 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)
math32_fast     | sccz80   | -0.1690752    | -0.1690867    | 1_198_780_765 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)
math32_z80n     | sccz80   | -0.1690752    | -0.1690867    | 0_576_942_516 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)
math32_z180     | sccz80   | -0.1690752    | -0.1690867    | 0_563_700_933 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#n-body)

## [spectral-norm](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/spectral-norm)

Library         | Compiler |  Value         | Ticks
-|-|-|-
correct value   | -->      | 1.274219991
genmath         | sccz80   | 1.274219989   | 14_817_735_124
math32          | zsdcc    | 1.274219      | 14_504_079_532
math32          | sccz80   | 1.274219      | 13_508_416_688
math32_fast     | sccz80   | 1.274219      | 12_069_049_285
math48          | sccz80   | 1.274219989   | 09_035_519_932
bbcmath         | sccz80   | 1.27421999    | 08_017_859_189
mbf32           | sccz80   | 1.274220      | 06_754_491_551
math32_z80n     | sccz80   | 1.274219      | 06_396_544_633
math32_z180     | sccz80   | 1.274219      | 06_120_760_761

## [mandelbrot](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks/mandelbrot)

Library         | Compiler | Ticks
-|-|-
genmath         | sccz80   | 3_589_992_847
math48          | sccz80   | 3_323_285_174
math48          | zsdcc    | 3_205_062_412
math32          | zsdcc    | 1_670_409_507
math32          | sccz80   | 1_653_612_845 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
math32_fast     | sccz80   | 1_495_633_606 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
math32_z80n     | sccz80   | 0_922_658_537 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
math32_z180     | sccz80   | 0_892_842_610 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#mandelbrot)
