
z88dk provides a variety of decompression routines as part of the library covering a range of decompressors. These routines have been provided with C function wrappers and are callable from code compiled using both sccz80 and zsdcc.

A comparison of several z80 decompression algorithms can be found here; https://www.cpcwiki.eu/forum/programming/new-cruncher-zx0/msg197727/#msg197727, but can be roughly summarised as "use ZX0".

## ROM data section compression

Both classic and newlib libraries support storing the data sections compressed in ROM and then expanding them into RAM. This behaviour is controlled by the `CRT_MODEL` pragma option;

* `CRT_MODEL=1` The data section is copied from ROM into RAM on program start.
* `CRT_MODEL=2` The data section is stored compressed in ROM and decompressed into RAM on program start. The compression used is zx7.
* `CRT_MODEL=3` The data section is stored compressed in ROM and decompressed into RAM on program start. The compression used is zx0.



## ZX0 Decompression `<compress/zx0.h>` 

Source site: https://github.com/einar-saukas/ZX0

Available: z80, z80n, Rabbit

ZX0 is an optimal data compressor for a custom LZ77/LZSS based compression format, that provides a tradeoff between high compression ratio, and extremely simple fast decompression.

C wrappers, including `__z88dk_callee` variants are available for all of the decompressor variants include the +zx centric [RCS](https://github.com/einar-saukas/RCS) routines for ZX Spectrum screens.

This is the recommended compression algorithm that new applications use.

## ZX1 Decompression `<compress/zx1.h>`

Source site: https://github.com/einar-saukas/ZX1

Available: gbz80, z80, z80n, Rabbit (not all variants available on gbz80)

ZX1 is a simpler but faster version of ZX0 that sacrifices about 1.5% compression to run about 15% faster. 

C wrappers, including `__z88dk_callee` variants are available for all of the decompressor variants include the +zx centric [RCS](https://github.com/einar-saukas/RCS) routines for ZX Spectrum screens.

The compression tool is not supplied with z88dk, so you'll need to download and compile it from the upstream site linked above.


## ZX2 Decompression `<compress/zx2.h>`

Source site: https://github.com/einar-saukas/ZX2

Available: gbz80, z80, z80n, Rabbit


ZX2 is a minimalist version of ZX1. It's intended to help compressing programs and data blocks so small that saving a few bytes in the decompressor itself can make a difference (for instance to create 1Kb demos).

The compression tool is not supplied with z88dk, so you'll need to download and compile it from the upstream site linked above.


## ZX7 Decompression `<compress/zx7.h>`

Available: gbz80, z80, z80n, Rabbit (not all variants available on gbz80)

C wrappers, including `__z88dk_callee` variants are available for all of the decompressor variants include the +zx centric [RCS](https://github.com/einar-saukas/RCS) routines for ZX Spectrum screens.

## APLib Decompression `<compress/aplib.h>`

[APLib](https://www.ibsensoftware.com/products_aPLib.html) is a general-purpose compression library written by Jørgen Ibsen, based on his aPACK executable compressor.

This decompressor is particularly useful on the +sms platform due to the 
availability of a direct to VRAM decompressor.

## PUCrunch Decompression `<cpc.h>`

PUCrunch decompression is available for +cpc only. The following function is provided:

    void cpc_Uncrunch(void *src, void *dest)

This function is provided for legacy purposes only, and it is strongly
recommended that new projects use one of the ZX decompressors.

## Exomiser 2 Decompression `<cpc.h>`

Exomiser 2 decompression is available for +cpc only. The following function is provided:

    void cpc_UnExo(void *src, void *dest)


This function is provided for legacy purposes only, and it isstrongly
recommended that new projects use one of the ZX decompressors.