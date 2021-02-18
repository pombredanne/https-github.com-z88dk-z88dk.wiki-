#  Bondwell Model 2

## Classic library support (`+cpm -subtype=bw2`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font
    * [x] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O
* [ ] Interrupts
* [ ] RS232

The Bondwell 2 are CP/M compatible, so the [same base library](Platform---CPM) can be used.

Library extras include the high resolution graphics support (640x200).

![](https://user-images.githubusercontent.com/10224992/107541499-79240600-6bc7-11eb-956a-ec23608a0a71.jpg)

### Command Line

    zcc  +cpm -subtype=bw2 -create-app program.c

A .dsk disc image (and a .COM file) will be created as a result.


### Emulator hints

[MAME](http://www.mamedev.org/) was recently updated and an issue related to the floppy disk access slowness has been solved. 

The command line options to boot CP/M and use the created disk image will be something like:

    mame64 bw2 -flop1 bondwl02.td0

After the CP/M boot, change the virtual floppy disk image (choose A.DSK).

Alternatively, the Dunfield's tools can be used (TD2IMD, TD02IMD and IMDU) to convert the CP/M system disk image into a RAW unencoded file, suitable to be altered with cpmtools.
The file A.COM is built together with the packaged A.DSK

# Links

* Information: http://www.thebattles.net/bondwell/
* Bios source code: http://www.thebattles.net/bondwell/BIOS02.MAC
