
This port is still at an earlier stage.  Only simple console I/O is supported.



# Quick Start

    zcc +abc800 -zorg=40000 program.c

# Extra options

The ABC800 native console library is more generic than the ABC80 one and includes most of the ABC models, so it is possible to produce simple universal console programs by choosing the ABC800 target:

    zcc +abc800 -create-app -subtype=hex -zorg=49200 program.c

..will produce an Intel HEX file valid on ABC80SIM emulator.

With the latest tools by H. Peter Alvin (https://www.zytor.com/pub/abc80/abcdisk/) it is possible to produce a .dsk image that can be used on both the ABC80SIM and ABCWin emulator.

For example you may add this into your Makefile target:
```
	bin2bac2.exe a.bin progName.bac 49200
	dosgen.exe $@.dsk 
	doscopy.exe $@.dsk -b progName.bac
```

