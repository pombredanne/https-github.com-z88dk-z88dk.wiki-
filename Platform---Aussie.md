# Knight 2000 (Aussie Byte)

The Aussie Byte is a CP/M compatible platform, so the [same base library](platform/cpm) can be used, but extra functionalities are available.
Library extras include the experimental (non working) high resolution graphics and the sound support for the 1-bit sound.



### Command Line

CP/M mode:

    zcc  +cpm -lm -create-app -laussie -oPROGRAM program.c

#### CPMTOOLS

For the CP/M format we suggest to use the [CPMTOOLS](http://www.moria.de/~michael/cpmtools/) **compiled with the LibDsk option** and edit an existing 84 tracks disk image.

An already built and configured version is available in the [Microbee Software Preservation Project](http://http://www.microbee-mspp.org.au/repository)


First of all edit its "diskdefs" file and add a section:

	# Knight SME Aussie Byte 5.25" DS DD 80x5x1024
	diskdef k80dsdd1024
	  seclen 1024
	  cylinders 80
	  sectrk 5
	  heads 2
	  sideoffs 128
	  blocksize 2048
	  maxdir 256
	  skew 2
	  skewstart 1
	  datasect 1
	  boottrk 4
	  os 2.2
	end


To transfer a program on a disk image erase the previous copy and refresh it, i.e.:
    cpmrm -T dsk -f k80dsdd1024 knight2000_operating.dsk 0:a.com
    cpmcp -T dsk -f k80dsdd1024 knight2000_operating.dsk a.com 0:a.com

The resulting disk image can be associated to the MESS emulator as a command line option:
    mame aussieby -flop1 knight2000_operating.dsk

Other disk images may have been created in RAW format (no libdsk header).

Many other formats are avaiable, in the mentioned pre-configured cpmtools variant.





