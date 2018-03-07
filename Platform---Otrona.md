#  Otrona Attachè

The Otrona Attachè computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.
Library extras include the graphics support (320x240).


### Command Line

    zcc  +cpm -lgfxattache -create-app program.c




### Emulator hints

Please be aware that [MAME](http://www.mamedev.org/) is not yet working correctly and several instabilities may happen. 


#### ImageDisk and "Appmake +Inject"

We suggest to use the [ImageDisk](http://www.classiccmp.org/dunfield/img/index.htm) and edit an existing [400K disk image](http://www.retroarchive.org/maslin/disks/otrona/oattache.td0).


First of all use TD02IMD to convert from the TeleDisk to the ImageDisk format:

    TD02IMD OATTACHE.TD0


Then, convert the IMD disk image to an editable RAW file (OATTACHE.BIN):

    imdu /B  OATTACHE OATTACHE.BIN


We weren't able to find a valid disk definition for CPMTOOLS, so we use a trick.
With an hex editor we spotted the MBASIC file position on the raw disk image, (knowing that the first bytes are c3,82,5f,1c,28,7b).

Then we use the appmake's INJECT option to create a copy of the disk image with our program pushed in the proper position:

    appmake +inject -b OATTACHE.BIN -o A.RAW -i a.com -s 201728


MAME is not able to read RAW disk images, so we encode it back to IMD:

    bin2imd  a.raw a.imd /2 DM=5 N=40 SS=512 SM=1-10


The command line options to boot CP/M and use the created disk image will be something like:

    mame attache -flop1 A.IMD


After the CP/M boot, just type 'mbasic' to run the program !

