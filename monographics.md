# MONOCHROME GRAPHICS FUNCTIONS (graphics.h)

 | Header     | [{z88dk}/include/graphics.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/graphics.h)      |     
 | -------------------------------------------------------------------------------------------------------------------------     
 | Source     | [{z88dk}/libsrc/graphics](https///github.com/z88dk/z88dk/tree/master/libsrc/graphics/)                       |   
 | Include    | #include `<graphics.h>`                                                                                           |
 | Linking    | n/a                                                                                                             |
 | Compile    | n/a                                                                                                             |
 | Comments   | These library functions implement various monochrome graphics functions thought to be easily portable.          |



# Mono Graphics API

These APIs (along with the [monochrome sprites](library/sprites/monosprites) ones) have been developed for as many target platforms as possible to permit the portability of simple graphical applications.  For this reason no color extensions are implemented.
The developer wishing to create portable applications has to keep in mind the differences between the various screen resolutions.

The most common ones are:

256x192

256x64

128x64

96x64 (considering that some text-only target system can provide a pesudo-graphics resolution of 80x72 those could converge on a minimal of 78x64 to cover as much platform as possible).

A wider resolution is supported too but at the moment not all the functionalities are covered.


## void plot(int x, int y)

Plot a pixel to screen

## void unplot(int x, int y)

Reset a pixel on the screen



## void xorplot(int x, int y)

Invert a pixel on the screen

## int point(int x, int y)

Get the status of a pixel
(this function might not work correctly on all the platforms)

## int multipoint(int hv, int length, int x, int y)

Get horizontal or vertical pixel bar, up to 16 pixel long, for a quick pattern recognition in graphics applications or games.
(this function might not work correctly on all the platforms)

## void draw(int x1, int y1, int x2, int y2)

Draw a line, given absolute coordinates

## void xordraw(int x1, int y1, int x2, int y2)

Draw a line in XOR mode, given absolute coordinates

## void undraw(int x1, int y1, int x2, int y2)

Draw a white line (clear a line), given absolute coordinates
## void drawto(int px, int py)

Draw a line, starting from the last plotted pixel to the given position

## void xordrawto(int px, int py)

Draw a line in xor mode, starting from the last plotted pixel to the given position

## void undrato(int px, int py)

Draw a white line starting from the last plotted pixel to the given position

## void drawr(int px, int py)

Draw a line, starting from the last plotted pixel to the given relative position

## void xordrawr(int px, int py)

Draw a line in XOR mode, starting from the last plotted pixel to the given relative position

## void undrawr(int px, int py)

Draw a white line starting from the last plotted pixel to the given relative position

## void drawb(int tlx, int tly, int width, int height)

Draw a box border

## void undrawb(int tlx, int tly, int width, int height)

Draw a white box border

## void xorborder(int tlx, int tly, int width, int height)

Invert the given box border.  Useful for "select area" functions.

## void circle(int x, int y, int radius, int skip)

Draw a circle.

## void uncircle(int x, int y, int radius, int skip)

Draw a white circle.

## void clg(void)

Clear the screen and (if necessary for the platform) enter in graphics mode.

## void clga(int tlx, int tly, int width, int height)

Clear the given screen area.

## void fill(int x, int y)

Fill an area starting at the given point.
(this function might not work correctly on all the platforms, and uses a lot of stack space)

# Handling complex filled shapes

The "stencil" object is an evolution of a base concept introduced by Rafael de Oliveira Jannone in his gfx library: a convex area is defined by two byte vectors, pointing respectivelty to the leftmost and the rightmost X coordinates.
They're stuffed in a single byte vector, long twice the Y axis.

First of all the programmer must initialize the 'stencil', object, then various shapes can be added to form the resulting convex object (note that no 'holes' are allowed on top and lower borders).
As soon as the object is finished it can be drawn on the screen with the "stencil_render()" function.
The resulting shape covers the previous picture.


## void stencil_init(unsigned char *stencil)

Set/Reset the couple of vectors being part of a "stencil".


## void stencil_add_point(int x, int y, unsigned char *stencil)

Add a single dot to a figure defined inside a stencil object, init the vector pointers.


## void stencil_add_liner(int dx, int dy, unsigned char *stencil)

Trace a relative line into a stencil object (extend shape).


## void stencil_add_lineto(int x2, int y2, unsigned char *stencil)

Trace a line into a stencil object up to a given coordinate (extend shape).

## void stencil_add_side(int x1, int y1, int x2, int y2, unsigned char *stencil)

Add a side to a figure defined inside a stencil object


## void stencil_add_circle(int x, int y, int radius, int skip, unsigned char *stencil)

Add a circular shape to a figure defined inside a stencil object


## void stencil_stencil_render(unsigned char *stencil, unsigned char intensity)

Render an area delimited by a stencil object, with the specified dither intensity (0..11)

## int getmaxx()

Get the maximum X coordinate value for the current target.

## int getmaxy()

Get the maximum Y coordinate value for the current target.

# Monochrome raster pictures

The easiest way to import monochrome raster picture is to use the "sprite" object.
The 'putsprite' function has been ported to almost all the supported target and permits to keep portable the data itself.  The sprite editors let you import an external data file.

The putsprite function is documented in detail in the [monochrome sprites](library/sprites/monosprites) section.

# Monochrome vector pictures

'Graphic Profiles' are byte streams containing vector and surface descriptions.
Their components are defined in [`<gfxprofile.h>`](http://z88dk.cvs.sourceforge.net/z88dk/z88dk/include/gfxprofile.h?view=markup).

A data converter called 'z80svg' is provided.   It converts data from the SVG format to a 'profile' stream.
A programmer might prepare his own pictures by just drawing them with InkScape, or directly describing the paths in data structures.

## void draw_profile(int dx, int dy, int scale, unsigned char *metapic)

Render a picture defined in a 'profile' byte stream.

