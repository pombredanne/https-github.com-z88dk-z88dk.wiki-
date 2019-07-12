The classic library supports applications being compiled with different maths libraries. Selecting a different library will have implications regarding the size, performance and which machine configuration an application can run in.

## Size of floating point number

When compiling with zsdcc a float is always allocated 4 bytes in the C code. However with sccz80, the size of a float depends on which maths library is being used. Traditionally, sccz80 has always allocated a 6 byte block for representing floats, but some maths libraries support a 4 byte float.

## `-lm` - (genmath)

Genmath is z88dk's traditional maths library. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises a single index register and runs on all of classic's targets (subject to memory).

Genmath can only be used with the sccz80 compiler.

## `-lmath48`

math48 has been imported into classic from the newlib. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises the alternate register set and such can't run on all classic targets.

math48 can be used with both sccz80 and zsdcc and is marginally faster than genmath.

## `-lbbc_math` - (40 bit maths library from BBC BASIC for z80)

The BBC maths library provides a 32 bit mantissa and 8 bit exponent. It's the same library as the native maths library on the z88. It can be linked with the options: `-fp-mode=z88 -lbbc_math`. It can be used on ix/iy switched platforms with `-fp-mode=z88 -lbbc_math_iy`.

## `-lmbf32` - (Microsoft 32 bit library)

Support has been added for the (4 byte, 8 bit exponent, 24 bit mantissa) Microsoft single precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* Mitsubishi Multi 8 (10k mode only)
* Mattel Aquarius
* TRS80/Color Genie
* CCE MC-1000

The library can be linked with the following options: `-fp-mode=mbf32 -lmbf32`

Typically using the mbf32 will result in a roughly 20% decrease in floating point performance when compared to genmath/math48. However, the size of your application will be greatly reduced.

## `-lmbf64` - (Microsoft 64 bit library)

Support has been added for the (8 byte, 8 bit exponent, 56 bit mantissa) Microsoft double precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* TRS80/Color Genie

The library can be linked with the following options: `-fp-mode=mbf64 -lmbf64`

mbf64 can be used with sccz80.

## `-lmath32` - (IEEE 32 bit library)

The IEEE 32 bit provides a 32 bit float format that is mostly compliant with IEEE-754. The library can be used with both sccz80 and zsdcc using the following options:

`-fp-mode=ieee -lmath32 -pragma-define:CLIB_32BIT_FLOAT=1`

or to link the fast multiply for z80: 

`-fp-mode=ieee -lmath32_fast -pragma-define:CLIB_32BIT_FLOAT=1`

math32 supports the z180 (`-lmath32_z180`) and ZX Spectrum Next (`-lmath32_zxn`) hardware multiply instructions, providing accelerated performance for these platforms. The z80 CPU is also supported through emulation of the hardware multiply format `16_8x8`, and also provides good performance.

The intrinsic functions are written in assembler. The higher level functions (trigonometric, exp, pow) are implemented by C functions extracted from the Hi-Tech C Floating point library, and the Cephes Math Library.

More details on the library can be found within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/math/float/math32).

## Machine specific libraries

* `-lmathz88 -fp-mode=z88` 6 bytes. 8 bit exponent, 32 bit mantissa. Cambridge z88
* `-lmz` 6 bytes. 8 bit exponent, 32 bit mantissa. ZX Spectrum, ZX81 
* `-lcpcmath` 6 bytes. 8 bit exponent, 32 bit mantissa. Amstrad CPC

All of these libraries use the floating point package located in the machine's ROM. This can result in a compact binary

# Benchmarks

The maths libraries have been lightly benchmarked using a couple of test programs within the [repository](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/EXAMPLES/benchmarks). These were tested using the classic library and the following compilation line:

`zcc +test -DPRINTF [file.c] [maths flags]`

As a result, the numbers include the time spent printing, however this isn't particularly time consuming in the overall scheme:

## n-body

Library         | Compiler | Value 1       | Value 2       | Ticks
-|-|-|-|-
correct values  | -->      | -0.169075164  | -0.169087605
genmath         | sccz80   | -0.169075164  | -0.169087605  | 3_652_736_949
math48          | sccz80   | -0.169075164  | -0.169087605  | 2_402_023_498
math32          | sccz80   | -0.169075264  | -0.169086709  | 1_400_110_112 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#execution-speed)
math32_fast     | sccz80   | -0.169075264  | -0.169086709  | 1_199_895_142 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#execution-speed)
math32_zxn      | sccz80   | -0.169075264  | -0.169086709  | 0_578_058_768 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#execution-speed)
math32_z180     | sccz80   | -0.169075264  | -0.169086709  | 0_562_947_777 [*](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/math/float/math32/readme.md#execution-speed)
mbf32           | sccz80   | -0.169916810  | -0.169916810  | 1_939_334_701
bbcmath         | sccz80   | -0.169075164  | -0.169087604  | 1_655_789_776

## spectral-norm

Library         | Compiler |  Value         | Ticks
-|-|-|-
correct value   | -->      | 1.274219991
genmath         | sccz80   | 1.274219989   | 14_817_735_124
math48          | sccz80   | 1.274219989   | 09_035_519_932
math32          | sccz80   | 1.274219155   | 13_508_416_688
math32_fast     | sccz80   | 1.274219155   | 12_069_049_285
math32          | zsdcc    | 1.274219155   | 14_504_079_532
math32_zxn      | sccz80   | 1.274219155   | 06_396_544_633
math32_z180     | sccz80   | 1.274219155   | 06_120_760_761
mbf32           | sccz80   | 1.274220347   | 06_754_491_551
bbcmath         | sccz80   | 1.274219988   | 08_017_859_189