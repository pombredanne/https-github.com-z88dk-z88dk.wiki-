# Hardware summary

* Z80 @ 3.125 Mhz
  * 22k ROM
    * 32k RAM, 16k vRAM (TVC 32k)
    * 64k RAM, 16k vRAM (TVC 64k)
    * 64k RAM, 64k vRAM (TVC 64k+)
* VDP: 6845 
* Sound: 1 channel, 4 bit volume level, independent of CPU

## Classic library support (`+tvc`)

* [x] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [ ] Generic console
    * [ ] Redefinable font 
    * [ ] UDG support
* [ ] Lores graphics
* [ ] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Compilation

    zcc +tvc world.c -o world.bin -create-app

This will create a .cas image that's suitable for loading into the WinTVC emulator. Start emulator, press ALT-C (load CAS file directly)

# Generic console modes

* Mode 2 = 64x24 text, 640x240 graphics
* Mode 4 = 32x24 text, 320x240 graphics
* Mode 16 = 16x24 text, 160x240 graphics

# Screenshots

// TODO

# Limitations

* In stdio I'm still struggling the fgetc_cons routine using the ROM editor. Sooner or later, but it will be there
* No sound support
* No graphics support
* The current max size of a file is around 26kB, so it will fit into 32k also.
* No VT-DOS, no UPM support
* No support for extra hardware (floppy card (original), serial card (original), multichip soundcard (new), multicart (new) )

# Links

* [WinTVC emulator](http://tvc.homeserver.hu/html/wintvcletoltes.html)
* [PCZ80TVC emulator](http://users.atw.hu/atkalabor/z80emuk.htm)
