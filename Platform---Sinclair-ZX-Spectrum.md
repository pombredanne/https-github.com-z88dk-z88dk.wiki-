#  ZX Spectrum  Platform

![](images/platform/spectrum.jpg)

## Hardware

* Z80 @ 3.54Mhz
* 48/128k RAM
* AY-3-891x (128k models only)


## Classic library support (`+zx`)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font
    * [x] UDG support
    * [x] Paper colour
    * [x] Ink colour (8 bit colour mod)
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [x] File I/O
* [x] Interrupts
* [x] RS232

# Compilation



## Building applications

The binary raw code can be relocated, made auto-relocating or left at its default position at $8000.
Tools are provided to package it in various ways, but the default one is the ".TAP" format.


### Command Line

    zcc +zx -lndos -create-app adv_a.c

or with the ANSI terminal:

    zcc +zx -clib=ansi -lndos -create-app adv_a.c

The -clib=ansi parameter enables the full VT/ANSI emulation (see below).

To get an audio file ready for the real hardware use the following command:
    zcc  +zx -clib=ansi -lm -create-app -subtype=wav program.c

Same as above but in turbo tape mode:
    zcc  +zx -clib=ansi -lm -create-app -subtype=turbo program.c


### Making a ROM

    zcc  +zx -subtype=rom -lndos -lm -create-app program.c

This command will compile the program at location '0', inserting a simplified interrupt handler (for the timer) and putting the system and the global variables just after the screen memory, appmake will be invoked to provide a 16K data block, fixed size.

Note that some of the libraries are not ready to work in ROM, thus a bit of tweaking can be necessary, even if many of them are now covered.

Under some condition, with lots of preset data is provided, an option permits to compress the default values before storing them in the ROM:   -pragma-define:CRT_MODEL=2


### Creating a bootable +3 disc

    zcc +zx -lndos -create-app adv_a.c -subtype=plus3

This command will create a bootable +3 disc suitable for autoloading from the main menu.


### +3 Locomotive CP/M

    zcc  +cpm -startup=3 -lzxcpm -lm -create-app program.c

This command will compile the program in [CP/M](Platform---CPM) mode permitting the use of some of the ZX Spectrum sound and graphics extensions.


## Math Libraries

The ROM calculator is supported by use of the `-lmzx` library. A more compact version of the library is available with `-lmzx_tiny` however this version lacks support for conversion to and from `long` variable types and parsing of scientific notation.

It should be noted that the ZX Maths libraries are of a lower precision than both _genmath_ and _math48_ and are also less performant. As a trade off, the amount of memory used is much reduced.


## File I/O

You can choose to link with a single library that implements file I/O functionality. 

### Spectrum +3 or Residos

    zcc +zx file.c -lp3

or:

    zcc +zx -DRESIDOS file.c -lp3

### Microdrive Support

Interface 1 can be supported by both the [generic basic driver](Platform-ZX-Basic-Driver) and a lowlevel library that will talk to the Interface 1 directly.


    zcc +zx file.c -lzxmdv

When using zxmdv.lib you'll have a very powerful tool, because the library talks to the Interface 1 at a very low level, permitting a true random access, and a real rename functionality (the only one we're aware of being able to rename files as big as the whole cartridge).
The library has been tested on the real hardware.

The number of contemporairly open file is limited only by the tape and the memory available space, but to achieve this the _malloc_ functions is used (see documentation for malloc).

The files created via the **open** function are of the so-called "PRINT" type, which means that you can create and edit them also with the BASIC OPEN command.


### ZX Basic Abstraction

To support the many disc systems that are available for the Spectrum, a [generic basic driver](Platform-ZX-Basic-Driver) can be linked in. You will have
to implement the BASIC program yourself and adapt to your disc system.


## The 'appmake' tool


The 'appmake' tool is activated by passing the "-create-app" parameter to the compiler.

Here are some other examples:

    zcc  +zx -lndos -lm -zorg=25000 -create-app program.c

The "-zorg=`<location>`" option locates the code at the desired position; it has effect both on the compiler and on the "appmake" tool, which will adapt the loader for the new location.

You can add options to the command line to modify the output .TAP file, for example: -Cz--screen -Czscreen.scr adds a picture while loading.

The available options are:

```
-h   --help            (bool)    Display this help
-b   --binfile         (string)  Linked binary file
-c   --crt0file        (string)  crt0 file used in linking
-o   --output          (string)  Name of output file
     --audio           (bool)    Create also a WAV file
     --ts2068          (bool)    TS2068 BASIC relocation (if possible)
     --turbo           (bool)    Turbo tape loader
     --extreme         (bool)    Extremely fast save (RLE)
     --patchpos        (integer) Turbo tape patch position
     --patchdata       (string)  Turbo tape patch (i.e. cd7fff..)
     --screen          (string)  Title screen file name
     --fast            (bool)    Create a fast loading WAV
     --dumb            (bool)    Just convert to WAV a tape file
     --noloader        (bool)    Don't create the loader block
     --noheader        (bool)    Don't create the header
     --merge           (string)  Merge a custom loader from external TAP file
     --org             (integer) Origin of the binary
     --blockname       (string)  Name of the code block in tap file
     --clearaddr       (integer) Address to CLEAR at
```

The 'patchpos' and 'patchdata' are for the turbo tape loader mods.


### Appmake Extras

The appmake tool can be run in “dumb” mode to generate the corresponding audio track of some external program. 

In example:

	appmake +zx -b cassette.tap --audio --fast --dumb

Builds the audio track for the given '.tap' file.
The optional ”fast” flag will produce a non-standard audio track which, even if faster, should be still loadable by the real computer. 



