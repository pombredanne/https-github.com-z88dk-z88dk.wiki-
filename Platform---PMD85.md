# Hardware summary

* MHB 8080A  @ 2.048Mhz
* 32k contiguous RAM
* 16k VRAM giving 288x256 resolution screen.

## Classic library support (`+pmd85`)

* [ ] Native console output
* [ ] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [ ] Paper colour
    * [x] Ink colour (Colour Ace only)
    * [x] Inverse attribute
    * [x] Bold attribute
    * [x] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics (only plot)
* [ ] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

# Compilation

    zcc +pmd85 program.c -create-app

A .ptp file will be generated suitable for loading into Mame or PMD85 emulator.

# PMD85 display

## Font support

The character cell on the PMD85 is natively 6 pixels wide and 8 pixels high. The generic console display routine will take the left-most 6 pixels within an 8x8 font to display on screen. The following fonts within z88dk work well:

- font_8x8_einstein_system
- font_8x8_msx_system
- font_8x8_sam_system
- font_8x8_pmd85_system

## Graphics support

The standard z88dk graphics library makes extensive use of z80 features. The PMD85 is built using an 8080 compatible processor so much of the library isn't available at present. However the low-level plot/xorplot/unplot/pointxy operations are available.

## Colour support

The `textcolor()` operation maps the colour correctly for PMD85 models with the ColorAce modification.


# Links

* PMD85 Emulator and docs: https://pmd85.borik.net/wiki/Emulator

