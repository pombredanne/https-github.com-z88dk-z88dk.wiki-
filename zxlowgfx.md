# ZX Spectrum Low Resolution Colour Graphics (zxlowgfx.h)

 | Header     | [{z88dk}/include/zxlowgfx.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/zxlowgfx.h)    |               
 | -----------------------------------------------------------------------------------------------------------------------               
 | Source     | [{z88dk/include/zxlowgfx.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include//zxlowgfx.h)                   |
 | Include    | #include `<zxlowgfx.h>`             |                                                                                      
 | Linking    | n/a                          |                                                                                           
 | Compile    | n/a                          |                                                                                           
 | Comments   | Assembly source embedded into the header file   |                                                                        

This library features a very low resolution, but permits to get full color support and very fast paging.

It is able to support picture buffering and can be pre-set to run in 32x48 or 64x24 pixels resolution mode.

See also the [mandelbrot](examples/snippets/zxspectrum/lr_mandel) and the [rotating cubes](examples/snippets/zxspectrum/lr_rotate) examples.


# Initialization

The defalut screen resolution is 32x48, but it is possible to activate the 64x24 one by defining the "ALTLOWGFX" variable.



## void cclg(int color)

Initializes the graphics screen with the appropriate hidden pattern and sets the background color.


# Control


## cplot(int x, int y, int color)

Plot a pixel to screen


## int cpoint(int x, int y)

Get the current color of a given pixel


## cdraw(int x1, int y1, int x2, int y2, int color)

Draw a line at the given coordinates with the given color


## cdrawr(int x, int y, int color)

Draw a line to the given coordinates with the given color starting from the last plotted point.


## cputsprite(int x, int y, int color, void *sprite)

Put a monochrome sprite at the given position, painted with the given color.




# Buffered graphics

Buffering is an optional feature; it can be enabled with the "bufferedgfx" environment variable.
 When active, the picture being drawn won't be visible till the next "ccopybuffer" call.

## cclgbuffer(int color)

If it exists, clear the color graphics buffer.


## void ccopybuffer(void)

If it exists, copy the color graphics buffer to the visible screen.


