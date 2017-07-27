# Gray Library (gray.h)



|^ Header     | [[https://raw.githubusercontent.com/z88dk/z88dk/master/include/gray.h|{z88dk}/include/gray.h]]      |
|^ Source     | [[https://github.com/z88dk/z88dk/tree/master/libsrc/graphics/gray/|{z88dk}/libsrc/graphics/gray]]   |
|^ Include    | #include <gray.h>                                                                                   |
|^ Linking    | n/a                                                                                                 |
|^ Compile    | n/a                                                                                                 |
|^ Supported  | [[platform:zx81|ZX81]], [[platform:ticalc|TI calculators]], [[platform:amstradcpc|Amstrad CPC]]     |
|^ Comments   | n/a                                                                                                 |


The gray library is based on advanced IRQ handlers which quickly swap two display memory pages, thus it is subject to many limitations:

    * In many cases the screen flickers (expecially on the ZX81)
    * Keeping two pictures costs a lot of memory 
    * It heavily slows down the CPU and makes it work continously, thus disabling the "power saving" tricks and sucking up batteries on the TI calculators
    * Accessing to two display pages slows down the applications comparing to the "default" B/W graphics.

# Constants

# Gray Levels

| G_BLACK | 0 |
| G_DARK  | 1 |
| G_GRAY  | 2 |
| G_LIGHT | 2 |
| G_WHITE | 3 |

# Functions


## g_page(int page)

Select actual gray page for the standard gfx funtions


## g_plot(int x, int y, int GrayLevel)

Plot a pixel on screen


## g_point(int x, int y)

Get pixel status


## g_draw(int x1, int y1, int x2, int y2, int GrayLevel)

Draw a line


## g_drawr(int px, int py, int GrayLevel)

Relative draw 


## g_drawb(int tlx, int tly, int width, int height, int GrayLevel)

Draw a box


## g_circle(int x, int y, int radius, int skip, int GrayLevel)

Draw a circle


## g_clg(int GrayLevel)

Clear map


# Notes

Recent changes integrate che gray functions in the core library file, so the following files will soon disappear:

    cpcgray.lib
    tigray82.lib
    tigray83.lib
    tigray83p.lib
    tigray85.lib
    tigray86.lib

