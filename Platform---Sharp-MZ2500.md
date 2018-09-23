# Specifications

* Z80 @ 6Mhz
* YM2203 audio
* 128k Main RAM + 128k Extension RAM
* ??k VRAM

# Quick start

    zcc +mz2500 -create-app program.c

An autoboot .dsk image is created automatically by appmake. At the moment, programs up to 24k are supported.

In addition to "native" compiles, this machine can also run CP/M. The z88dk +cpm target supports generating discs in the required format:

    zcc +cpm -subtype=mz2500 -create-app program.c

# Generic console

The generic console is enabled by default on the mz2500. The following screen modes are supported:

* Mode 0: Text, 40x24
* Mode 1: Text, 80x24

# Limitations

This port is at an early stage and support is minimal.

# Links

* [Takeda Emulator](http://takeda-toshiya.my.coocan.jp) - tested emulator
* [MZ-2500 Manual (Japanese)](http://www.8bity.cz/2017/sharp-mz-2500-user-manual/)
* CP/M discs and other software at theoldcomputer
