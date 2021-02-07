
 |            |                                 |
 | ---------- | ------------------------------- |                                                 
 | Header     | [{z88dk}/libsrc/sprites/software/sp1/spectrum/spectrum-sp1.h](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/sprites/software/sp1/spectrum/spectrum-sp1.h)  |                                                   
 | Source     | [{z88dk}/libsrc/sprites/software/sp1](https://github.com/z88dk/z88dk/tree/master/libsrc/sprites/software/sp1)  |                                                                                                             
 | Include    | #include `<sprites/sp1.h>`                                                                                     |                                                                                                    
 | Linking    | -lsp1                                                                                                          |                                                                                                            
 | Compile    | make sp1 from {z88dk}/libsrc for instructions                                                                  |                                                                                                              
 | Supported  | [ts2068 512x192 mode](platform/ts2068), [zx spectrum](platform/zx), [zx81 true hi-res](platform/zx81)          |                                                                                                        
 | Comments   | **1.** The location of SP1's variables and the display size to manage are specified by editing a customization file prior to library compilation.  |                                                                         
 | | **2.** Library compilation places sp1.lib into the C libs directory and sp1.h into the include directory.  Only one version of the SP1 library can exist at a time.  |                                                                  
 | | **3.** The SP1 library implicitly allocates memory through functions the user program must supply.  See [library memory allocation](library/memory_allocation#memory_allocation_implicitly_performed_by_z88dk_libraries) for details.  |


## Overview

SP1 is a software sprite library that was designed to minimize the amount of screen area redrawn between screen refreshes.  It does this by dividing the entire screen into character cells, typically 8x8 pixels in size, though this varies from platform to platform, and by maintaining a dirtied character cell list.  Each such character cell is described by a *struct sp1_update* which contains information on the background graphics for the cell and all sprites that occupy the cell.  As sprites are moved around and background is modified, the affected *struct sp1_update*s are added to the dirtied character cell list automatically.  Each *struct sp1_update* can only be added to the list exactly once so that each character cell only gets drawn once even if the character cell is modified several times by, for example, multiple sprites passing thrugh the cell or by multiple changes to a cell's background graphics.  Using the library to change background graphics or move sprites only causes the affected *struct sp1_update*s to be added to the dirtied character cell list; no actual screen changes occur until the *sp1_UpdateNow()* function is called, which iterates through the dirtied character cell list drawing each character cell to screen.

SP1 understands two graphical entities: background tiles and sprites.  Background tiles can either be a single byte character code (for example the letter 'A') or an absolute memory address from which background graphics for that cell are copied.  Single byte character codes allow string printing and quick background animation effects; the graphics associated with each such character code is redefinable and is initialized to point into the platform's native character set where possible.  Using an absolute memory address to specify background graphics allows a secondary display file to be set up as background.  Background of specific character cells can be freely specified as character code or absolute address at any time.

SP1 also supports sprites.  Sprites are graphical objects that float over the background and amongst themselves.  Any number of any size sprites are supported, with performance suffering as the number and size of moving sprites on screen increases (recall that SP1 uses a differential update algorithm, meaning only areas of the screen that change, perhaps because of moving sprites, are redrawn; static sprites do not get redrawn).  Sprites exist on one of 256 planes with lower planes closest to the viewer.  A sprite on plane 255 would lie just above the background while a sprite on plane 0 would lie above all other sprites and the background.  The draw order (which sprite is on top of which) is indeterminate if two overlapping sprites lie in the same plane.

There are several main varieties of sprites such as mask, or, xor and load, that determine how sprite graphics are mixed with the background and other underlying sprites.  Within each of these main varieties there are many more subtypes that further refine how background mixing is done and how quickly it is done.  SP1 also comes with an optional built-in software rotater that can quicky shift sprite graphics to get pixel-resolution placement of sprite graphics.  It is also possible to add user-defined sprite draw functions.

Aside from these mainstream features, the SP1 engine also has some unique features:


*  Flicker free operation without synchronizing with the raster.

*  Sprites and text are clipped to arbitrary rectangular areas aligned to character cells on screen.

*  Background and sprite graphics can be easily animated.

*  Individual character cells can be removed from the engine so that they are never drawn by SP1.  This can give the appearance of sprites moving behind background elements, for example.

*  Individual character cells can be mapped to any location in the display at runtime.

*  Sprites can be occluding, meaning anything underneath them is not drawn.  This is a speed-up technique.

*  The individual characters making up a sprite can be a different draw type allowing, for example, characters along the sprite's outline to be made the slower mask type and interior character to be made the fast load type.

*  The screen area managed by SP1 is configurable; if there is insufficient RAM simply reduce the size of screen handled by the engine.

Because of its differential update nature (SP1 only redraws areas of the screen that change), SP1 is not designed for scrolling applications.  It is possible to scroll by character amounts, something that may be suitable for board-type games.


## Applications

SP1's predecessor was used to make the following ZX Spectrum programs:

 | [ Cannon Bubble](http://cezgs.computeremuzone.com/eng/card.php?id=14 ) | [ Minesweeper](http://www.geocities.com/aralbrec/spritepack/examples/minesweeper.zip ) |
 | ---------------------------------------------------------------------- | ----------------------------------------------------------------------------------------
 | {{library:sprites:cannon_bubble.gif}}                                  | {{library:sprites:minesweeper.gif}}                                                      | 
 | [ Moggy](http://cezgs.computeremuzone.com/eng/card.php?id=7 )          | [ Phantomas Infinity](http://cezgs.computeremuzone.com/eng/card.php?id=10 ) |           
 | {{library:sprites:moggy.gif}}                                          | {{library:sprites:infinity.gif}}                                                         | 

##  Portability 

SP1 is currently available for the [ ZX Spectrum](platform/zx ) and is being ported to the Amstrad CPC, MSX1, VZ-200 and Timex/SE computers.  If you would like to help with these ports or a port to any other platform, please introduce yourself on the z88dk mailing list.

## Tutorial & Documentation


*  Under the Hood
    * An understanding of how the SP1 engine works will make it much easier to understand how to use it well
    * The SP1 memory map
    * Customizing and compiling the library

*  The SP1 API
    * An overview of the function groups in the SP1 API
    * A description of the data structures and constants found in the header file
    * How SP1 performs dynamic memory allocation implicitly when creating sprites
    * Documented SP1 API

*  Example Programs

*  Advanced
    * Creating custome sprite types and draw code
    * Mirrors using occluding sprites without background clear
    * Shimmer effects and reflections using mirrors and custom sprite draw code
    * Lighting effects using attribute sprites
    * Non-rectangular sprites
    * Statically creating sprites to avoid code baggage of sprite creation functions


## External links


*  [splib2-chat-with-aa-2005-09.pdf: notes from the MojonTwins website](http://www.mojontwins.com/warehouse/splib2-chat-with-aa-2005-09.pdf)

*  [splib2-tutorial.pdf from the MojonTwins website](http://www.mojontwins.com/warehouse/splib2-tutorial.pdf)

(splib2 is an older version of the library but the underlying algorithms are the same.  Some of the issues touched on in the above tutorials have been more simply solved in sp1)


