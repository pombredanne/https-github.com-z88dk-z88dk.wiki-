#  Otrona Attachè

The Otrona Attachè computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.
Library extras include the graphics support (320x240).


### Command Line

    zcc  +cpm -lgfxattache -create-app program.c




### Emulator hints

Please be aware that [MAME](http://www.mamedev.org/) is not yet working correctly and instabilities may happen.  Some program size limit could be still present also with the updated appmake tool.


#### ImageDisk and "Appmake +cpm"

We suggest to use the [ImageDisk](http://www.classiccmp.org/dunfield/img/index.htm).    The same file created with the described procedure should be valid for ImageDisk to transfer the disk image onto a real floppy.

To test the resulting program on the MAME emulator an [OS disk image](http://www.retroarchive.org/maslin/disks/otrona/oattache.td0) is also necessary.


We use the appmake's 'cpm option to create a raw disk image holding our program (e.g. A.COM):

    appmake +cpm -f ATTACHE -b a.com


The Otrona Attache' MAME driver is not able to read RAW disk images, so we encode it to IMD:

    bin2imd  a.raw a.imd /2 DM=5 N=40 SS=512 SM=1-10


The command line options to boot CP/M and use the created disk image will be something like:

    mame attache -flop1 oattache.td0 -flop2 A.IMD


After the CP/M boot, just enter 'b:' and the program name to run it !


The 'sq.com' utility present on another disk image (otr-util.td0) can be patched at 133120 to overwrite DU.COM and run correctly up to 4.5K long programs, but the emulator will need a system disk to boot:

    mame attache -flop1 oattache.td0 -flop2 A.IMD


