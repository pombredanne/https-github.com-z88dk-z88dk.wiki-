# Debug (debug.h)

 | Header     | [{z88dk}/include/debug.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/debug.h)   |
 | ----------------------------------------------------------------------------------------------------------------
 | Source     | [{z88dk}/libsrc/debug](https://github.com/z88dk/z88dk/tree/master/libsrc/debug/)                  |
 | Include    | #include `<debug.h>`           |                                                                     
 | Linking    | -ldebug                      |                                                                     
 | Compile    | n/a                          |                                                                     
 | Comments   |                              |                                                                     

Low level debugging library.


# DEBUG API

## disz80

unsigned int disz80(unsigned int address, unsigned int lines);

Disassembles code at the given address.  If the MYSELF constant is passed in place of a valid address, it will disassemble the current program location, showing how really the C program has been compiled.
Returns the address reached.



## dump

unsigned int dump(unsigned int address,unsigned int count);


Dump function: if the MYSELF constant is passed as an address, displays #count stack words and returns the current SP value (dump excluded), otherwise dumps memory bytes and returns the address reached.


