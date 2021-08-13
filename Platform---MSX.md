
![](images/platform/msx.jpg)

## Classic library support (`+msx`)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font
    * [x] UDG support
    * [x] Paper colour
    * [x] Ink colour
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [x] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [X] One bit sound  (on the keyboard speaker)
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O (MSXDOS + MSXDOS2)
* [x] Interrupts
* [ ] RS232

### Quick start

    zcc +msx -create-app adv_a.c

This will generate two files, **a.bin** (a pure binary block to be run at $9c40, sometimes requiring extra data blocks) and **a.cas**, a fully packaged binary tape image suitable for the MSX emulators.

    zcc +msx -create-app -subtype=wav adv_a.c

This will create also 'a.wav', ready to be transferred on a tape.

For the above examples the BASIC command is  BLOAD "CAS:",R


    zcc +msx -subtype=msxdos -oadva.com -lm adv_a.c

This will generate a file, **adva.com** which is located at $100, as required by MSXDOS.

#### 16K model
To run on a 16K machine, you need to move the code origin to the upper half.
'-zorg=49200' should be a good option for you zcc command line.  Any working address for the 16k model will still work on the 32k models.

### ROM mode

The generate a ROM binary add the option `-subtype=rom`. In this mode, not all functions will work.

Under some condition, if lots of preset data is provided, an option permits to compress the default values before storing them in the ROM: -pragma-define:CRT_MODEL=2


#### MegaROM mode

MegaROMs can be created using z88dk - simply compile with `zcc +msx -subtype=rom...`. The memory organisation is assumed to be as follows:

```
- 0x4000 - 0x7fff = Fixed area, will contain crt0, z88dk library routines
- 0x8000 - 0xbfff = Paged area, treated as 16k chunks
- 0xc000 - 0xffff = RAM
```

Logically z88dk treats the memory map as being 16kb pages, transparently handling the fact that the ROM mapper may actually be an 8k mapper.

To place functions into banks, you should use the `#pragma bank NN` directive, where NN is a decimal number between 1 and 31. 

There is no default MegaROM mapper configured, to enable one, one of the following options need to be supplied to the zcc command line:

- `-pragma-define:MAPPER_ASCII16` - Enables the ASCII16 mapper
- `-pragma-define:MAPPER_ASCII8` - Enables the ASCII8 mapper
- `-pragma-define:MAPPER_KONAMI` - Enables the Konami without SCC mapper

From a C usage perspective there's no difference between the various mappers. Different mappers may be cheaper to build in hardware, or support more memory.

A simple example project is contained within `z88dk/examples/banked` - this example also works on the Gameboy.

### Disk subtype

*  compile the adv_a game

        zcc +msx -create-app -subtype=disk adv_a.c

It generates three files, a.bin, (pure binary file) and a.msx (binary file with some header stuff) as well as an a.img which is a MSX-DOS format disc image.

The file can be loaded from MSX-BASIC with the command:

	bload "a.msx",r

### MSXDOS subtype

z88dk provides MSXDOS1 (using CP/M bdos calls) and MSXDOS2 (using MSXDOS2 API calls) libraries for accessing files on disc.

        zcc +msx -create-app -subtype=msxdos adv_a.c

Will create a .img file containing a binary that uses BDOS calls to access files.

        zcc +msx -create-app -subtype=msxdos2 adv_a.c

Creates a .img containing a binary that uses MSXDOS2 API calls. Additional functionality such as `mkdir()`, `chdir()` is available when compiling with the msxdos2 subtype.


### The TMS9918a library

z88dk provides a [TMS9918a](Classic-TMS9918) library that allows hardware access to the VDP chip. All graphical functionality supplied by z88dk uses these functions and such is hardware independent.

### The VT/ANSI console driver

The ANSI driver provides a VT100 emulation, accepting escape codes that are compatible across the platforms that support the ANSI driver and of course unix terminals.
The conio.h variant provided by z88dk is tightly interconnected to this driver and provides useful extra features like cursor positioning and detection of the current txt resolution.

#### How to change the font size in the VT/ANSI emulation library

The selection of columns is now a link time option, for example with ansi test you can compile as follows:

    zcc +msx ansitest.c -create-app -lndos -pragma-need=ansiterminal -pragma-define:ansicolumns=64

Valid columns are:

     24, 28, 32, 36, 40, 42, 51, 64, 80, 85, 128

#### How to change the font that is used

On the MSX you can switch to the ROM font by specifying the following options to the compile line:

    -pragma-define:ansifont=7359 -pragma-define:ansifont_is_packed=0 -pragma-define:ansicolumns=32


### External Links



*  [Como programar para o MSX usando a linguagem C](http://fernando-aires.blogspot.it/2012/05/como-programar-para-o-msx-usando.html)

*  [MSX用クロス開発のすすめ（z88dk）, cross-development Recommendations for MSX and z88dk (Japanese)](http://juangotoh.hatenablog.com/entry/2015/10/29/231107)

*  [MSX Info pages](http://msx.hansotten.com/), maintained by Hans Otten

*  [MSX Assembly page](http://map.grauw.nl/), maintained by Laurens Holst (AKA "Grauw")
