# Classic Graphics `<graphics.h>`

The graphics API provides a portable abstraction for drawing graphics primitives onto the displays of target platforms.

Classic graphics supports targets with screen sizes ranging from 64x64 to 1024x512 and as such when writing code that is meant to run on many targets, it is important that these difference are held in mind.

Traditionally, the graphics subsystem has been monochrome, and as such all functions do not take a colour parameter. However, on many targets the colour of a plotted pixel can be chosen by calling the `textcolor()` function with the appropriate colour defined in `<conio.h>`. This is a result of many targets supporting graphics based on hardware semi-graphics modes.

.

## Plotting primitives `<graphics.h>`

The following functions plot/reset pixels at the native resolution of the screen mode. (0,0) is the top left corner of the screen.

### `void plot(int x, int y)`

Plot a pixel to screen

### `void unplot(int x, int y)`

Reset a pixel on the screen

### `void xorplot(int x, int y)`

Invert a pixel on the screen

### `int point(int x, int y)`

Get the status of a pixel

## "Chunky" plotting primitives

The following functions plot/reset a pixel that is equivalent to a 4x4 pixel. These directives are perfect for emulating the lores graphics available on computers such as the ZX81 on machines that only have hires graphics.

### `void c_plot(int x, int y)`

Plot a 4x4 pixel to screen

### `void c_unplot(int x, int y)`

Reset a 4x4 pixel on the screen

### `void c_xorplot(int x, int y)`

Invert a 4x4 pixel on the screen

### `int c_point(int x, int y)`

Get the status of a 4x4 pixel

## Miscellaneous functions

### `int getmaxx()`

Get the maximum X coordinate value for the current target. On targets with multiple screen modes this returns the value for the currently selected mode.

### `int getmaxy()`

Get the maximum Y coordinate value for the current target. On targets with multiple screen modes this returns the value for the currently selected mode.

### `int multipoint(int hv, int length, int x, int y)`

*This function might not work correctly on all the platforms*

Get horizontal or vertical pixel bar, up to 16 pixel long, for a quick pattern recognition in graphics applications or games.


## Line drawing

### `void draw(int x1, int y1, int x2, int y2)`

Draw a line, given absolute coordinates

### `void xordraw(int x1, int y1, int x2, int y2)`

Draw a line in XOR mode, given absolute coordinates

### `void undraw(int x1, int y1, int x2, int y2)`

Draw a white line (clear a line), given absolute coordinates
### `void drawto(int px, int py)`

Draw a line, starting from the last plotted pixel to the given position

### `void xordrawto(int px, int py)`

Draw a line in xor mode, starting from the last plotted pixel to the given position

### `void undrawto(int px, int py)`

Draw a white line starting from the last plotted pixel to the given position

### `void drawr(int px, int py)`

Draw a line, starting from the last plotted pixel to the given relative position

### `void xordrawr(int px, int py)`

Draw a line in XOR mode, starting from the last plotted pixel to the given relative position

### `void undrawr(int px, int py)`

Draw a white line starting from the last plotted pixel to the given relative position

## Shape drawing

### void drawb(int tlx, int tly, int width, int height)

Draw a box border

### `void undrawb(int tlx, int tly, int width, int height)`

Draw a white box border

### `void xorborder(int tlx, int tly, int width, int height)`

Invert the given box border.  Useful for "select area" functions.

### `void circle(int x, int y, int radius, int skip)`

Draw a circle.

### `void uncircle(int x, int y, int radius, int skip)`

Draw a white circle.

## Clearing and filling

### `void clg(void)`

Clear the screen and (if necessary for the platform) enter in graphics mode.

### `void clga(int tlx, int tly, int width, int height)`

Clear the given screen area.

 
### `void xorclga(int tlx, int tly, int width, int height)`

Invert the given screen area.

### `void fill(int x, int y)`

*This function may not work correctly on all platforms since it uses a lot of stack space*

*This functions is not available for 8080 targets*

Fill an area starting at the given point.

