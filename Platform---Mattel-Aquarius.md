# Hardware summary

* Z80 @ 3.5Mhz
* 4kb RAM (expandable to 28kb)
* 8kb ROM (Microsoft BASIC)
* 40x24 character mapped display
* Optional AY8192


# Classic library support (`+aquarius`)

* [x] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font
    * [ ] UDG support
    * [x] Paper colour
    * [x] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics (80x72 and 80x48)
* [ ] Hires graphics
* [x] PSG sound
* [x] One bit sound  (on the keyboard speaker)
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O (MSXDOS + MSXDOS2)
* [ ] Interrupts
* [ ] RS232

## Quick start

    zcc +aquarius -lm -create-app -o adventure adv_a.c

-or-

    zcc +aquarius -clib=ansi -lm -create-app -o adventure adv_a.c

The binary converter (appmake) will create two files: _adventure.caq and adventure.caq


The first one is the BASIC loader, and the second one is the binary block.
They should be loaded in sequence; type **CLOAD**, play the first file, then type **RUN** and play the second one.

### Building ROM images

z88dk supports building 8k ROM images for the Aquarius:

    zcc +aquarius world.c -subtype=rom -create-app

The ROM file can then be inserted using the emulator. Depending on the emulator, you may need to reset the machine following insertion.

_Note_ mame loads ROMs by default without expanded RAM. The memory configuration z88dk chooses for the stack causes issues with the ROM screen printer. If you print to console then you must use `-pragma-redirect:fputc_cons=fputc_cons_generic` to switch to the generic console.

## Library support

Lores graphics by default is 80x72, a more evenly sized 80x48 graphical mode is available by linking `-lgfxaq48`

## Emulator notes

Tested under the "Virtual Aquarius" emulator, by James the Animal Tamer.

	- run the emulator				aquarius
	- from within the emulator, load the loader:	cload
	- press ENTER twice, then play the "_adventure.caq" cassette file (loader)
	- at the "OK" prompt, type RUN <ENTER>
	- press ENTER twice, then play the "adventure.caq" cassette file
	- wait... then have fun !

# Links

* [(atariage.com) - Yet another Death Star port](http://atariage.com/forums/topic/173559-intellivision-homebrew-istar-wip/)
* [(atariage.com) - Sprite demo for the Mattel Aquarius](http://www.atariage.com/forums/topic/173909-aquarius-sprite-demo-complied-using-the-z88dk-devkit/)
* [(atariage.com) - HW development idea: 320x200-bitmap-graphics-hack !](http://atariage.com/forums/topic/233221-aquarius-320x200-bitmap-graphics-hack/)
* [(atariage.com) - z88dk Aquarius Development](http://atariage.com/forums/topic/220410-aquarius-z88dk-aquarius-development/)
