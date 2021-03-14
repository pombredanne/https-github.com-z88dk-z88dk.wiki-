#  Amstrad PCW

## Classic library support (`+cpm -subtype=pcw40`, `+cpm -subtype=pcw80`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [ ] Generic console
    * [ ] Redefinable font
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Hires graphics
* [ ] PSG sound (DK'Tronics)
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O
* [ ] Interrupts
* [ ] RS232

The Amstrad PCW computers are CP/M compatible, so the [same base library](Platform---CPM) can be used.

Library extras include the high resolution graphics support (720x256).


### Command Line

    zcc  +cpm -subtype=pcw40 -create-app program.c

    zcc  +cpm -subtype=pcw80 -create-app program.c

A .dsk disc image (and a .COM file) will be created as a result.


### Emulator hints

#### MAME

[MAME](http://www.mamedev.org/) keeps the pcw models mostly in single drive configuration at startup, the virtual disk must be configured after starting the emulator.

In example, to run a program for on the PCW9256, an 80 tracks disk image must be built (-subtype=pcw40).

After getting the '.dsk' file for your program, MUST must boot CP/M:

    mame64 pcw9256 -flop1 cpm3.td0

After the CP/M boot, enable the second disk controller and issue a "reset" command on MAME.

Then, enter the disk menu and configure "a.dsk" for the second floppy drive.   "reset" again.

Now the system should identify the disk format properly, type:  "b:a.com"  to load and run your program.


The file A.COM is built together with the packaged A.DSK for customized file transfer processes.


#### JOYCE

Depending on the machine configuration, a valid disk image will need to be either 40 or 80 tracks.

Please note that the disk menu can be misleading, be sure to confirm your choice ("OK") on exit.

Under some circumstance (mostly with the 40T format) the created disk will be readable only on the same drive used to boot.

#LINKS

http://www.fvempel.nl/basic.html

