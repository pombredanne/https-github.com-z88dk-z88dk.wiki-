# ZX Microdrive fcntl drivers

There are two ways to use the ZX Microdrives with Z88DK:

 1.  to use the [generic BASIC driver](library/zxspectrum/fcntl/zxbasdrv)
 2.  to use the native library (zxmdv.lib)

## zxmdv.lib

When using zxmdv.lib you'll have a very powerful tool, because the library talks to the Interface 1 at a very low level, permitting a true random access, and a real rename functionality (the only one we're aware of being able to rename files as big as the whole cartridge).
The library has been tested on the real hardware.

The number of contemporairly open file is limited only by the tape and the memory available space, but to achieve this the "malloc" functions is used (see documentation for malloc).

The files created via the **open** function are of the so-called "PRINT" type.
It means that you can create and edit them also with the BASIC OPEN command.

