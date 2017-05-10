# Introduction

z88dk has an extensive set of libraries that has evolved over many years; it comes to around 100,000 lines of assembly code.  This and the out-of-the-box targetting of a wide variety of z80 machines is what distinguishes z88dk from other z80 development environments.

The libraries are designed to be called from either C or assembler and the inclusion of a C compiler in the project allows rapid, portable, and simpler program development as an option.  Over the years the libraries have grown in a somewhat disjoint manner and sdcc has arisen as an alternative for compiling C programs for the z80.  The C language itself has also evolved over the past 40 years and the C libraries available for the z80 have not kept pace, if they were ever fully compliant in the first place.

The new clib is aiming to satisfy several goals:


*  Implement as large a subset of the C11 standard as is reasonable on an 8-bit uP.

*  Be the fastest and smallest z80 library among libraries with the most capability.

*  Separate implementation from the C interface so that the library is easily used in assembler projects and with any z80 C compiler.

*  Make use of the new sections feature of z80asm, the underlying linking assembler in z88dk. Sections permit the automatic generation of ROMable code and targetting of bankswitched memory systems.

*  The entire library must be re-entrant to support the multithreading features demanded in C11.

*  The library must be highly configurable at library build time and at compile time to allow the user to choose between fast or small implementations.

*  Supply an object oriented driver model that allows new targets to quickly gain access to tty-emulated input and output terminals as well as disk drivers.

*  Support any number of attached devices and assign them to a global namespace.

*  Portable to many z80 platforms.

*  Make the distinction between C for "big" machines and C for "small" machines disappear.

The new clib implements a larger set of standard C features than the existing clib but it does not contain all the non-standard libraries that many current users enjoy.  The new clib is an opportunity for us to rethink how things should be done and eventually new versions of those libraries, perhaps more capable, will migrate over.   The existing clib may also generate a smaller footprint for many programs because its i/o model is less general.  For these reasons, the existing clib will continue to exist in parallel with the new one for the foreseeable future.

# Overview

The new C library is written in assembler, is re-entrant and is multi-threading aware.  It currently consists of more than 400 functions and 30,000 lines of asm source.  The library design is ambitious and is meant to be similar in capability to common 32-bit C libraries.

The library is continually expanding but its current content is listed below.

## Abstract Data Types

A container library modelled on C++'s STL containers.

### Non-Intrusive Containers Storing Copied Data

#### b_array

A fixed size array holding bytes.  Modelled on C++ vector`<char>` but without the ability to expand.  Can be used as wrapper on top of a statically allocated C-array.  The library uses b_array in its terminal drivers to hold the edit buffer.

#### b_vector

A resizable array of bytes.  Modelled on C++ vector`<char>`, just like the C++ type, b_vector can grow via realloc as needed.  A max_size parameter can be specified to indicate a maximum size that any particular b_vector can grow to.  The library uses b_vector to implement getdelim() and memstreams.

b_vector is implemented on top of b_array.

#### w_array

A fixed size array holding 16-bit words.  Modelled on C++ vector`<void*>` but without the ability to expand.  Provides the same functionality as b_array but for words.

w_array is implemented on top of b_array.

#### w_vector

A resizable array of words.  Modelled on C++ vector`<void*>`, just like the C++ type, w_vector can grow via realloc as needed.  A max_size parameter can be specified to indicate a maximum size that any particular w_vector can grow to.  Provides the same functionality as b_vector but for words.

w_vector is implemented on top of b_vector.

#### ba_priority_queue, bv_priority_queue, wa_priority_queue, wv_priority_queue

A priority_queue storing either bytes (b*) or 16-bit words (w*).  Modelled on C++ priority_queue`<>`, this type maintains items in heap order according to a user-supplied comparison function.  Items can be extracted in order in O(lg n) time.  The underlying container storing items is one of b_array, w_array, b_vector or w_vector.  The vector containers allow growth via realloc while the array containers are static in size.  Extracting all items from a priority_queue will leave the items stored in the underlying container in sorted order; this is an implementation of heapsort.

#### ba_stack, bv_stack, wa_stack, wv_stack

A stack storing either bytes (b*) or 16-bit words (w*) in LIFO order.  Modelled on C++ stack`<>`, this data structure is a simple adapter of the underlying container functions.  The underlying container storing the items is one of b_array, w_array, b_vector or w_vector.  The vector containers allow growth via realloc while the array containers are static in size.

### Intrusive Containers Storing References to User Data

User objects stored in these containers must supply space in their data structures for use by the container.  The amount of space needed varies by container and can range from two bytes (singly linked list) to five bytes (red-black tree).  Intrusive containers do not have to be concerned with memory allocation issues and this can both reduce their code size and speed up actions.

#### p_forward_list

A singly linked list of objects modelled on C++ forward_list`<void*>`.  Objects stored in the list must have two bytes available to store next links.

Within clib, balloc (list of free memory blocks), stdio (list of FILE structures), and mutex (list of waiting threads) use p_forward_list.

