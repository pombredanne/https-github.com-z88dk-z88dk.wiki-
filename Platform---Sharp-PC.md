#  Sharp PC-G8xx, PC-E2xx..

The Sharp PC-Gxxx are programmable calculators featuring C and assembly low level programming features.

Basic support (24x4 text mode) is provided via BIOS (IOCS) for the following models:
PC-E200, PC-E220, PC-G801, PC-G802, PC-G803, PC-G805, PC-G811, PC-G813, PC-G815, PC-G820, PC-G830

Advanced graphics functions are available for:
PC-G850, PC-G850V

Sound mod is supported.


### Command Line

4 rows text:

    zcc +g800 -create-app program.c


6 rows text:

    zcc +g800 -create-app -clib=g850b program.c


8 rows text, (progam must first initialize the screen by clearing it [e.g. fputc_cons(12)] ):

    zcc +g800 -create-app -clib=g850 program.c



### Emulator hints

The 'G800' emulator is a japanese SHARP PC-G850/G815/E200 emulator for Linux, MacOSX and Windows.

The z88dk compiled programs are automatically converted into Intel HEX files, which can be passed to the emulator as command line options, eg:

    g800 -machine=e200 a.ihx

At the emulated calculator's prompt, enter  'g100'  to run the program.


### Web links

http://ver0.sakura.ne.jp/pc/
http://ver0.sakura.ne.jp/g800/
http://ver0.sakura.ne.jp/doc/
http://wwwhomes.uni-bielefeld.de/achim/pc-e220.html


