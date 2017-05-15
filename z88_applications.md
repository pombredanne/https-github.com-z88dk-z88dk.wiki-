# Creating Z88 Applications

## Introduction

## Constructing the Application Dor

For the creation of applications two header files are important: dor.h and application.h - both of these need to be included in order to construct the application DOR.

dor.h contains some constants for the application type and application.h uses these constants to construct the DOR, hence they should be included in this order. However! It's not as simple as that - application.h needs some information from you such as application name etc - there are defaults, but I don't think you'd want to use those!

The information is given courtesy of #define in between the including of dor.h and application.h, you should define the following:

	#define HELP1   "..."
	#define HELP2   "..."
	#define HELP3   "..."
	#define HELP4   "..."
	#define HELP5   "..."
	#define HELP6   "..."


The information for the application help page, if HELP1 is not defined then the standard HELP page is used `<grin>`, if any of the others (HELP2 to HELP6) is not defined then "" is substituted

	#define APP_NAME "..."


The name of the application (as appears in the Index)

	#define APP_KEY  '[KEY]'


The hotkey for the application

	#define APP_INFO "..."


The equivalent of *name from BASIC

	#define APP_TYPE [constants or'd together as from dor.def]


The application type, if you don't define this then the default type is bad with OZ preserving the screen (i.e. very, very bad!)

	#define APP_TYPE2 [constants]


The application type byte 2 - specifies whether caps lock should be on or not.

All of the constants above are in z80asm format - not C style, so insert Z88 control sequences in the form ".." &blah &".." and not \[blah] or you might get a bit of suprise!

## Handling Redrawing of the Screen

As mentioned above, the default application type is bad and to let OZ redraw the screen, this isn't particularly friendly as you can imagine so you can turn off the redraw screen flag by ''#define APP_TYPE'' yourself, this does however leave you with a bit of a problem - you won't be able to redraw your screen, so, to this end, I've provided an facility for you to be able to do it - simply define a function as follows:


	#pragma redirect redrawscreen = _my_redrawfunction 
	void my_redrawfunction() 
	{
	        /* Put some code here */
	}


For an example of this see the examples/app/dstar.c demo.

## Menus and Topics

You are allowed up to 7 topics per application and 24 items within each topic (if you ever fit this many into an app please let me know!) you define a topic with #define TOPIC[num] "[name]". Each item under a topic is named via #define TOPIC[num]_[item], it's hotkey by #define TOP[num]_[item]KEY, code by #define TOPIC(...)CODE and its attribute (see dev notes and dor.h) by #define TOPIC(...)ATTR. But thats getting confusing, so have a look at the dstar.c example in examples/app to see what I mean.

