# Hardware summary

* Z80 @ 3.125 Mhz
  * 22k ROM
    * 32k RAM, 16k vRAM (TVC 32k)
    * 64k RAM, 16k vRAM (TVC 64k)
    * 64k RAM, 64k vRAM (TVC 64k+)
* VDP: 6845, 4bit RGB color (1-1 bit RGB and 1 bit luminance)
  * 3 graphics modes are available: 2 color, 4 color and 16 color modes (with 640, 320 and 160 pixel columns, respectively)
* Sound: 1 channel, 4 bit volume level, independent of CPU
* 4 expansion ports on top with almost all possible lines exposed
* 1 side-port for 'program module' (cartrigde games, programs)
* 1 centronics printer port
* 1 monitor + 1 RGB port
* 1 RF port

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

`clock()` is partially implemented. The `clock()` returns a long (casted to time_t), the number of elapsed 20.096ms during startup (in case the raster interrupts were not hijacked). The internal counter in TVC is 16bit, so this allows to count up to around 22mins.. No helper functions are implemented yet.

# Compilation

    zcc +tvc world.c -o world.bin -create-app

This will create a .cas image that's suitable for loading into the WinTVC emulator. Start emulator, press ALT-C (load CAS file directly).

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
