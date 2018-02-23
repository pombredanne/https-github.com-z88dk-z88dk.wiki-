

The Philips VG-5000 was a computer marketed mostly in France, about 70000 machines were sold.


# Quick start

	zcc +vg5k -subtype=wav -create-app -lm program.c

(ansi VT emulation is implicit)


The "-subtype=wav" option creates an audio stream.   If omitted only a ".K7" file, suitable for emulators, will be created.

"-subtype=fastwav" is a boosted variant, slightly speeding the generated audio stream.


The program can be loaded via the "CLOAD" command.   It will run automatically.

If the '-zorg=xxxx' option is used, the autorun will be disabled, in that case the program should be run with "CALL xxxx".


# External links

[VG5000 WebSite, emulator and software](http://dcvg5k.free.fr/)

[VG5000 WebSite](http://vg5k.free.fr/)
