# Machine Specifications

* CPU: Z80 @ 3.579 Mhz
* VDP: TMS9918, 16k VRAM
* PSG: SN76489
* RAM: 1k 
* ROM: 8k

# Compilation

    zcc +coleco world.c -create-app

A 32k ROM file will be generated that can be inserted into the emulator.

# Support

The VDP used by the ColecoVision is the same as the one in the MSX, as such many MSX examples will work. However, the machine is not particularly well equipped with memory so care must be taken.

You can reduce RAM usage by making static data `const` where possible - this will keep it ROM rather than copying to ROM.

# Keypads and joypads

The Colecovision controller has a joystick as well as keypad with 12 keys. The joypad and two of the fire buttons are supported by the `joystick()` function with indices 1 and 2.

The keypad is additionally supported by `joystick()` with indices 3 and 4. In this mode, the joypad is returned in the lower byte and the upper byte contains an ascii value relating to the key that has been pressed: '0' - '9', '#', '*', 'C', 'D'

# Links

* [Emulator](http://takeda-toshiya.my.coocan.jp/pv2000/index.html)
