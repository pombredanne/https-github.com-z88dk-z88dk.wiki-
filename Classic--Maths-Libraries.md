The classic library supports applications being compiled with different maths libraries. Selecting a different library will have implications regarding the size, performance and which machine configuration an application can run in.

## Size of floating point number

When compiling with sdcc a float is always allocated 4 bytes in the C code. However with sccz80, the size of a float depends on which maths library is being used. Traditionally, sccz80 has always allocated a 6 byte block for representing floats, but some maths libraries support a 4 byte float.

## -lm (genmath)

Genmath is z88dk's traditional maths library. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises a single index register and runs on all of classic's targets (subject to memory).

Genmath can only be used with the sccz80 compiler.

## -lmath48

math48 has been imported into classic from the newlib. It provides a 48 bit number, with an 8 bit exponent and a 40 bit mantissa. It utilises the alternate register set and such can't run on all classic targets

math48 can be used with both sccz80 and sdcc and is marginally faster than genmath.

## Machine specific libraries

* `-lmathz88 -fp-mode=z88` 6 bytes. 8 bit exponent, 32 bit mantissa. Cambridge z88
* `-lmz` 6 bytes. 8 bit exponent, 32 bit mantissa. ZX Spectrum, ZX81 
* `-lcpcmath` 6 bytes. 8 bit exponent, 32 bit mantissa. Amstrad CPC

All of these libraries use the floating point package located in the machine's ROM. This can result in a compact binary

## mbf32 - (Microsoft 32 bit library)

Support has been added for the (4 byte, 8 bit exponent, 24 bit mantissa) Microsoft single precision library. This is available for machines that run Microsoft BASIC and the appropriate entry points have been located. To date, the following machines are supported:

* VTech Laser 500
* Mitsubishi Multi 8 (10k mode only)

The library can be linked with the following options: `-fp-mode=mbf32 -lmbf32`

Typically using the mbf32 will result in a roughly 20% decrease in floating point performance when compared to genmath/math48. However, the size of your application will be greatly reduced.

## IEEE 32 bit library

_Being written_
