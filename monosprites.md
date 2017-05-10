# MONOCHROME SPRITES FUNCTIONS (part of games.h)

 | Header     | [{z88dk}/include/games.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/games.h)      |           
 | -------------------------------------------------------------------------------------------------------------------           
 | Source     | [{z88dk}/libsrc/graphics](https///github.com/z88dk/z88dk/tree/master/libsrc/graphics/)                       |   
 | Include    | #include `<games.h>`                                                                                           |   
 | Linking    | n/a                                                                                                             |
 | Compile    | n/a                                                                                                             |
 | Comments   |           |                                                                                                      


These library functions are part of the monochrome graphics functions, thought to be easily portable.





# Sprite functions

The sprite functions are based on a special data structure permitting dynamic sprite sizes: the first two bytes define the sprite size, while the next ones describe the picture. 

The bits are right-padded to fill the last byte in a row.



## getsprite(int x, int y, void *sprite)

Pick up a sprite directly from the screen.

This functions isn't still working.


## putsprite(int ortype, int x, int y, void *sprite)

Draw a sprite of variable size it can behave in three different ways, to be specified in the "ortype" parameter:

### SPR_OR

the sprite will just be printed on the screen without deleting what's already there

### SPR_XOR

the sprite will be XORed with the pictures already present on the screen

### SPR_AND

the sprite will be "printed in white", eventually leaving a shape.  This feature permits the use of "sprite masks".

# Screen areas save and restore



## void bksave(int x, int y, void *sprite)

Saves the screen area in a sprite-like structure. Position and size are saved in it (taking 4 bytes), but the area granularity works at byte-level, to run faster.

Example:

```

char  *background;

background=malloc( 4 + (width/8+1) * (height) );
sprintf(background, "%c%c",width,height);
bksave(x,y,back);

```

## void bkrestore(void *sprite)

Restores the background area.

Example:

```

bkrestore(background);

```


# Functions in graphics.h

## int multipoint(int hv, int length, int x, int y)

Get horizontal or vertical pixel bar, up to 16 pixel long, for a quick pattern recognition in graphics applications or games. (this function might not work correctly on all the target platforms) 
Note that point() and multipoint() are declared in "graphics.h", but might be useful in games too.


# The Sprite editor tool by Daniel McKinnon

This is a simple sprite editor for z88dk with it's main feature being that it automatically will generate code for you.

Its features make it very useful to create animated sprite arrays and to import external pictures into sprite structures.

## How to use

The new Sprite Editor version is a little more handy:

A key reference sheet can be displayed in any moment by keeping F1 pressed.

The "import picture" function ('L' key) is very powerful, try it with your favourite picture format; when a 'SNA' file is being opened the editor tries to extract the Character set and UDG maps.

{{:library:sprites:spedit-sheet.jpg|}}

The brown "buttons" on the bottom bar can be repeatedly clicked by dragging the mouse.

{{:library:sprites:spedit-bar.jpg|}}


# Links to external tools

[SpEdito - Marcello Zaniboni's Sprite Editor](http://www.marcellozaniboni.net/spedito/index.html)

[SevenuP - Jaime Tejedor Gómez's Sprite Editor](http://metalbrain.speccy.org/)


