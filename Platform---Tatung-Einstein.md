# Machine Specifications

* CPU: Z80 @ 4Mhz
* VDP: TMS9928, 16k VRAM + TK-02 (MC6845 with 2k VRAM)
* PSG: AY-3-8910
* RAM: 64k


## Classic library support (`+cpm -subtype=einstein`)

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
* [ ] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [x] File I/O
* [ ] Interrupts
* [ ] RS232

The Tatung Einstein is a CP/M compatible platform, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.

![](images/platform/einstein-wall.jpg)
![](images/platform/einstein.jpg)

The Einstein video generated by a TMS9929A VDP, as a result the MSX libraries available.

### Command Line

    zcc  +cpm -lm -subtype=einstein -oPROGRAM.COM program.c -create-app

A .dsk image file will be created that can be loaded as the second disc in emulators. For example:

   mame einstein -flop1 dos80.dsk -flop2 PROGRAM.dsk

When the Einstein boots enter:

```
1:APP.COM
```

And your program will start. 

Alternatively, you can omit -create-app and use the [EDIP program](http://www.einstein.talktalk.net/edip.html) to create a bootable disc image.

Be sure to create upper case files to make them readable by the Einstein operating system.

### TK02 80 column card

The TK02 is supported via switching to mode 10 using `console_ioctl`, you'll need to start Mame as follows:

```
mame einstein -pipe tk02 -flop1 SYSTEM80.DSK
```

### Emulators

* Mame

### Web links

* https://web.archive.org/web/20121212082438/http://www.einstein.talktalk.net
* http://www.tatungeinstein.co.uk/
