# OBSTACK.H

| | |
|-|-|
| Include    | #include `<obstack.h>` or #include `<alloc/obstack.h>`     |                                                                  
| Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/alloc/obstack.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sccz80/alloc/obstack.h) |
| | [{z88dk}/include/_DEVELOPMENT/sdcc/alloc/obstack.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/alloc/obstack.h) |
| Source     | [{z88dk}/libsrc/_DEVELOPMENT/alloc/obstack](https://raw.githubusercontent.com/z88dk/z88dk/master/libsrc/_DEVELOPMENT/stdlib/z80/) |
http://gcc.gnu.org/onlinedocs/libiberty/Obstacks.html

# CREATING OBSTACKS

## void *obstack_init(struct obstack *ob, size_t size)

# ALLOCATION IN AN OBSTACK

## void *obstack_alloc(struct obstack *ob, size_t size)

## void *obstack_copy(struct obstack *ob, void *p, size_t size)

## void *obstack_copy0(struct obstack *ob, void *p, size_t size)

# FREEING OBSTACK OBJECTS

## void *obstack_free(struct obstack *ob, void *object)

# GROWING OBJECTS

## void *obstack_1grow(struct obstack *ob, int c)

## void *obstack_blank(struct obstack *ob, int size)

## int obstack_grow(struct obstack *ob, void *data, size_t size)

## int obstack_grow0(struct obstack *ob,void *data,size_t size)

## void *obstack_int_grow(struct obstack *ob, int data)

## int obstack_printf(struct obstack *ob, const char *format, ...)

(stdio)

## int obstack_vprintf(struct obstack *ob, const char *format, va_list arg)

(stdio)

# FAST GROWING OF OBJECTS

## void *obstack_1grow_fast(struct obstack *ob,int c)

##  void *obstack_blank_fast(struct obstack *ob, int size)

## void *obstack_int_grow_fast(struct obstack *ob, int data)

# STATUS OF AN OBSTACK

## void *obstack_base(struct obstack *ob)

## void *obstack_finish(struct obstack *ob)

## void *obstack_next_free(struct obstack *ob)

## size_t obstack_object_size(struct obstack *ob)

## size_t obstack_room(struct obstack *ob)

# OBSTACK DATA ALIGNMENT

## size_t obstack_align_distance(struct obstack *ob, size_t alignment)

## int obstack_align_to(struct obstack *ob, size_t alignment)

