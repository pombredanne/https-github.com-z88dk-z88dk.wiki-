======= NASCOM 1 & 2  hints & tools =======

{{platform:nascom3.jpg|}}

The code should work with both NASSYS and T MONITORs, and is located by default at $1000.


# Quick start

    zcc +nascom -lm -o adventure -create-app adv_a.c
-or-
    zcc +nascom -clib=ansi -lm -o adventure -create-app adv_a.c

This command will build a file called "adventure.nas", an HEX formatted binary file suitable to be run on the existing emulators.

It is possible to choose a full VT/ANSI emulation or a tiny console support.
The ANSI version of the library depends a bit less on the ROM because the video memory is addressed directly.

You can also change the program location with the "-zorg=" option.  The default position is at address $1000.



# Virtual Nascom

Download and build from http://repo.or.cz/w/virtual-nascom.git

To run

```
./virtualnascom adventure.nas
```

Once the monitor is running, enter "E 1000"


# VNASCOM Emulator hint


To run a compiled program:

```
vnascom ROM=nassys1.nas RAM=adventure.nas KBD=E1000
```



# NASCOM related WEB Links

http://www.nascomhomepage.com

http://www.myplace.nu/nascom
