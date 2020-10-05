# Hardware

* Sharp LR35902 at 4.19 MHz
* 8k VRAM
* 8k General purpose RAM
* 32k cartridge + banked memory

## Classic library support (`+gb`)

* [x] Native console output (GBDK)
* [x] Native console input (GBDK)
* [ ] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
    * [x] Paper colour (APA mode)
    * [x] Ink colour (APA mode)
    * [x] Inverse attribute (APA mode)
    * [x] Bold attribute (APA mode)
    * [x] Underline attribute (APA mode)
* [x] Lores graphics
* [x] Hires graphics (via gbdk calls)
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

# Compilation

    zcc +gb -create-app [file.c]

A .gb file will be created suitable for insertion into an emulator

# Library support

The "standard" GBDK library is provided with z88dk. The standard library (printf, strings etc) is supplied by the usual z88dk libraries.

## Generic console

The following screen modes are supported by the generic console:

* "Text", mode 0, tile based, supports lores graphics (4x4 pixels) and scrolling
* "Drawing", mode 1, APA mode, supports lores graphics (via z88dk) and pixel graphics (via gbdk)

In mode 0, the font and UDGs are copied to the into the tilemap, as a result you should set the font and graphics once the mode has been switched.

### Custom fonts

The fonts within the z88dk library contain a header defining their size. If you use a custom font then you'll need to include the following [file](https://github.com/z88dk/z88dk/blob/master/libsrc/target/gb/fonts/lower.asm) before including your font as a binary blob. The linked file contains both a header and some additional graphics for lores graphics and the virtual keyboard.


## Limitations

* Graphics support is not complete

# Compiler support

Both `sccz80` and `zsdcc` are supported. 

## Banked memory

z88dk supports explicit bank placement for user code. Your functions should be placed into the `BANK_XX` section where `XX` is a hex number from 01 to 1F - note, sdcc support is not quite correct.

When the function is annotated as `__banked`, intra-bank calling will be enabled. Unlike gbdk, `__banked` functions can return `long` values.

# Links

* [Jeff Frohwein's pages](http://www.devrs.com/gb/) - historically the best gameboy development resource