#### p_forward_list_alt

A singly linked list of objects also modelled on C++ forward_list`<void*>` with the exception that adding an item to the end of the list can be done in O(1) time.  Objects must have two bytes available to store next links.  The cost is the p_forward_list_alt_t structure is four bytes compared to two for p_forward_list_t.

p_forward_list_alt is implemented on top of p_forward_list.

#### p_list

A doubly linked list of objects modelled on C++ list`<void*>`.  Objects stored in the list must have four bytes available to store next and previous links.

p_list is implemented on top of p_forward_list.

#### p_queue

A container that stores objects in FIFO order.  Items are pushed into the back of a queue and popped from the front.  p_queue is modelled on C++ queue`<void*>`.  Objects stored in the queue must have two bytes available to store next links.

p_queue is an adapter of p_forward_list_alt.

#### p_stack

A container that stores objects in LIFO order.  p_stack is modelled on C++ stack`<void*>`.  Objects in the stack must have two bytes available to store next links.

p_stack is an adapter of p_forward_list.

## Alloc

The library offers several options for memory allocation.

### balloc

A block memory allocator [(aka memory pools)](http://en.wikipedia.org/wiki/Memory_pool).  An array of lists of available blocks is maintained.  Each list contains memory blocks of known, fixed size.  Allocation occurs by requesting an available block from a specific list, thus retrieving a block of known size.  If the lists are maintained such that block sizes increase with the list array index, a bestfit function can be called to retrieve a block of certain minimum size.  The allocated blocks have one-byte of overhead to identify which list they belong to so that freeing can be performed without specifying which list the block came from.  The program is responsible for populating the lists of blocks and determining block sizes.

The block memory allocator is very fast, has small code footprint, has little overhead and suffers no external fragmentation.  The downside is block sizes are fixed and pre-determined.

balloc is well suited as an allocator for frequent allocation of small fixed size blocks.

### malloc

[The traditional C heap allocator](http://en.wikipedia.org/wiki/Malloc).  A heap is a single contiguous block of memory out of which allocations are made.  Allocation requests can be any size, can be requested from a specific address (see falloc, "fixed alloc" a non-standard function) or can be requested with specific alignment (see C11 aligned_alloc).  The allocation strategy is first-fit, except for realloc which applies largest-fit.  Realloc always allocates out of the largest block in the heap so that repeated realloc requests to grow a block can be very quickly satisfied.

There can be any number of heaps in a program and the malloc library can be directed to allocate out of a specific heap through a named-heap api, which is a similar idea to Win32's heap api.  When using the standard C allocation functions, the thread's implied heap is used to satisfy requests.  The support for multiple heaps was added specifically to aid management of memory in multiple memory banks.  It can also be used to solve synchronization issues between threads or to divide available memory among different purposes.

C11 requires that heaps are protected by mutexes and this is the case for the z80 implementation.  Callers can choose to use functions with an "_unlocked" suffix to bypass the mutex synchronization.

The heap is the most flexible memory allocator but is also slowest and largest as a result.  It can also suffer from fragmentation issues, especially on machines with limited memory space such as the z80.  Over long periods of repeated allocations and deallocations of variably sized blocks, the heap can become so fragmented that a program can no longer run.  It is for this reason that there is more than one memory allocator type in the z80 clib.

The C standard specifies that malloc is used for several library functions.  Replacing the malloc functions with compatible implementations of malloc, realloc and free can allow the program to use library functions in conjunction with custom memory allocators.

### obstack

[GNU's Obstack](http://en.wikipedia.org/wiki/Obstack).  An obstack is a single contigous block of memory from which objects are sequentially allocated.  A fence pointer is maintained, below which already allocated objects lie and above which lies the available memory.  A newly allocated object is given memory beginning at the fence's address and the fence is advanced past the end of the object, pointing at the next memory address available for the next object.  There is also a collection of functions that aid in growing an object at the top of the obstack, among them obstack_printf().

Free-ing amounts to rewinding the fence pointer.  This means it is not possible to individually free objects, except for the last one allocated.  Instead the state of the obstack is rewound to some earlier point in the program.  This is particularly useful in embedded systems where the program may allocate some memory from the obstack for the next task and when the task is completed a single obstack_free() is executed to rewind the state of the obstack to what it was before the task ran.  Memory performance is then a known in such a system with no possibility of leaks.

There can be any number of individual obstacks created in a program.  Because of its simple nature, the obstack is very fast and has medium size code footprint.

## Arch

Library code written for specific target machines.

### spectrum

Sinclair ZX Spectrum.

#### display

A complete set of functions for manipulating display file addresses.

#### font

#### graphics

Low level graphics primitives needed by the general graphics library.  The graphics lib will not be integrated initially so only contains a pattern flood filler at this time.

## Ctype

The C standard character classification functions.  There are separate C and assembler versions of each function in order to keep the asm versions as lightweight as possible as they are used frequently in the libraries.  The functions are procedural rather than table driven as the cost of a dedicated table was deemed wasteful.

## Error

Assembly level exit functions used by the library to exit and set error number.

## Font

A place to store platform independent font drivers and their fonts.

### fzx

A proportional font driver for bitmap displays originally designed for the ZX Spectrum, created by Andrew Owen and implemented by Einar Saukas.  More than 70 fonts are currently available.  The z88dk version of the library has been augmented to include functions for calculating string widths in pixels and for determining character metrics.

A PC tool is available for creating new fonts.

## Graphics

## L

A repository of low level assembly subroutines used by the compilers and the clib.  This ranges from the mundane such as fetching a word at an offset from HL to higher level activities including integer math and number `<->` string conversions.

## Locale

## Stdio

Stdio is implemented using a messaging method that implements a virtual file system (vfs).  Messages are generated by the stdio functions and are delivered through a chain of consumers terminating at a driver where a particular request is ultimately satisfied.  The vfs is divided into two parts, one part deals with character i/o and the other part deals with operations on directories.  The character i/o portion is complete and allows full implementations of most of the stdio functions, however there is currently no way to direct that output to a terminal or device.  Instead i/o can be performed on strings with sprintf / sscanf or i/o can be directed toward a memstream.  Memstreams are a C extension that can treat a block of memory as a file.

The stdio implementation is intended to be as fully featured as possible, with options to remove functionality to reduce code size.  These code reduction options are not available as of this time, however as the entire library is written in assembler, it should still compare favourably in terms of size against existing z80 implementations even with all features enabled.

Compared to the existing z88dk stdio implementation, the missing format specifiers are added as well as accurate character input/output counting for all concerned functions, and several missing functions are now added.

Compared to the existing sdcc stdio implementation, missing format specifiers are added, a mechanism for supporting any number of devices is added, the scanf half of stdio is added, and many missing functions are added.

**Currently not implemented:

remove, rename, tmpfile, tmpnam, fclose, fopen, freopen, setbuf, setvbuf, rewind.**

setbuf() and setvbuf() will likely never be implemented as more than no-op operations.  The buffers they specify are the big-machine solution to the very slow i/o requests of single characters the clib typically generates.  By buffering a large block of data from the device or file, those single char i/o requests can be satisfied locally without having to drop out of user space, into kernel space, through possibly several open fds and finally to a device driver.  Unfortunately, allocating buffers for each open FILE to solve this problem is a problem for small memory machines.  Instead, this clib has been written to use a different strategy to solve the problem.  i/o is performed in blocks rather than character at a time and a vfs message was introduced to allow character screening at the driver.  The screening allows the driver code to determine if a character will be accepted by the stdio caller.  If, for example, a call from fscanf is looking to read a decimal integer, the driver gets a screening function that will tell the driver when to stop reading chars that don't constitute a decimal integer.  In this way, character-at-a-time examination can occur without any performance hit and without intervening buffers.

The remaining functions can only be implemented when the second half of the vfs is completed.

C11 requires that FILE structures are protected by mutexes.  This is also the case with this implementation.  GNU introduced alternate entry points for some functions with an "_unlocked" suffix that allows stdio functions to bypass the mutex lock.  This feature does have a legitimate application outside of subverting the library's synchronization method so this clib has extended this feature to all stdio functions.

## Stdlib

Contains the standard C toolkit for number `<->` string conversions, random number generation, searching and sorting, memory allocation and communicating with the environment outside the program.  Most C11 functions are implemented along with some common functions found in gnu and posix.

**Currently not implemented: atof, atoll, strtod, strtof, strtold, strtoll, strtoull, getenv, system, llabs, lldiv, mblen, mbtowc, wctomb, mbstowcs, wcstombs.**

Floating point and environment functions are unavailable for now.

Support for multi-byte character sets will likely never be added.  sdcc supplies some basic 64-bit functions for longlongs but the library does not support that type as it is very inefficient on the z80 and has little value on small architectures.

## String

An extensive collection of string manipulation functions.  All C11 functions are implemented along with many functions found in gnu, posix and bsd libraries.

### Far String

The z80 is only capable of directly addressing 64k so programs are written assuming a 64k address space.  This is called the near memory model and is the normal software development model.

Some z80 systems add memory beyond 64k using various bankswitching methods.  The far memory model views all such memory as part of a single, linear address space.  Functions able to transparently operate in this larger address space are considered "far".  Treating bankswitched memory as part of a linear address space in a transparent manner costs a lot in terms of execution speed, code size and coding effort, so the library of far functions is necessarily small.

Currently a small collection of block move functions allows data to be copied between banks in a transparent manner.

## Temp

A location to store temporary library code.  This may be code from z88dk's regular libraries that has been modified to be callable from either sccz80 or sdcc, but has not yet been reviewed for inclusion in the new clib.  The code here is meant to help with testing using existing substantial projects.

### input

### sp1

## Threads

## Z80

