# Hardware summary

* Z80 @ 3.125 Mhz
  * 20k ROM, and depending on computer type
    * 32k RAM, 16k vRAM (TVC 32k)
    * 64k RAM, 16k vRAM (TVC 64k)
    * 64k RAM, 64k vRAM (TVC 64k+). The 16kB vRAM pages can be switched and mapped, but the same videomodes are available.
* VDP: 6845, 4bit RGB color (1-1 bit RGB and 1 bit luminance)
  * 3 graphics modes are available: 2 colour, 4 colour and 16 colour modes (with 640, 320 and 160 pixel columns, respectively)
* Sound: 1 channel, 4 bit volume level, independent of CPU
* 4 expansion ports on top with almost all possible lines exposed
* 1 side-port for 'program module' (cartridge games, programs)
* 1 centronics printer port
* 1 monitor + 1 RGB port
* 1 RF port

_This target is currently being created_

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

# Screenshots

// TODO

# Limitations

* In stdio I'm still struggling the fgetc_cons routine using the ROM editor. Sooner or later, but it will be there
* No sound support
* No graphics support
* The current max size of a file is around 26kB, so it will fit into 32k also.
* No VT-DOS, no UPM support
* No support for extra hardware (floppy card (original), serial card (original), multichip soundcard (new), multicart (new) )

Under wine, you may need to change the video driver in the WinTVC emulator to "Direct3D9":  "Opci??k -> Video Driver"

# Links

* [WinTVC emulator](http://tvc.homeserver.hu/html/wintvcletoltes.html)
* [PCZ80TVC emulator](http://users.atw.hu/atkalabor/z80emuk.htm)
