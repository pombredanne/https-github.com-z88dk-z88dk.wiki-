#  OSBORNE 1

The OSBORNE computers are CP/M compatible, so the [same base library](Platform---CPM) can be used, but extra functionalities are available.
Library extras include the graphics support for 104x48 low resolution graphics.


### Command Line

    zcc  +cpm -lgfxosborne1 -create-app program.c


### Emulator hints

Hardware emulation for 'osborne1' [MAME](http://www.mamedev.org/) works well, see below for a possible way to transfer your programs on a disk image and successfully boot.


#### ImageDisk and CPMTOOLS

We suggest to use the [ImageDisk](http://www.classiccmp.org/dunfield/img/index.htm) and edit the popular [MS BASIC disk image](http://www.retroarchive.org/maslin/disks/osborne/os1basic.td0).

[CPMTOOLS](http://www.moria.de/~michael/cpmtools/) will be also necessary to alter the disk image.   An entry for the Osborne disks should already be present in the "diskdefs", but it is advised to verify the parameters or to add an extra entry:

    # BEGIN osb1sssd  Osborne 1 - SSSD 48 tpi 5.25" - 256 x 10
    # Test OK - image size = 102,400, from Don Maslin's archive
    diskdef osb1sssd
      seclen 256
      tracks 40
      sectrk 10
      blocksize 2048
      maxdir 64
      skew 2
      boottrk 3
      os 2.2


First of all use TD02IMD to convert from the TeleDisk to the ImageDisk format:

    TD02IMD OS1BASIC.TD0


Then, convert the IMD disk image to an editable RAW file (OS1BASIC.RAW):

    /B OS1BASIC OS1BASIC.RAW


Now, supposing you built a program named 'A.COM', replace the auto-starting program with it on the RAW disk image:

    cpmrm -f osb1sssd  OS1BASIC.RAW 0:autost.com
    cpmcp -f osb1sssd  OS1BASIC.RAW a.com 0:autost.com
    

MAME is not able to read RAW disk images, so we encode it back to IMD:

    bin2imd  OS1BASIC.RAW a.imd /1 DM=2 N=40 SS=256 SM=1-10


The command line options to boot CP/M and use the created disk image is something like:

    mame osborne1 -flop1 A.IMD


### Screenshots

