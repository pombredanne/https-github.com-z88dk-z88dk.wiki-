# Hardware summary

* Z80 @ 3.993600 Mhz
* 64k RAM, 32k ROM + 2K chargen
* VDP: HD46505 (m6845 clone), 48K VRAM total
* Audio: AY-3-8192 on ports 0x18 and 0x19

# Compilation

    zcc +multi8 world.c -create-app

The resulting .cas file can be loaded into an emulator by entering the following commands:

    MON
    R
    GC000

# Limitations

* Despite having 64k of RAM, z88dk compiles are limited to ~10k due to the memory map
* Only coloured text mode is supported by the console

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/multi8/)
* [Technical documentation](http://takeda-toshiya.my.coocan.jp/multi8/tech.html)