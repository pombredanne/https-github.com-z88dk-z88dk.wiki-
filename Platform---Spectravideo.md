# Spectravideo SVI

The Spectravideo SVI-318 and SVI-328 are now well supported.
Most of the generic functions (monochrome graphics, 1 bit sound, stdio, etc) are supported as well as most of the [MSX](platform/msx) specific ones.



#### Quick start

    zcc  +svi -lm -create-app program.c

This will create a .CAS file.
Once compiled, run the SVI emulator and load the "tape" with the /r option.

If you are using the Jimmy Mårdell's ["SVI" emulator](http://www.acc.umu.se/~yarin/sviemu/), you can specify the autoload option:
    svi -tapa adv.cas

Other emulators, in example BlueMSX need the full BASIC command to be typed in:
    bload "cas:",r
 
A WAV audio file can be created too, with the following option:

    zcc  +svi -lm -create-app -subtype=wav program.c


#### 16K model
To run on a 16K machine, you need to move the code origin to the upper half.
'-zorg=49200' should be a good option for you zcc command line.  Any working address for the 16k model will still work on the 32k models.
