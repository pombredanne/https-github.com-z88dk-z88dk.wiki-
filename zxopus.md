# Opus Discovery functions (zxopus.h)

 | Header     | [{z88dk}/include/zxopus.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/zxopus.h)    |             
 | -------------------------------------------------------------------------------------------------------------------             
 | Source     | [{z88dk}/libsrc/spectrum/opus](https://github.com/z88dk/z88dk/tree/master/libsrc/spectrum/opus)                   |
 | Include    | #include `<zxopus.h>`             |                                                                                  
 | Linking    | n/a                          |                                                                                     
 | Compile    | n/a                          |                                                                                     
 | Comments   |                              |                                                                                     

This library permits low-level operations on the Opus Discovery interface.



# Joystick

## set_kempston (int mode)

Sets the kempston emulation (1=on, 0=off)

## int get_kempston ()

Gets the kempston emulation status (1=on, 0=off)


# Disk

##  int opus_getblocks (int drive)

Gets the number of sectors

##  int opus_getblocksize (int drive)

Gets the sector size


# Parallel Port

## opus_lptwrite (unsigned char databyte)

Sends a character to parallel port

## unsigned char opus_lptread ()

Reads a character from the parallel port

