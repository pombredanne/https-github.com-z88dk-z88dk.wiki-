The Philips VG-5000 was a computer marketed mostly in France, about 70000 machines were sold.


# Quick start

	zcc +vg5k -subtype=wav -create-app -lm program.c

(ansi VT emulation is implicit)


The "-subtype=wav" option creates an audio stream.   If omitted only a ".K7" file, suitable for emulators, will be created.

The program can be loaded via the "CLOAD" command, and run with "CALL 20480".

# Generic Console

The generic console implementation supports font configuration using CRT_FONT and 128 UDGs. The VG5k is somewhat unusual in that its characters are 8x10 pixels. To simplify portability, both font and UDGs should be passed as 8 line characters. These are then expanded out to 10 lines by the library. 

# External links

[VG5000 WebSite, emulator and software](http://dcvg5k.free.fr/)

[VG5000 WebSite](http://vg5k.free.fr/)

