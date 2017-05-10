# Unsupported Targets - hardly possible

In this page we're listing those z80 targets platforms presenting limits or technical constraints which will make a z88dk port quite challenging.



### Epson RC-20

This is a.. watch !


### GameBoy

The GameBoy is not a full z80 CPU, i.e. a big portion of the z88dk libraries rely on one of the index registers, which simply does not exist on the GameBoy.


### NEC V20 / V30 CPUs

Such CPUs are known to emulate the 8080 opcodes but the Z80 is not emulated).

If anyone has different information please report it !
This would permit support for a good range of earlier IBM PC clones and the Casio CFX/AFX/FX/Prizm family


### 8085 CPUs

Sorry, crucial opcodes used by z88dk are missing.
Same for 8080, obviously.


### Seiko UC-2000/UC-2200

This is a docking station for a ..watch !
Not sure it can be programmed in a different language than BASIC and it is a quite rare item.   Probably too small and fragile on the physical and logical points of view.

### Texas Instruments calculator TI-81

Only 2048 bytes of memory for program storage.  Possible file transfer issues.

[Announcment telling it is possible to force asm code execution on the TI81](http://www.ticalc.org/archives/news/articles/14/145/145220.html)

### 80486 CPU

An AWK script to convert all the z80 opcodes to 80486 equivalents is provided.  By the way z88dk is not ready for it, the whole build framework would have to be re-arranged, libaries need a heavy adjustment and a different assembler should be invoked.

