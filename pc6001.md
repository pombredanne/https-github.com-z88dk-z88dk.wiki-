======= NEC PC-6001 =======

![](images/platform/pc6001c.gif)

The Japanese series of PC-6001 computers includes the following systems:

NEC Trek

PC-6001

PC-6001 MK II

PC-6601

# Quick start

	zcc +pc6001 -create-app -lm program.c
- or -
	zcc +pc6001 -clib=ansi -subtype=32k -oprogram -create-app -lm program.c



The following "subtype" options are supported:

    (no parameter) or 16k  ->  N60 BASIC (16K)
    32k                    ->  N60 BASIC (32K)
    n60m                   ->  N60m BASIC (32K)
    rom                    ->  Cartridge, max 16K, not all the libraries will work

Except forom the ROM mode, a file named 'program.cas' will be created.
To use the ezperimental audio options, try the following:

	zcc +pc6001 -create-app -lm -Cz--audio program.c
- or -
	zcc +pc6001 -create-app -lm -Cz--audio -Cz--fast program.c

It will create a WAV file too.

To load the program do the following:

	- Answer '2' to the "How Many Pages ?" question (mandatory for 32K modes).
	- Load the 'program.cas' file (can be renamed to 'program.p6' if your emulator requires it).
	- Issue the 'cload' BASIC statement.
	- Issue the 'run' command: the machine code block will be loaded and run.
	
If your program terminates and you wish to run it again, you can change the BASIC program line by modifying the two last digits of the location for EXEC from '0F' to '37.
This wil bypass the loader and enter in the program directly.


# ROM options

The -subtype=rom option can be enforced with extra options to pass to appmake:

To produce a file in Intel Hex format (cannot be mixed with chipsize):
`-Cz--ihex` 

To fill the whole ROM file
`-Cz"--romsize 16384"`

To split the ROM file in two smaller ones, etc..
`-Cz"--romsize 16384 --chipsize 8192"`

	

# Emulator Hints

	* "Virtual NEC Trek" is good and easy to use.

	* IP6WIN: If in trouble (japanese characters being typed), press F6.

	* Some emulator (and the untested P6DatRec tool) need the '.cas' file to be renamed to '.p6'
	