An advanced feature can glue together binary blocks to get a ready made package; in example:

	appmake +zx -b program.bin --org 32752 --screen screen.scr --fast --extreme -o program.tap


The screen can be passed in 'tap' or 'tzx' format too.

## Other Tools

### tapmaker

This is a quick and dirty hack originally written for
mymzx - this takes any file and creates a .TAP file for it
- the file contained is of type CODE and loads to address 0.

This tool appends to the end of the specified .TAP file so
watch out!


## ZX Library Features

### Calling Basic

Basic routines can be called using the routines in the `[<spectrum.h>]`(zxbasic) header file. 

### Interface 1

Low level access to microdrives is available through the `[<zxinterface1.h>]`(zxinterface1) header file.

### Opus Discovery

Support for the Opus Discovery is available through the `[<zxopus.h>]`(zxopus) header file.

### TRDOS/Betadisk support

### ZXMMC Support

### Currah uSpeech Support

The Currah uSpeech is supported using the `[<zxcurrah.h>]`(zxcurrah) header file.

### Low resolution (32x48 or 64x48) Graphics

A low resolution colour graphics mode is available using the `[<zxlowgfx.h]`(zxlowgfx) header file. This feature is unfortunately not fully
librarified and the header file contains the implementation.

### SP1 Sprite Library

The [sp1](sp1) sprite library is available in the classic library.

### Mouse Support

Direct access to both Kempston and AMX mouse hardware is defined in the `<spectrum.h>`  header file. 


## Console driver modes

### The standard ZX Spectrum console driver

The ZX console driver extends the [Generic Console](Classic-GenericConsole) and adds the following codes:

```
1,64	  - Switch to 64 column mode
1,32	  - Switch to 32 column mode
2,hh,ll   - Set the address of the 32 column font. ll is low byte, hh = high
	    byte. By default this points to the ROM font at 15616
18,x	  - Turn flash on/off (x=49/48 ('1'/'0')
19,x	  - Turn on/off bright 
```

All the commands, except for the font address can be embedded 
in strings using either octal or hexadecimal representation. Address changes
can be sent either using fputc() or using printf with the address as
parameters.

Characters greater than 127 are interpreted as udgs in 32 column mode.

It is recommended that the utility functions in `<conio.h>` are used to change text colours etc. Colours are indexed as per the colours in `<conio.h>`, to switch to native colour indices you can add the following option to the compile: `-pragma-define:CLIB_CONIO_NATIVE_COLOUR=1`

The screen scrolls when line 24 is "hit", the routine used is in the 48k ROM.

#### Fonts

By default the screen driver uses the ZX font for 32 column display and a custom one for 64 column
display. These can be easily changed at command line using a pragma directive:

    -pragma-redirect=CRT_FONT=<some label>

Or for the 64 column font:

    -pragma-redirect=CRT_FONT_64=<some label>

Some extra fonts are included within the classic library ZX library and can be activated using the
following pragma directives:

    -pragma-redirect=CRT_FONT=_font_8x8_bbc_system
    -pragma-redirect=CRT_FONT=_font_8x8_clairsys
    -pragma-redirect=CRT_FONT=_font_8x8_clairsys_bold

Alternatively, the font can be switched at runtime using the 0x02 escape code detailed earlier - to change the 32 column font, switch to 32 column mode and
similarly switch to 64 column mode to change the 64 column font.

The available fonts can be seen in the screenshots below:

![](images/platform/fonts/font1.png)
![](images/platform/fonts/font2.png)
![](images/platform/fonts/font3.png)
![](images/platform/fonts/font4.png)
![](images/platform/fonts/font5.png)

### The ROM driver

The classic library can alternatively use the rst 16 routine within the ZX ROM. This is a compile time option and can be selected by adding the following to the command line:

    -pragma-redirect:fputc_cons=fputc_cons_rom_rst

The routine disables the scroll? feature and naturally supports all of the standard ZX control codes. ASCII value 0x0a (10) is mapped through to 0x0d (13) for line return compatibility. In this mode, only the standard 24/26 UDGs are supported.

### The FZX driver

Alternatively FZX fonts can be used as the console output, to enable them compile with the flag:

    -pragma-need=fzxterminal

The font can be configured with the following pragma:

    -pragma-redirect:CRT_FONT_FZX=_ff..

Where the font name is the same as defined by newlib.

Scrolling is handled by the screen routine in ROM3 once more.

### The VT/ANSI console driver

The ANSI driver provides a VT100 emulation, accepting escape codes that are compatible across the platforms that support the ANSI driver and of course unix terminals.
The conio.h variant provided by z88dk is tightly interconnected to this driver and provides useful extra features like cursor positioning and detection of the current txt resolution.

#### How to change the font size in the VT/ANSI emulation library

The selection of columns is now a link time option, for example with ansi test you can compile as follows:

    zcc +zx ansitest.c -create-app -lndos -clib=ansi -pragma-define:ansicolumns=64

Valid columns are:

     24, 28, 32, 36, 40, 42, 51, 64, 80, 85, 128, ROM24, ROM28, ROM32 and ROM36.

#### How to change the font that is used

You can switch to the ROM font by specifying the following options to the compile line:

    -pragma-define:ansifont=15616 -pragma-define:ansifont_is_packed=0

### Links

[Death Star game, ZX Interface 2 ROM version](http://www.fruitcake.plus.com/Sinclair/Interface2/Cartridges/Interface2_RC_New_3rdParty_DeathStar.htm)

[Black Star game, a Space Invaders clone + sources](https://www.usebox.net/jjm/blackstar/)

[Introducción a Z88DK (Spanish)](http://wiki.speccy.org/cursos/z88dk/contenidos)