#  ZX Spectrum  Platform

{{platform:spectrum.jpg|}}


## Introduction

The ZX Spectrum platform is very well supported in the Z88 Development Kit.
All the generic functions are covered (excepting the FAR memory model) and many platform specific libraries are available for both the standard 48K (or 16K) model and the various clones and peripherals.


## Building applications

The binary raw code can be relocated, made auto-relocating or left at its default position at $8000.
Tools are provided to package it in various ways, but the default one is the ".TAP" format.


### Command Line

    zcc +zx -lndos -create-app adv_a.c
-or-
    zcc +zx -clib=ansi -lndos -create-app adv_a.c

This command will build a .TAP tape image, linking a foo file library and the default math support.
To save some memory it is possible to use the ZX Spectrum specific math library, by changing the "-lm" parameter with "-lmzx" or "-lmzx_tiny", but it will run slower and with less precision.

The -pragma-need=ansiterminal parameter enables the full VT/ANSI emulation (see below).

If file support is required it is possible to change the "-lndos" parameter and pass a library name (see the [ZX Spectrum libraries](library/zxspectrum/fcntl) section).

To get an audio file ready for the real hardware use the following command:
    zcc  +zx -clib=ansi -lm -create-app -subtype=wav program.c

Same as above but in turbo tape mode:
    zcc  +zx -clib=ansi -lm -create-app -subtype=turbo program.c


### Making a ROM

    zcc  +zx -subtype=rom -lndos -lm -create-app program.c

This command will compile the program at location '0', inserting a simplified interrupt handler (for keyboard, etc) and putting the system and the global variables just after the screen memory, appmake will be invoked to provide a 16K data block, fixed size.

Note that some of the libraries are not ready to work in ROM, thus a bit of tweaking will probably be required, this is not for beginners.


### +3 Locomotive CP/M

    zcc  +cpm -startup=3 -lzxcpm -lm -create-app program.c

This command will compile the program in [CP/M](platform/cpm) mode permitting the use of some of the ZX Spectrum sound and graphics extensions.

### Pragma directives

`<file>`
#pragma output nostreams   - Don't allocate storage for file streams

#pragma output norom3      - Don't include code for calling into the BASIC ROM - certain library functions may need this
`</file>`

### The 'appmake' tool


The 'appmake' tool is activated by passing the "-create-app" parameter to the compiler, as already shown.
Here are some other examples:



    zcc  +zx -lndos -lm -zorg=25000 -create-app program.c

The "-zorg=`<location>`" option locates the code at the desired position; it has effect both on the compiler and on the "appmake" tool, which will adapt the loader for the new location.


Edit {z88dk}/lib/config/zx.cfg to customize your binary package (i.e. -Cz--screen -Czscreen.scr adds a picture while loading).
The available options are:

`<file>`

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
     --merge           (string)  Merge a custom loader from external TAP file
     --org             (integer) Origin of the binary
     --blockname       (string)  Name of the code block in tap file

`</file>`


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

### Other Tools

#### tapmaker

This is a quick and dirty hack originally written for
mymzx - this takes any file and creates a .TAP file for it
- the file contained is of type CODE and loads to address 0.

This tool appends to the end of the specified .TAP file so
watch out!


#### bin2tap

** Warning:  this tool overwrites the specified .TAP file **

(this one is also included in the standard compiler command line options):

Creates .TAP file with a very simple BASIC loader at the beginning.
After converting, a LOAD "" is sufficient to run the code.




#### bin2bas-rem

** Warning:  this tool overwrites the specified .TAP file **

For skilled developers only.

Creates a new TAP file (overwriting if necessary) with a BASIC program line in which we store the self-relocating binary file as if it was a REMark.

The relocation routine is patched by this tool to fit with the ZX Spectrum behaviour.

To compile a relocatable program:
`<file>`
	zcc +zx -Ca-R -lm -lndos program.c
	bin2bas-rem a.bin a.tap <line number>
`</file>`

To run the machine code:
`<file>`
	PRINT USR (PEEK 23635+256*PEEK 23636+5)
`</file>`

Alternatively it is possible to make use of the system variable 23637 (location of the next line to be run) and stuff different programs in a single BASIC source: 
`<file>`

	  1 DEF FN l()=USR(PEEK 23637+256*PEEK 23638+5)
	100 LET x=FN l()
	101 REM (program)
	200 LET x=FN l()
	201 REM (another merged program)

Hints:   DO NOT edit the REM line.  You need to place it at its right position with this tool.
         If you are in trouble try to re-MERGE the REM lines onto you main BASIC block.

`</file>`


## Advanced use of the ZX Spectrum libraries



### The standard ZX Spectrum console driver

The new screen driver is still fairly minimal but it does feature 64 column text and 32 column text (switchable) and can be easily extended.

Here are the byte sequences for it:

`<file>`

1,64	  - Switch to 64 column mode
1,32	  - Switch to 32 column mode
2,hh,ll   - Set the address of the 32 column font. ll is low byte, hh = high
	    byte. By default this points to the ROM font at 15616
3,hh,ll   - Set the address for the UDGs by default this points to
	    65368

8,9,10,11 - Move in x and y as you would expect
12	  - Form feed - clears the screen and moves print posn to 0,0
13	  - Carriage return - advances y and sets x to 0

16,x	  - Set the ink colour  (x = 48-55 ('0'-'7'))
17,x	  - Set the paper colour (x = 48-55 ('0'-'7'))
18,x	  - Turn flash on/off (x=49/48 ('1'/'0')
19,x	  - Turn on/off bright 
20,x      - Turn on/off inverse (x=49/48 ('1'/'0')

22,y,x	  - Move to position y,x on the screen (0<=y<=23, 0<=x<=63)
	    NB. y and x are displaced by 32 eg to move the print position
	    to (0,0) use 22,32,32. 32 column mode also uses 64 column
	    coordinates.

`</file>`

All the commands, except for the udg address/font address  can be embedded 
in strings using either octal or hexadecimal representation. Address changes
can be sent either using fputc() or using printf with the address as
parameters.

Characters greater than 127 are interpreted as udgs. These are always
printed using the 32 column mode. This gives a potential of 128 udgs
though obviously, it's possible to swap between banks in the middle of
printing a string.

The screen scrolls when line 24 is "hit", the routine used is in the
48k ROM.







### The VT/ANSI console driver



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

[Black Star game, a Space Invaders clone + sources](https///www.usebox.net/jjm/blackstar/)

[Introducción a Z88DK (Spanish)](http://wiki.speccy.org/cursos/z88dk/contenidos)
