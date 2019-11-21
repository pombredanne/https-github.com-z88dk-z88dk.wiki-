# Hardware summary

* Z80 @ 3.57Mhz
* 2k RAM
* AY8910
* VDP: TMS9928, 16k VRAM

## Classic library support (`+myvision`)

* [ ] Native console output
* [x] Native console input
* [x] ANSI vt100 engine
* [x] Generic console
    * [x] Redefinable font 
    * [x] UDG support
* [ ] Lores graphics
* [x] Hires graphics
* [x] PSG sound
* [ ] One bit sound
* [x] Inkey driver
* [x] Hardware joystick
* [ ] File I/O
* [x] Interrupts
* [ ] RS232

# Compilation

    zcc +myvision program.c -create-app

A 24k ROM file will generated which can be loaded into mame using the following command line:

    mame myvision -cart program.rom

# Keyboard and Joystick handling

The keyboard on this console is arranged as follows:

```
                       B
                  A          D    E
                       C
  1 2 3 4 5 6 7 8 9 10 11 12 13   14
```

These are mapped into the following keycodes (hex) returned by `fgetc_cons()`:

```
                       11
                  08         09    45
                       10
  31 32 33 44 55 36 37 38 39 14 15 16 17   18
```

Joystick 1 uses keys A,B,C,D,E,10, joystick 2 uses keys 1,2,3,4,5,6

# Additional library

The VDP used by the MyVision is the same as the one in the MSX, as such many MSX examples will work. However, the machine is not particularly well equipped with memory so care must be taken.

You can reduce RAM usage by making static data `const` where possible - this will keep it ROM rather than copying to RAM.

# Links

* Mame emulator