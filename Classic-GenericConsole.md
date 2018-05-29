The classic library supports multiple target machines and each target may have its own idiosyncratic method of controlling print position and other limitations such as reserving areas of the screen for status lines etc

To simplify cross-platform porting, classic provides a ANSI/VT100 emulation library that understands standard control sequences that will also work on Linux, Mac, or even Windows consoles. For the highest degree of portability using the ANSI driver is recommended.

In general, the ANSI driver will prefer using a graphics mode which may well be slow, additionally the ANSI/VT100 driver is a large chunk of code, adding into the project may well increase the size of your project by around 2-3K.

As a smaller (around 400 bytes) alternative, the classic library provides a generic console driver which allows classic applications to use the same control codes across targets without the overhead of the ANSI/VT100 library.

For certain platforms where native print positioning is not trivially available (for example trs80, Sprinter, Nascom, Z1013), it is enabled by default providing richer functionality than was previously available.

The driver is based on the implementation for the ZX Spectrum, and hence has the control codes detailed in the next section.

##  Control codes

The generic console supports the following control codes:

```
4,x       - Disable (0) or enable (1) vertical scrolling

8,9,11    - Move in x and y as you would expect
12        - Form feed - clears the screen and moves print posn to 0,0
10        - Carriage return - advances y and sets x to 0
13        - Moves down a row
16,n      - Set the ink colour (*)
17,n      - Set the paper colour (*)
20,n      - Enable/disable inverse video (*)
22,y,x    - Move to position y,x on the screen (0<=y<=23, 0<=x<=63)
            NB. y and x are displaced by 32 eg to move the print position
            to (0,0) use 22,32,32.

The parameter for those marked with (*) is taken as a bitwise and of the lower 4 bits. Typically these are offset to [0-9] for the lower values.
```

## VT52 control codes

The generic console additional supports VT52 control codes, the following are supported:

```
[ESC] A - Move the cursor to beginning of line above.
[ESC] B - Move the cursor to beginning of line below.
[ESC] C - Move the cursor right by one.
[ESC] D - Move the cursor left by one
[ESC] E - Clear the screen and place the cursor in the upper left corner.
[ESC] H - Move the cursor to the upper left corner.
[ESC] I - Move the cursor to beginning of line above.
[ESC] J - Erase all lines after our current line
[ESC] K - Clear the current line from the current cursor position.
[ESC] Y - row col 'Goto' Coordinate mode - first will change line number, then cursor position (both ASCII - 32)
[ESC] b - Byte after 'b' sets new foreground color (ASCII - 32)
[ESC] c - Byte after 'c' sets new background color (ASCII -32)
[ESC] p - start inverse video
[ESC] q - stop inverse video
[ESC] s - Enable/disable vertical scrolling
```

Both sets of control codes are active, if memory is extremely tight then one or the other set can be disabled and the library rebuilt - this will save around 40-60 bytes

## Colour mapping

By default, an attempt is made to map any colours to a standard range, this can be disabled (and the targets native colours will be used) with the compile time option `-pragma-define:CLIB_CONIO_NATIVE_COLOUR=1`

## Defining a custom font

On those targets that support either bitmapped graphics or redefining the character set, a custom font can be specified in one of two ways.

At compile time the option `-pragma-redirect:CRT_FONT=...` can be used, the available fonts are shown below:

![](images/platform/fonts/font1.png)
![](images/platform/fonts/font2.png)
![](images/platform/fonts/font3.png)

Alternatively, this can be done at compile time using `console_ioctl(IOCTL_GENCON_SET_FONT32, &addr)` where addr is a font reference from `<font/font.h>

## Defining UDGs

Characters > 128 are treated as UDGs on platforms that support them. The number available depends on the platform, but at present is either 128 or 32 (Jupiter Ace, Exidy). The address for these can be set programmatically using: `console_ioctl(IOCTL_GENCON_SET_UDGS, &addr)`

## Setting the screen mode

Some of the platforms support alternate screen modes, these can be switched to using: `console_ioctl(IOCTL_GENCON_SET_MODE, &mode)`. At present this functionality is available to the CPC (Modes 0,1,2) and MC-1000 (Mode 0 = text, Mode 1 = graphics)