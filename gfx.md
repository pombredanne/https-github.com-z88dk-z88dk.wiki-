# MSX GFX library


 | Version    | all                                                                                                             |                                                                                                                                                                                                                                                                                                                       
 | ------------------------------------------------------------------------------------------------------------------------------                                                                                                                                                                                                                                                                                                                       
 | Header     | [{z88dk}/include/msx/gfx.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/msx/gfx.h), [{z88dk}/include/msx/line.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/msx/line.h), [{z88dk}/include/msx/3d.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/msx/3d.h), [{z88dk}/include/msx/defs.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/msx/defs.h)                   |
 | Source     | [{z88dk}/libsrc/msx](https///github.com/z88dk/z88dk/tree/master/libsrc/msx/)                                 |                                                                                                                                                                                                                                                                                                                          
 | Include    | #include `<msx/gfx.h>`                                                                                                |                                                                                                                                                                                                                                                                                                                   
 | Linking    | n/a                                                                                                            |                                                                                                                                                                                                                                                                                                                        
 | Compile    | n/a                                                                                                             |                                                                                                                                                                                                                                                                                                                       
 | Supported  | [MSX](platform/msx), [Spectravideo](platform/spectravideo), [SC-3000](platform/sc3000), [EINSTEIN](platform/einstein)  |                                                                                                                                                                                                                                                                                                                
 | Comments   | n/a                                                                                                             |                                                                                                                                                                                                                                                                                                                       


NOTE: not all the functions are fully supported on all the listed targets


The GFX library by Rafael de Oliveira Jannone is almost fully included.
A relevant extension is the "native" assembly implementation of the "stencils" (codename adopted in z88dk for this rendering technique), permitting to quickly display complex, sizable, filled shapes.

Note that Rafael's work is meant to run with the Hitech C compiler, don't bother him for z88dk related issues !


