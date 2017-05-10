======= The generic BASIC driver =======

The "zxbasdrv" driver is a trick to get some basic functionality
from the fcntl functions on a ZX Spectrum and to have a sort of
simple hardware abstraction layer for similar needs (hardcopy, etc).

There are some samples in the z88dk/support/zx/zxbasdrv directory: the zxbasdrv allows hardware to be driven from BASIC. If the hardware interface doesn't support streams, then only the block LOAD and SAVE functions can be implemented.

The ASM part extracts a drive path from the file names, so you can 
specify them with an MS-DOS like syntax.
Valid examples for file names are:
	"a:filename"	(drive #1)
	"H:filename"	.. i.e. 8th microdrive
	"filename"	(the file will be searched in drive #1)
	"2:filename"	..numbers are allowed, too !
	"P::"		Printer device on stream #3

======= Transparently linking the BASIC driver  =======

The 'appmake' tool has been extended to permit to pick a BASIC program from a TAP file and merge it with the loader.
Such feature makes easier the linkage of a driver at compile time, like in the following example:

    zcc +zxansi -lzxbasdrv -create-app  -zorg=27000 -Cz--merge -Czc:\if1.tap program.c

You may notice that the '**zxbasdrv**' library has been specified and that the '**--merge**' option is used to pass the appropriate BASIC portion to 'appmake'.


======= Description of the BASIC program line numbers  =======

Description of the BASIC program line numbers corresponding to the various driver functions



## 10 - Set default drive number

(just after the LOAD CODE command)
	set d=<default drive number>


## 7499 - query driver description

	s$ = Name/Description


## 7500 - OPEN sequential file

	s=stream number (4-15); the ASM part detects the existence of the channel and handles a "file not found" condition if it is missing
	f=mode flag (0=read, 256=write, etc..)
	d=drive number (it is extracted from the file name, see above)
	n$=file name
	


## 7550 - CLOSE sequential file

	s=stream number



## 7600 - LOAD block

	a=address or 0 if unspecified
	l=length or 0 if unspecified
	d=drive number
	n$=file name


## 7650 - SAVE block

	a=address
	l=length
	d=drive number
	n$=file name


##  7700 - Init printer channel (open #3)

Optionally you can handle a "file name" to redirect the printer output.
This entry works as a sort of "OPEN" command, but the stream number must be forced to #3.


## 7750 - Close printer channel (close #3)

Here the drivers should normally just close the stream #3.


## 7800 - Hardcopy

Do hardcopy (or dump screen to file, or whatever you prefer)


## 7900 - DELETE file

	d=drive number
	n$=file name


## 7950 - RENAME file

	d=drive number
	n$=old file name
	o$=new file name

