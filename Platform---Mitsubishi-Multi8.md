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

The default compile target is limited to about 10k. To be able to write a larger program we need to switch the machine into all ram mode and then load our program. The following is a non-automated way to do it:

    cd z88dk/support/multi8
    zcc +multi8 bootstrap.c -create-app -o bootstrap

This will create our bootstrap loader.

You can then compile your program with the `-subtype=64k` option:

    zcc +multi8 adv_a.c -subtype=64k -create-app -o adv_a

You can then concatenate the two files together:

    cat bootstrap.cas adv_a.cas > adventure.cas

And then load that file as usual.


# Limitations

* Only coloured text mode is supported by the console

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp/multi8/)
* [Technical documentation](http://takeda-toshiya.my.coocan.jp/multi8/tech.html)