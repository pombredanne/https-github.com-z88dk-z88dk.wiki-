# Various Tools Included with z88dk


## Build and binary conversion tools

See also the target section for platform specific tools.


### appmake

Normally it is not necessary to call "appmake" directly, it is embedded in the compilation process.
To activate it, add the **-create-app** flag to the 'zcc' command line.

### ihex

Reads in a binary file and outputs an Intel Hex Record

### inject

This tool inserts a binary block into a file (i.e. a memory dump) at the given position.

Usage:

    inject [binary file] [reference file] [position] [injected output file]

## Library tools


### z80nm library inspector

Tool to inspect the content of a z80asm library.

### rel2z80 library converter

Experimental tool to extract z80asm compatible binary blocks from a CP/M ".REL" library.


## Developement tools



### sprite editor

[Sprite editor](library/sprites/monosprites#the_sprite_editor_tool_by_daniel_mckinnon) by Daniel McKinnon

