#  TIKI-100

The Tiki-100 is a CP/M compatible platform, so the [same base library](platform/cpm) can be used, but extra functionalities are available.

Library extras include the sound support for the Yamaha PSG chip and the BW graphics, at the incredible resolution of 1024x256.

Target specific tricks permit to draw in 4 or 16 color mode as well.

![](images/platform/tikimandel.png)

### Command Line

    zcc  +cpm -lm -ltiki100 -oPROGRAM.COM program.c -create-app -subtype=tiki100


### Generic console

The generic console is available for the TIKI-100, the following screen modes are supported:

- Mode 1 - 1024x256x2 (128x32 characters)
- Mode 2 - 512x256x4 (64x32 characters)
- Mode 3 - 256x256x16 (32x32 characters)

In mode 1, the palette is white on black, in mode 3 a 16 colour palette broadly matching the colours defined in `conio.h` is available.

The palette can be changed using the `gr_setpalette()` defined in `tiki100.h`

Peeking for the character at a screen location `cvpeek()` only works reliably if the background is palette index 0 (default is black). Unlike other machines supporting pixel perfect colour which separate foreground and background or have a shadow screen available, on the tiki100 no such facility is available.

### Emulator hints

Currently the best TIKI-100 emulator we know is [MAME](http://www.mamedev.org/).
Another good option is the [Djupdal's Tiki-100 emulator](http://www.djupdal.org/tiki/emulator/), even if it is currently less accurate (i.e. it lacks the sound support).

Using the options `-create-app -subtype=tiki100` a TIKI-100 compatible disk is generated as part of the compilation. However you may wish to add the generated file onto a disk that already contains files. 

#### CPMTOOLS

We suggest to use the [CPMTOOLS](http://www.moria.de/~michael/cpmtools/) and edit an existing 200K or 400K disk image (the former KON-TIKI system versions don't support 400K disks).

First of all edit its "diskdefs" file and add a section:

	diskdef tiki400
	  seclen 512
	  tracks 80
	  sectrk 10
	  blocksize 2048
	  maxdir 128
	  skew 0
	  boottrk 2
	  os 2.2
	end

	diskdef tiki200
	  seclen 512
	  tracks 40
	  sectrk 10
	  blocksize 1024
	  maxdir 128
	  skew 0
	  boottrk 2
	  os 2.2
	end

To transfer a program on a disk image erase the previous copy and refresh it, i.e.:
    cpmrm -f tiki400 tikitpas.dsk 0:program.com
    cpmcp -f tiki400 tikitpas.dsk program.com 0:program.com


#### TikiDisk

A specific command line tool is also available ([tikidisk, by Asbjørn Djupdal](http://www.djupdal.org/tiki/emulator/)).

Supposing we are editing a disk image named "brage.dsk":

    tikidisk brage.dsk a a.com    <-- Add the 'a.com' file to the disk image (overwrite the existing copy if present)


