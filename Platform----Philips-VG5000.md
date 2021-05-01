## Hardware summary

The Philips VG-5000 was a computer marketed mostly in France, about 70000 machines were sold.

* Z80 @ 4Mhz
* 16kb ROM
* 24kb RAM
* EF9345 video processor, 25x40 columns

## Classic library support (`+pmd85`)

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [x] Paper colour
    * [x] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [x] Lores graphics
* [ ] Hires graphics (only plot)
* [ ] PSG sound
* [x] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232

# Quick start

	zcc +vg5k -subtype=wav -create-app -lm program.c


The "-subtype=wav" option creates an audio stream.   If omitted only a ".K7" file, suitable for emulators, will be created.

"-subtype=fastwav" is a boosted variant, slightly speeding the generated audio stream.


The program can be loaded via the "CLOAD" command.   It will run automatically.

If the '-zorg=xxxx' option is used, the autorun will be disabled, in that case the program should be run with "CALL xxxx".

## Custom font support

The VG5k hardware provides an 8x10 character grid, however by default within z88dk 8x8 characters are used, so each character is padded with a blank line at the top and bottom. This can lead to undesirable gaps when displaying UDGs. If the option `-pragma-define:CLIB_FONT_HEIGHT=10` is used, then all 10 lines of the character are filled. The font and UDG assets must contain 10 rows per character.

# External links

[VG5000 WebSite, emulator and software](http://dcvg5k.free.fr/)

[VG5000 WebSite](http://vg5k.free.fr/)
