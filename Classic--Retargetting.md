#  Introduction 

Adding a new platform target to z88dk can be a fairly simple process depending on the commonality with existing targets. For example, to add a new TMS9918 target can be just a few hours work, with most of that time spent testing keyboard handling.

## Basic levels of support

A minimal port should consist of the following items:

* A zcc configuration file
* A crt0 file
* An implementation of fputc_cons() and library
* Appmake code to generate a file that can run on an emulator or on a real device

### ZCC configuration file

The configuration file tells zcc where to find the crt0, library file and defines any pre-processor macros that can be used to identify the target.

They are relatively small files, so it's usually best to copy the file from a target with similar hardware and search and replace the references for your chosen identifier.

### The crt0 file

The crt0 file contains the entry point and is responsible for defining the memory layout, setting up the assembler sections and defining constants such as CPU speed that are used by the library.

Again, it's easiest just to copy the file from a similar target and adjust memory addresses, values to suit.

### An implémentation of fputc_cons()

fputc_cons() is the function that will output a character to the display device. The file should be located in `{z88dk}/libsrc/stdio/[machineid]/fputc_cons.asm`. Typically this routine will consist of a call to the machine's firmware.

Once you've got the function it's time to create a library file. The contents of a library file are (by convention) defined in `{z88dk}/libsrc/machineid.lst`. The minimal contents of that file would be:

    stdio/[machineid]/fputc_cons
    @z80.lst

The library file will be built by `{z88dk}/libsrc/Makefile`, so here you need to add `[machineid]_clib.lib` to the `all:` target and then define a rule:

```
[machineid]_clib.lib:
        @echo ''
        @echo '--- Building [Machine] Library ---'
        @echo ''
        $(call buildgeneric,[machineid])
        TARGET=[machineid] TYPE=z80 $(LIBLINKER) -DSTANDARDESCAPECHARS -DFOR[machineid] -x$(OUTPUT_DIRECTORY)/[machineid]_clib @$(LISTFILE_DIRECTORY)/[machineid].lst
```

The classic library does not disturb the iy register, however it will use the ix register. As a result if your machine requires that ix is preserved then the last line of the rule should look like this:

```
        TARGET=[machineid] TYPE=ixiy $(LIBLINKER) --IXIY -DSTANDARDESCAPECHARS -DFOR[machineid] -x$(OUTPUT_DIRECTORY)/[machineid]_clib @$(LISTFILE_DIRECTORY)/[machineid].lst
```

This will force the library to be assembled in such way that where ix is used in a source file, it will be replaced with iy in the resulting binary.

You can then build the library with `make [machineid]_clib.lib` and copy the resulting file to `{z88dk}/lib/clibs/`

### Appmake support

All targets should support creating a file that will run on either an emulator or the real hardware. appmake has built in support for creating and padding ROM files and provides many routines to solve common file generation problems, however it's probable that your chosen machine isn't already supported.

Once you've done all of these steps, you should be able to compile and run a hello world example.

## Adding keyboard support

When keyboard input is required, the classic library will call the `fgetc_cons()` function. The implementation of this file should be in `{z88dk}/libsrc/stdio/[machineid]/fgetc_cons.asm`. Some parts of the library or examples require polling of the keyboarders using `getk()`, this file is located in `{z88dk}/libsrc/stdio/[machineid]/fgetc_cons.asm`

## Adding the generic console

...

## Adding PSG support

z88dk contains drivers for the AY-3-8910 and SN76489.

...

## Adding one bit sound support



...

## Adding graphics support

...

## Adding inkey keyboard reading

...

## Adding file I/O

Should file handling be required, then the following standard routines
from fcntl.h (or unistd.h as it is sometimes known), alternatively, should file handling not be required, then linking with the -lndos library will satisfy any compile-time requirements.

	int open(char *name, int flags, mode_t mode);
	int creat(char *name, mode_t mode);
	int close(int fd);
	size_t read(int fd, void *ptr, size_t len);
	size_t write(int fd, void *ptr, size_t len);
	long lseek(int fd,long posn, int whence);


	int __FASTCALL__ readbyte(int fd);

This function reads a byte from filehandle fd (which is 
supplied in the register pair hl), if an error occurred it
should return EOF (-1) and return with carry set. Otherwise
it should return with carry reset and hl holding the byte
just read

	int writebyte(int fd, int c);

This function writes byte c to filehandle fd, once more if
an error occurs it should return EOF and carry set, otherwise
hl holds the byte just written and carry is reset

	void fabandon(FILE *fp)

Abandon file with the handle fd - this is called by the system
on program exit should it not be able to close a file. This
function is a dummy function on the z88 but for example on the
Spectrum +3 this function would be of use.

