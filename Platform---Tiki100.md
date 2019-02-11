#  TIKI-100

The Tiki-100 is a CP/M compatible platform, so the [same base library](platform/cpm) can be used, but extra functionalities are available.

Library extras include the sound support for the Yamaha PSG chip and the BW graphics, at the incredible resolution of 1024x256.

![](images/platform/tikimandel.png)

### Command Line

    zcc  +cpm -lm program.c -create-app -subtype=tiki100

Will create a 40 track .dsk image suitable for use with an emulator or writing on to a physical floppy.

### Generic console

The generic console is available for the TIKI-100, the following screen modes are supported:

- Mode 1 - 1024x256x2 (128x32 characters)
- Mode 2 - 512x256x4 (64x32 characters)
- Mode 3 - 256x256x16 (32x32 characters)

In mode 1, the palette is white on black, in mode 3 a 16 colour palette broadly matching the colours defined in `conio.h` is available.

The palette can be changed using the `gr_setpalette()` defined in `tiki100.h`

Peeking for the character at a screen location `cvpeek()` only works reliably if the background is palette index 0 (default is black). Unlike other machines supporting pixel perfect colour which separate foreground and background or have a shadow screen available, on the tiki100 no such facility is available.

### Graphics

Plotting automatically detects which screen mode and plots a pixel accordingly. You can change the colour of the plotted pixel using the `textcolor()` function from conio.h


### Emulator hints

Currently the best TIKI-100 emulator we know is [MAME](http://www.mamedev.org/).
Another good option is the [Djupdal's Tiki-100 emulator](http://www.djupdal.org/tiki/emulator/), even if it is currently less accurate (i.e. it lacks the sound support).

Using the options `-create-app -subtype=tiki100` a TIKI-100 compatible disk is generated as part of the compilation. 