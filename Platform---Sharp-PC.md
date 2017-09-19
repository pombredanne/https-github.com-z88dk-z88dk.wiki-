#  Sharp PC-G8xx, PC-E2xx..

The Sharp PC-Gxxx are programmable calculators featuring C and assembly low level programming features.


### Command Line

4 rows text:

    zcc +g800 -create-app program.c
    zcc +g800 -create-app -clib=g850 program.c


6 rows text, (progam must first initialize the screen by clearing it [e.g. fputc_cons(12)] ):

    zcc +g800 -create-app -clib=g850b program.c



### Emulator hints

The 'G800' emulator is a japanese SHARP PC-G850/G815/E200 emulator for Linux, MacOSX and Windows.

The z88dk compiled programs are automatically converted into Intel HEX files, which can be passed to the emulator as command line options, eg:

    g800 -machine=e200 a.ihx

At the emulated calculator's prompt, enter  'g100'  to run the program.


### Web links

http://ver0.sakura.ne.jp/pc/


