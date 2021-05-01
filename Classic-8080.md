After a few decades of not supporting 8080 code generation, z88dk can once again generate code that will run on 8080 and 8085 based computers.

## Supported targets

The following targets are 8080 only:

* [PMD85](Platform---PMD85)
* [Vector06c](Platform-Vector06c)
* [Mikro 80](Platform--Mikro80)
* [Specialist](Platform-Special)
* [DAI](Platform---Dai)
* [Radio 86](Platform--Radio86)
* [Altair 8800](Platform--Altair8800)
* [LVIV](Platform---Lviv)
* [Krokha](Platform--Kroka)
* [TRS80 Model 100](Platform---M100) (8085)

Additionally, the CP/M target can generate 8080 binaries. To do so, add the option `-clib=8080` to the command line. For example:

```
zcc +cpm -clib=8080 adv_a.c
```

A .bin file will be generated that should run on 8080 based CP/M machines.

## Limitations

The 8080 CPU implements a subset of the z80 instruction set that the compilers and libraries within z88dk have traditionally targeted. As a result not all features are available. If you use a feature that is not available then your program will fail to link.

### `<stdio.h>`

The following features from `<stdio.h>` are not available in the 8080 library:

* `funopen()` and stdio pluggable device support
* The `scanf` family. This can be restored if required.

### `<stdlib.h>`

The following features from `<stdlib.h>`are not available in the 8080 library:

* `qsort` and `bsearch`
* z88dk extensions: `inp`, `outp`, `extract_bits`

### `<math.h>`

There are two maths libraries that run on the 8080: daimath32 and mbf32. Daimath32 is contained within the repository, however for mbf32, we only ship the bindings with z88dk not the implementation.

### z88dk extension libraries

The following extension libraries are not available in the 8080 library:

* Allocation: balloc
* Algorithm/adt: All
* Debug library

### Compiler support

Only `sccz80` supports generation of 8080 code.





