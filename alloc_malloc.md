# MALLOC.H


| | |
|----|---|
| Include    | #include `<malloc.h>` or #include `<alloc/malloc.h>`      |
| Header     | [{z88dk}/include/_DEVELOPMENT/sccz80/alloc/malloc.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sccz80/alloc/malloc.h?content-type=text%2Fplain) |
| | [{z88dk}/include/_DEVELOPMENT/sdcc/alloc/malloc.h](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/include/_DEVELOPMENT/sdcc/alloc/malloc.h?content-type=text%2Fplain) |

Other References:


*  [GNU libc Manual](http://www.gnu.org/software/libc/manual/html_node/Unconstrained-Allocation.html#Unconstrained-Allocation)

*  [The Open Group](http://pubs.opengroup.org/onlinepubs/9699919799/)

*  [The C11 Draft Standard](http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/c11_n1570.pdf)

The C11 standard requires that the malloc-related functions be protected by a lock against simultaneous access from multiple threads.  The library implements these locks but they are deactivated by default in the [library configuration](temp/front#library_configuration).  These locks should remain deactivated until the library has incorporated multithreading.

Dynamic memory is allocated out of the heap.  The heap is a single contiguous block of memory whose size is set at either compile-time or runtime depending on the [crt configuration](temp/front#crt).  The actual memory space used by the heap will either be reserved in the BSS section or it will lie between the end of the BSS section and the stack.

**THE** heap refers to the particular heap implied by the standard C library functions malloc, free, etc.  The library allows the creation of multiple heaps from which allocations can occur using the named heap API.  The only difference between the standard C library functions and the named heap API is that the named heap functions must be supplied the address of the heap to allocate from.  The address of the standard library's implied heap is **%%__malloc_heap%%** (C name "_malloc_heap") and if this is given to the named heap API, those functions will allocate out of the same heap as the standard C library functions.

There are four purposes served by the named heap API:


*  Available memory from disjoint areas of the memory map can be assigned to different heaps and used as a dynamic memory pool.

*  Individual heaps can be assigned to allocate memory for different purposes.  This may help to solve fragmentation issues.

*  Heaps can be created in different memory banks.

*  In a multithreading environment, threads can be given their own heaps so that blocking isn't an issue.

When allocating or freeing from a heap, the entire heap must be paged in.

## TYPES

```
heap_info_t  | struct { int type; void *address; size_t size };
```
 
# MEMORY MANAGEMENT FUNCTIONS

### void *aligned_alloc(size_t alignment, size_t size)

Attempts to allocate *size* bytes of memory with alignment *alignment* from the heap in first-fit order.  *alignment* must be an exact power of two and if it is not, the next higher power of two will be used instead.  A pointer to the successfully allocated memory is returned.

If allocation fails, 0 is returned, the carry flag is set and errno is set to:


*  [EINVAL] alignment is out of range

*  [ENOMEM] sufficient memory unavailable

### void *calloc(size_t nmemb, size_t size)

Attempts to allocate *nmemb* times *size* bytes of memory from the heap in first-fit order.  If the allocation is successful, the allocated memory is zeroed and a pointer to the block is returned.

If allocation fails, 0 is returned, the carry flag is set and errno is set to:


*  [ENOMEM] sufficient memory unavailable

### void *_falloc_(void *p, size_t size)

Attempts to allocate *size* bytes at address *p* from the heap.  If successful *p* is returned.

If allocation fails, 0 is returned, the carry flag is set and errno is set to:


*  [ENOMEM] memory unavailable

### void free(void *p)

If *p* is 0 the function returns.  Otherwise the block of memory pointed to by *p* is returned to the heap for re-use.

### void *malloc(size_t size)

Attempts to allocate *size* bytes from the heap in first-fit order.  A pointer to the successfully allocated memory is returned.

If allocation fails, 0 is returned, the carry flag is set and errno is set to:


*  [ENOMEM] sufficient memory unavailable

### void *memalign(size_t alignment, size_t size)

An alias for aligned_alloc().

### int posix_memalign(void **memptr, size_t alignment, size_t size)

Attempts to allocate *size* bytes of memory with alignment *alignment* from the heap in first-fit order.  *alignment* must be an exact power of two and if it is not, the next higher power of two will be used instead.  A pointer to the allocated memory is written to *memptr*.

If allocation fails, a non-zero errno is returned (below), the carry flag is set and errno is set to:


*  [EINVAL] alignment is out of range

*  [ENOMEM] sufficient memory unavailable

### void *realloc(void *p, size_t size)

If *p* is 0, locates the largest block of memory available in the heap and attempts to allocate *size* bytes from it.

If *p* is not zero, attempts to resize the allocated block *p* to *size* bytes.  This can include shrinking the allocated block or trying to expand it in place.  If the block is being expanded and *size* bytes cannot be allocated then a new allocation of *size* bytes is attempted from the largest block available in the heap.  If that is successful, the bytes stored at address *p* are copied to the new memory allocation.

If successful a pointer to the new memory block is returned and *p* should be discarded.

If unsuccessful, *p* is still valid and there are no other side effects.

If allocation fails, 0 is returned, the carry flag is set and errno is set to:


*  [ENOMEM] sufficient memory unavailable


# NAMED HEAP - INITIALIZATION

### void *heap_init(void *heap, size_t size)

Creates a heap of *size* bytes at address *heap*.  *size* must be at least 14 bytes (no error checking is done).  Returns *heap*, the address of the heap.

	char myheap[5000];
	...
	heap_init(myheap, 5000);      // initialize heap, 5000 bytes available
	a = heap_alloc(myheap, 100);  // allocate 100-byte block


### void *heap_destroy(void *heap)

Destroys the mutex associated with the heap.  When the library incorporates multithreading this may release any threads waiting on the heap.


# NAMED HEAP - INFORMATION

### void heap_info(void *heap, void (*callback)(heap_info_t *hi))

Iterates over all the blocks in *heap*, calling *callback* with a pointer to block descriptor *hi* for each block. The memory pointed at by *hi* is allocated from the stack and is owned by the library.

The *callback* function receives information about the block in a *heap_info_t* structure:

```
 | hi->type     | 0 for header, 1 for allocated, 2 for available  |
 | ----------------------------------------------------------------
 | hi->address  | the address of the block  |                      
 | hi->size     | the size of the block in bytes  |                
 ```

# NAMED HEAP - MEMORY MANAGEMENT FUNCTIONS

*``__malloc_heap``* holds the address of the standard library's heap.  The 16-bit value stored there is the address that should be passed as *heap* in the functions below if allocation out of the standard library's heap is desired.  *heap* is the address of the memory block used for allocation.

### void *heap_alloc(void *heap, size_t size)

See malloc().  Allocates out of the heap *heap*.

### void *heap_alloc_aligned(void *heap, size_t alignment, size_t size)

See alloc_aligned().  Allocates out of the heap *heap*.

### void *heap_alloc_fixed(void *heap, void *p, size_t size)

See _falloc_().  Allocates out of the heap *heap*.

### void *heap_calloc(void *heap, size_t nmemb, size_t size)

See calloc().  Allocates out of the heap *heap*.

### void heap_free(void *heap, void *p)

See free().  Returns memory to the heap *heap*.

### void *heap_realloc(void *heap, void *p, size_t size)

See realloc().  Allocates out of the heap *heap*.

