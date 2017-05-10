#  Introduction 

As can be seen from the number of platforms that z88dk can generate code for, retargetting the kit for a new platform can be a trivial process.

This document is split into several sections, each of which adds an added degree of functionality to the new target, if you wish to retarget to a new machine then it is recommended that the directory structure mentioned in this documentis followed - this permits easier integration and prevents the source tree from getting overly messy, with over 1500 files and over 7MB of code this is a very important thing to do!

# Startup Code

The first thing to do is to create a crt0 file in the z88dk/lib directory, this file is used for startup and should initialise the stack, set the origin of the program, parse any command line arguments (if supported), call the main function and then release resources on program completion.

There are a number of recommended starting places for these files, dependent on whether the application is ROM based or RAM based. If,

   * The system is ROM based, base the startup file on lib/rex_crt0.asm
   * The system is RAM based, base the startup file on lib/cpc_crt0.asm

The name of the file should be something along the lines of [3/4 letter id]_crt0.asm. This 3/4 letter id should be the unique indentifier that will be used throughout the source tree (Yes, the Spectrum is different, but this was a mistake a long time ago!).

In addition to the .asm file, a file with the suffix .opt is required, this should include the .asm, each for the z88, this file contains:

	
	        INCLUDE         "z80_crt0.asm"


Whilst in the lib/ directory, a .cfg file should be created lib/config directory for the target. Base it on the configuration file for the startup that was used.



[z88dk forum - Information on how the crt0 files are being extended for the future z88dk releases.](http://www.z88dk.org/forum/viewtopic.php?id=8655)





# Floating Point Routines

A set of floating point routines is supplied which will work on any Z80
(it does use undocumented opcodes, but...), however should that target have a an FP calculator in the firmware, it could be advantageous to take advange of it. Use the mathz88.asm file in the lib directory as a template. To avoid the need to change the compiler, the -doublestr flag can be supplied to the compilation which will use the native functions to change floating point strings into values at run time. This obviously increases the overhead of using floating point arithmetic, and as such it might be desirable to add a constant to the compiler to represent the value 0.1.

# Stdio Library Routines

## Initial Steps

Create a directory z88dk/libsrc/stdio/[machine id]. This is the directory that will contain the basic routines that are needed for supporting a machine. 

##  Required Functions 

	int fgetc_cons(void)


Should read a key from the keyboard returning the ASCII
key value in hl. This routine should return 0 if no key
is pressed, however unless an error occurs it should wait
for a keypress.

	void fputc_cons(char code)


Should print code to the screen

	int fgets_cons(char *buf, int maxlen)


Should read a string from the keyboard. Done this way because
some computers (i.e. z88) have an OS call to do this which
offers editing facilities

	void puts_cons(char *str)


*Optional* Writes a NUL terminated string to the console, this call
is provided to conserve memory usage should no other stdio
routines be required

	int getk(void)


*Optional* Should read the current state of the keyboard, returning 0 if no key is pressed

# File I/O

## Initial Steps

Create a directory libsrc/fcntl/machine_id

## Required Functions

Should file handling be required, then the following standard routines
from fcntl.h (or unistd.h as it is sometimes known), alternatively, should file handling not be required, then linking with the -lndos library will satisfy any compile-time requirements.

	int open(far char *name, int flags, mode_t mode);
	int creat(far char *name, mode_t mode);
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

Once the above functions are available, the entire z88dk stdio library
(including printf etc) is available for use, in addition to all the ctype,
string, assert, setjmp, near malloc, some stdlib and the generic math
routines - just a little work yields over 100 usable functions!