# Handling complex filled shapes

*This feature is not available for 8080 targets*

The "stencil" object is an evolution of a base concept introduced by Rafael de Oliveira Jannone in his gfx library: a convex area is defined by two byte vectors, pointing respectivelty to the leftmost and the rightmost X coordinates. They're stuffed in a single byte vector, long twice the Y axis.

First of all the programmer must initialize the 'stencil', object, then various shapes can be added to form the resulting convex object (note that no 'holes' are allowed on top and lower borders).
As soon as the object is finished it can be drawn on the screen with the "stencil_render()" function.
The resulting shape covers the previous picture.


### `void stencil_init(unsigned char *stencil)`

Set/Reset the couple of vectors being part of a "stencil".


### `void stencil_add_point(int x, int y, unsigned char *stencil)`

Add a single dot to a figure defined inside a stencil object, init the vector pointers.


### `void stencil_add_liner(int dx, int dy, unsigned char *stencil)`

Trace a relative line into a stencil object (extend shape).


### `void stencil_add_lineto(int x2, int y2, unsigned char *stencil)`

Trace a line into a stencil object up to a given coordinate (extend shape).

### `void stencil_add_side(int x1, int y1, int x2, int y2, unsigned char *stencil)`

Add a side to a figure defined inside a stencil object


### `void stencil_add_circle(int x, int y, int radius, int skip, unsigned char *stencil)`

Add a circular shape to a figure defined inside a stencil object


### `void stencil_stencil_render(unsigned char *stencil, unsigned char intensity)`

Render an area delimited by a stencil object, with the specified dither intensity (0..11)



# Monochrome raster pictures

The easiest way to import monochrome raster picture is to use the "sprite" object.
The `putsprite()` function has been ported to almost all the supported target and permits to keep portable the data itself.  The sprite editors let you import an external data file.

# Sprite functions `<games.h>`

The sprite functions are based on a special data structure permitting dynamic sprite sizes: the first two bytes define the sprite size, while the next ones describe the picture.

The bits are right-padded to fill the last byte in a row.

## `void putsprite(int ortype, int x, int y, void *sprite)`

Draw a sprite of variable size it can behave in three different ways, to be specified in the "ortype" parameter:

* `SPR_OR` the sprite will just be printed on the screen without deleting what's already there

* `SPR_XOR` the sprite will be XORed with the pictures already present on the screen

* `SPR_AND` the sprite will be "printed in white", eventually leaving a shape.  This feature permits the use of "sprite masks".

  

# Screen areas save and restore `<games.h>`

*Not available on 8080 targets.*

## `void bksave(int x, int y, void *sprite)`

Saves the screen area in a sprite-like structure. Position and size are saved in it (taking 4 bytes), but the area granularity works at byte-level, to run faster.:

```c
char  *background;

background=malloc( 4 + (width/8+1) * (height) );
sprintf(background, "%c%c",width,height);
bksave(x,y,back);
```

## `void bkrestore(void *sprite)`

Restores the background area.

```c
bkrestore(background);
```



# Monochrome vector pictures `<gfxprofile.h>`

*Not available on 8080 targets*

'Graphic Profiles' are byte streams containing vector and surface descriptions.

A data converter called `z80svg` is provided. It converts data from the SVG format to a 'profile' stream.
A programmer might prepare his own pictures by just drawing them with InkScape, or directly describing the paths in data structures.

## `void draw_profile(int dx, int dy, int scale, unsigned char *metapic)`

Render a picture defined in a 'profile' byte stream.



# Links for external tools

z88dk provides a monochrome sprite editor in source code format, in the {z88dk}/support/sprites folder.   It requires the 'Allegro 5' library, its graphics interface is not elegant but it is very powerful and portable.

External alternatives are:

[SpEdito - Marcello Zaniboni's Sprite Editor](http://www.marcellozaniboni.net/spedito/index.html)

[SevenuP - Jaime Tejedor Gómez's Sprite Editor](http://metalbrain.speccy.org/)
