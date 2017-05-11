# BALLOC.H

| | |
|-|-|
| Include    | #include `<alloc/balloc.h>`                                                                                              |
| Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/alloc/balloc.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/alloc/balloc.h) |
| | [{z88dk}/include/_DEVELOPMENT/sdcc/alloc/balloc.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/alloc/balloc.h) |
| Source     | [{z88dk}/libsrc/_DEVELOPMENT/alloc/balloc](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/alloc/balloc) |
# BLOCK INITIALIZATION

## void *balloc_addmem(int q, size_t num, size_t size, void *p)

## size_t balloc_blockcount(int q)

# BLOCK ALLOCATION

## void *balloc_alloc(int q)

## void *balloc_firstfit(int q, int numq)

# BLOCK FREE

## void balloc_free(void *p)

