
This port is still at an earlier stage.  Only simple console I/O is supported.



# Quick Start

    zcc +abc800 -zorg=40000 program.c

# Extra options

The ABC800 native console library is more generic than the ABC80 one and includes most of the ABC models, so it is possible to produce simple universal console programs by choosing the ABC800 target:

    zcc +abc800 -create-app -subtype=hex -zorg=49200 program.c

..will produce an Intel HEX file valid on ABC80SIM emulator, but currently due to emulator limits it can be tested only in ABC80 mode.

