

![](images/platform/mtx.jpg)



# Quick start


### MTX 500 mode (32K):

    zcc +mtx -create-app -oadventure.bin adv_a.c

This memory model works with the MTX512 too, but before loading the user must type:

    POKE 64122,0 
    NEW


### MTX 512 mode (64K and more):

    zcc +mtx -create-app -startup=2 -oadventure.bin adv_a.c


### Extra options

The binary converter (appmake) will create a file named "adventure.wav" (along with binary file in MtxEmu format, used also in older MESS versions).

Other options for appmake include a 'turboload' mode (-Cz ––fast) and the MacTX MTB binary format (-Cz ––mtb).


### ROM mode

This is untested, add "-subtype=rom".


# Supported libraries

Most of the [monochrome gaphics](Library---monographics) lib and most of the [MSX](Platform---MSX) stuff are supported, but still experimental.


# Appmake extras

The appmake tool can be run in "dumb" mode to generate the corresponding audio track of some external program.

Example:

    C:\>appmake +mtx --dumb --audio -b ASTROPAC
    
    Info: Program Name found in header:  ASTROPAC
    
    Stklim: $f8f2
    System Variables block length ........... 601 byte(s)
    $FACC (prg block length) ................ 993 byte(s)
    $FA81 (CALCST): $c001
    $FA7B (VARNAM): $c000
    Variables block length .................. 1 byte(s)
    Extra data: 8576 byte(s), creating one more block

The optional "---fast" flag will produce a non-standard audio track which, even if faster, should be still loadable by the real computer.

# Links

http://primrosebank.net/computers/mtx/tools/PD/mtxtools_z88dk.htm

http://www.mtxworld.dk/main.php

