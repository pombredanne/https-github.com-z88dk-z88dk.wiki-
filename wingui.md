# GUI (gui.h)

 | Header     | [{z88dk}/include/gui.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/gui.h)    |                       
 | -------------------------------------------------------------------------------------------------------------                       
 | Source     | [{z88dk}/libsrc/graphics/x11](https://github.com/z88dk/z88dk/tree/master/libsrc/graphics/x11) (part of)               |
 | Include    | #include `<gui.h>`             |                                                                                         
 | Linking    | -lx11                        |                                                                                         
 | Compile    | n/a                          |                                                                                         
 | Comments   |                              |                                                                                         

Very basic but portable windowing functions, based on the monochrome graphics library.

It is now part of the X11 emulation library.


# Windowing API

## gui_win structure

```
struct gui_win {
        char    *back;
        char    x;
        char    y;
        char    width;
        char    height;
        char    flags;
};
```

The gui_win structure includes a pointer to a sprite-like formatted memory area, the x and y coordinates and its size.
The flags describe the graphical appearance and can combine the following features:

    WIN_BORDER
    WIN_LITE_BORDER
    WIN_SHADOW

There is a further possibility: defining the "ALTGUI" flag at compile time the "round corners" mode is activated.





## win_open(struct gui_win win)

Allocates a memory space for storing the background area (see malloc) and paints a clean box.

Example:
```
// #include "malloc.h"  <--  This should be defined, in a real case
#include "gui.h"

main()
{
    struct gui_win win;

    win->x = 30; win->y = 15;
    win->flags = WIN_BORDER|WIN_SHADOW;
    win->height = 20; win->width = 40;
    win_open (win);
}

```


## win_close(struct gui_win win)

Recovers the background area and frees the relative memory area.

Example:

```

    win_close (win);

```