Each topic and each command on a topic may have a help page attached to it, have a look at useless.c to see how it's done - remember don't specify so much help that it goes over the 8k limit or all hell will break loose (I'm sure you can live with that limitation!)

As an aside, topics also have an attribute byte, which can be defined by #define TOPIC[num]ATTR. All attributes can be left out in which case they default to 0.

In order to handle menu selections you should define a function thus:

	#pragma redirect handlecmds = _my_handlecmds_func
	
	void my_handlecmds_func(int code)


Where code is the command code that has been selected. If you don't do this then you can't react to your menus!.

## Application Types

### Tuning options

	
	#pragma -reqpag=X      - Set the number of "bad" memory pages. z88dk will calculate this value for normal usage
	#pragma -expandz88     - Require an expanded z88
	#pragma -no-expandz88  - Doesn't require an expanded z88
	#pragma -zorg=X        - Set the origin of the application to X (default is 49152)


### Bad, but not so bad Applications

If the amount of static variables used by your application is less than 8k (the minimum amount of bad memory that can be allocated) then give the flag -reqpag=num to the frontend where num is the number of 256 byte pages of bad memory required - the unused memory up to the 8k limit is given back to OZ on pre-emption. To find out what num is for your application look at the .opt/.asm file and search for DEFVARS and count the number of bytes there. Sorry, there is no foolproof way to do this automatically. For example the dstar.c program needs -reqpag=5 and rpn.c needs -reqpag=1. Remember that the startup code (maybe unnecessarily) reserves a byte at $2000 so add 1 to your variable count before dividing by 256!

### Good Applications

It should in theory be possible to write good applicatons with the kit - simply don't use any static variables, supply the flag -reqpag=0 to the frontend and of course set the the appropriate APP_TYPE

I have placed all variables used by the startup code into safe workspace area to allow this facility - I reserve 50 bytes which is more than sufficient for our needs (we need 47 ATM) to allow for future expansion.

If you're feeling particularly brave, you can write good apps that have static variables...as long as you don't have too many of them that is. You can specify the flag -safedata=[n bytes] where n is the amount of static data that you want, this space is taken from the safe workspace, which remember is also shared by the stack, mail and other such things. A suppose a safe limit would be in the region of 500 bytes or so, but experiment, if you don't have arrays of local variables and no recursion you might be able to get away with more, as the saying goes, your mileage may vary!!

## Changing the origin of the Application

The default origin for an application is 49152 - this should give you plenty of space to do whatever you like, however, perhaps you want to slot it in elsewhere in an eprom - in this case use the -zorg= flag. NB. The application DOR created assumes that it is located in top top bank (63) of an EPROM cartridge, so unless you have a large application don't relocate it to < 49152 [This is to ensure that the
application entry point is in segment 3 - the DOR contains a dummy startup which then jumps to the real start address.]

## Using a Near Heap

## Using the Far Heap

### Overview & Limitations

The Z88 far memory model used by the Z88 Development Kit uses a 3-byte far pointer size. Near pointers can easily be cast as far pointers, so it is easy to write programs using a mixture of near and far memory. It is obviously not possible to cast far pointers as near pointers, however.

There is no limit to the size of the far memory heap (other than the 4Mb limit imposed by the Z88 architecture!), and allocations of any size can be made, provided your heap is large enough and the host Z88 has sufficient RAM available. Very large heaps will require more of your bad application RAM to be reserved for memory management, however, and full details of this are given in the next section.

The far memory model is only supported by the application target type in Small C; so it is not possible to write programs running under BASIC using far data. Due to the memory overheads required by the allocation routines (see implementation details if you're interested), any application which uses this model will be bad.

Packages may use far pointers as parameters passed to them by applications, but since each application has its own far heap managed through its static workspace, the following limitations apply:

   * the far pointer may only be used while the process to which it belongs is active
   * Unordered List Item
   * far pointers may not be passed from one process to another

###  Compiler Directives 

Using the far memory allocation words in your Z88 applications is quite straightforward. Firstly, you must signal that you will be using the far memory functions with the following line:

	  #define FARDATA 1


This line should be before any includes, as the standard library header files use it to determine whether to make far memory versions of the library functions available.

Secondly, specify the size of the far heap your application requires with a line as follows:

	  #pragma -farheap=65536


The above line gives a heapsize of 64K. You can specify any size up to a maximum of 4194304 (4Mb). Bear in mind the following points:

   * memory is not actually reserved in the Z88 until it is allocated with the malloc function, so you can specify a large heapsize without worrying that your application is "wasting" large amounts of the Z88s memory
   * the amount of bad application memory (or static workspace) required by the allocation system is equal to (HEAPSIZE/128)+487 bytes, where HEAPSIZE is the maximum heap size in bytes

For example, if you have a maximum heapsize of 65536 bytes, the amount of static workspace used will be 999 bytes. A 4Mb heap would require about 32.5K of static workspace!

### Allocating and using far memory

Within your C application, you can now use the following functions:

	far void *malloc(long size)


Returns a far pointer to a region of memory "size" bytes in size

	void free(far *ptr)


Free the previously allocated region of memory at "ptr"

	void freeall(void)


Frees all allocated memory (this is done automatically when applications are terminated)

You can use various string functions on far memory pointers; where functions take two parameters, you can also provide near pointers as parameters (remembering to cast them as far!) The following functions are currently available for far pointers: strchr, strrchr, strcpy, strncpy, strcat, strncat, strlen, strlwr, strupr.

For more information, take a look at the example in examples/app/farmall/.

### Implementation details

Knowledge of the way far memory is implemented on the Z88 may be helpful to anyone wishing to write a similar library for another machine, or for those hoping to port the method to their own (non-C) environment.

Also, if you are planning to rewrite the Z88 operating system (!) or replace any of the memory-allocation calls using the pkg_ozcr call, you should take careful note of the assumptions I have made here, and try not to break them ;-)

