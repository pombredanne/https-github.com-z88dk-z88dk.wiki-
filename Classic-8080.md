After a few decades of not supporting 8080 code generation, z88dk can once again generate code that will run on 8080 and 8085 based computers.

## Supported targets

### 8080 CPU targets

* [PMD85](Platform---PMD85)
* [Vector06c](Platform-Vector06c)
* [Mikro 80](Platform--Mikro80)
* [Specialist](Platform-Special)
* [DAI](Platform---Dai)
* [Radio 86](Platform--Radio86)
* [Altair 8800](Platform--Altair8800)
* [LVIV](Platform---Lviv)
* [Krokha](Platform--Kroka)

Additionally, the CP/M target can generate 8080 binaries. To do so, add the option `-clib=8080` to the command line. For example:

```
zcc +cpm -clib=8080 adv_a.c
```

A .bin file will be generated that should run on 8080 based CP/M machines.

### 8085 CPU Targets

 * [TRS80 Model 100](Platform---M100)
 * RC2014 using the [8085 CPU Module](https://feilipu.me/2021/08/15/8085-cpu-on-the-z80-bus/)

The RC2014-8085 support consists of two subtypes specifically for use with the 8085 CPU Module.

The `basic85` subtype produces a HEX file for use with the [Microsoft Basic ROM](https://gitlab.com/feilipu/NASCOM_BASIC) (either with or without the APU Module), and can be uploaded to the RC2014 using the `HLOAD` Basic keyword.

Additionally, both the RC2014 `basic` and `basic85` subtypes are supported by the `z88dk-ticks` emulator. Where the resulting binary just needs to be titled `rc2014.bin`, and emulated with the correct machine type.
```
zcc +rc2014 -subtype=basic85 my_c_file.c my_asm_file.asm -o my_hex_file -create-app
```
The `acia85` subtype produces a HEX ROM file that provides serial interfaces using the RC2014 ACIA Serial Module.
```
zcc +rc2014 -subtype=acia85 my_c_file.c my_asm_file.asm -o my_hex_rom_file -create-app
```

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

There are two [maths libraries](Classic--Maths-Libraries) that run on the 8080/8085: `daimath32`, extracted from the [DAI](Platform---Dai) ROM, and `mbf32` Microsoft Basic Floating Point, extracted from Microsoft Basic 4.7 and optimised for the 8080, 8085,and z80 CPUs.

### z88dk extension libraries

The following extension libraries are not available in the 8080 library:

* Allocation: balloc
* Algorithm/adt: All
* Debug library

### Compiler support

Only `sccz80` supports generation of 8080 code.





