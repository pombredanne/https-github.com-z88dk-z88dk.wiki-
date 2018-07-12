### Introduction

This port is an improvement of the ZX Spectrum one, which already includes some automatic fix to solve the compatibility problems on the TS2068.

Most of the extra functionalities permit to take benefit of the extended video mode for text and graphics.

Please refer to the [ZX Spectrum platform](platform/zx) docs for further information about most of the available features (appmake, etc).



### Command Line

Spectrum mode:

    zcc  +ts2068 -subtype=nohrg -create-app program.c

Extended mode:

    zcc  +ts2068 -create-app program.c

-or-

    zcc  +ts2068 -clib=ansi -create-app program.c

### Extended functions

Extra functionalities are available for such platform, notably the [Sprite Pack 1 library](library/sprites/sp1) supporting the 512x192 graphics resolution.

## Math Libraries

The ROM calculator is supported by use of the `-lm2068` library. 

It should be noted that the ZX Maths libraries are of a lower precision than both _genmath_ and _math48_ and are also less performant. As a trade off, the amount of memory used is much reduced.


### Links

[Alvin's site about Timex computers](http://www.geocities.com/aralbrec/)
