The classic library supports multiple target machines and each target may have its own idiosyncratic method of controlling print position and other limitations such as reserving areas of the screen for status lines etc

To simplify cross-platform porting, classic provides a VT100/ANSI emulation library that understands standard control sequences that will also work on Linux, Mac, or even Windows consoles. For the highest degree of portability using the ANSI driver is recommended.

In general, the ANSI driver will prefer using a graphics mode which may well be slow, additionally the ANSI driver is a large chunk of code, adding into the project may well increase the size of your project by around 2-3K.

As a smaller (around 300 bytes) alternative, the classic library provides a generic console driver which allows classic applications to use the same control codes across targets without the overhead of the ANSI library.

For certain platforms where native print positioning is not trivially available (for example trs80, Sprinter, Nascom, Z1013) , it is enabled by default providing richer functionality than was previously available.

The driver is based on the implementation for the ZX Spectrum, and hence has the control codes detailed in the next section.

##  Control codes

The generic console supports the following control codes:

```
4,x       - Disable (0) or enable (1) vertical scrolling

8,9,11    - Move in x and y as you would expect
12        - Form feed - clears the screen and moves print posn to 0,0
10        - Carriage return - advances y and sets x to 0
13        - Moves down a row
22,y,x    - Move to position y,x on the screen (0<=y<=23, 0<=x<=63)
            NB. y and x are displaced by 32 eg to move the print position
            to (0,0) use 22,32,32.
```