Note that all the assumptions made below are true for all versions of OZ up to and including v4.0

####  Memory overheads 

Whenever far memory support is required in an application, a certain amount of bad application workspace is automatically reserved. This consists of:

   * pool_table (224 bytes) Used to hold memory pool handles allocated by OZ
   * copybuff (258 bytes) Used by some library routines as a temporary buffer for OZ calls that use strings (this is not used internally by memory allocation routines, and will not be considered further)
   * malloc_table (2 bytes for every 256 bytes required in the far heap) The most important structure, holding details of the bank and address of each 256-byte allocation in the heap

#### The pool table

This structure, as noted above, is used to hold the pool handles that have been opened by the application. Initially, it is filled with nulls, indicating that no pools are open.

This is a table of 1-byte entries, each corresponding to a bank in the Z88's memory map. As banks 0-31 are always ROM (internal), the first entry corresponds to bank 32, with the final entry corresponding to bank 255. Each entry holds either 0 (indicating no pool open for that bank), or a value which relates directly to the pool handle returned by OZ for that bank.

Three important assumptions are made when constructing and using this table:

   * each OZ pool handle always gives allocations from the same bank; when the bank is exhausted, no further allocations are possible with the handle
   * as a consequence, for each process, only one pool handle is ever associated with a particular bank
   * the values of OZ pool handles are always of the form $0XY0, where $XY!=$00

The first two assumptions mean that we only need 224 table entries (one for each bank) no matter how much memory we allocate. The last assumption means that each entry only needs to be one byte in size (we shift the handle value 4 bits to the right), and a zero value can indicate no pool.

#### The malloc table

Each entry in the malloc table consists of two bytes, and corresponds to an allocation of 256 bytes of far memory.

The first byte contains the bank number containing the allocation (0 indicates no allocation has been made for this entry), and the second byte contains the high byte of the address of the memory allocation (always in segment 1).

The low byte of the address is not required, since for allocations of 256 bytes, OZ always returns an address with a low byte of zero; thus this is always assumed.

#### Allocation method

The first stage in memory allocation is to locate a section of the malloc table which has not yet been allocated (ie contains nulls) and which is large enough for the required allocation. An extra 2 bytes are always added to the amount of memory required, to hold the number of pages (256-byte allocations) in the allocated block.

When a suitable region has been located (malloc always uses the smallest suitable region), we start making allocations and filling the malloc table. This is done as follows:

first, existing pools in the pool table are checked, and allocations made from these
if all existing pools are exhausted, new pools are opened and allocations made from these
If all pools are exhausted and no further ones can be opened before all memory is allocated, the memory allocated so far is freed, and malloc returns an error.

Once the allocations are completed, the number of 256-byte pages used is stored in the first two bytes, and a far pointer following this value is returned.

#### Deallocation method

Deallocating is much more straightforward. The pointer is decremented twice, and the value fetched is thus the number of 256-byte pages that were allocated.

A loop is entered which frees each 256-byte allocation, and zeros the corresponding entry in the malloc table.

Note that no pool handles are freed during this process, but they become usable again when further mallocs are performed. All handles in the pool table are freed by the freeall function, which is automatically called on exit from the application.
