# Turbo C BGI emulation macros (bgi.h)

 | Header     | [{z88dk}/include/bgi.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/bgi.h)    |             
 | -------------------------------------------------------------------------------------------------------------             
 | Source     | [{z88dk}/libsrc/graphics/x11](https///github.com/z88dk/z88dk/tree/master/libsrc/graphics/x11) (part of)     |
 | | [{z88dk}/libsrc/graphics/lib3d](https///github.com/z88dk/z88dk/tree/master/libsrc/lib3d) (part of)   |                  
 | Include    | #include `<bgi.h>`                             |                                                               
 | Linking    | -lx11 -llib3d                                |                                                               
 | Compile    | -DGFXSCALEX=2/5 (256x192)                    |                                                               
 | | -DGFXSCALEX=4/5 -DGFXSCALEY=2/5 (512x192)    |                                                                          
 | Comments   |                                              |                                                               


Minimal implementation of the Turbo C BGI graphics functions plus few more Borland C peculiarities.



