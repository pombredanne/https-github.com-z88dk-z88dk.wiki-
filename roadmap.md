## Release Schedule

z88dk will be released on the Sunday of the second week of July and January, thus the next release dates is:

   * v1.10   - 10/1/2010









## Roadmap

The roadmap contains a list of features that should be in a given release:

#### v1.9 - July 2009

   * stdio
New implementation of stdio available for testing on selected target(s).  The main purpose of the rewrite was to make the library 100% asm, reduce memory footprint, introduce support for multiple device drivers, make it possible to integrate sockets / file descriptors fully into the stdio model, and supply some missing functionality demanded by standard C.

   * strings
      * add strlcat()
      * add strlcpy()
   * stdlib
      * add qsort(), will accompany existing non-standard (but faster) l_qsort()
      * add bsearch(), will accompany existing non-standard (but faster) l_bsearch()
      * itoa() and ltoa() changed to output variable radix numbers which is more inline with modern implementations of these functions
   * input
      * fix in_GetKey() which was dropping keypresses unnecessarily
      * in_Wait() and in_Pause() now measure time in milliseconds
   * lib3d (mostly coming from the MSX HTC GFX lib)
      * new 'stencil' object for fast convex polygons dithered fill
      * optimizations in size and speed
      * new vector functions
      * big inprovements in managing triangles
   * New targets
      * add support for Galaksija, including lo-rez graphics and 1 bit sound support
      * add support for TRS80, including lo-rez graphics and 1 bit sound support
   * C128
      * "SG Tools" are now part of z88dk; SID sound, low and high resolution GFX, timers and much more.
      * much better base graphics support
      * support for both "CP/M" mode and C128 BASIC mode binaries
   * Amstrad CPC
      * fixed a bug in the ROM interposer causing crashes in many functions
      * many target specific improvements
      * much better base graphics support
   * MSX
      * add functions coming from "GFXLIB" by Rafael de Oliveira Jannone
      * much better base graphics support
      * target specific code for DSP sound, graphics and controllers.
   * CP/M
      * improved fcntl
   * TI83+ and TI84
      * appmake output binary format changed
   * ZX81
      * Functions to exchange integer values with BASIC
   * ZX Spectrum
      * target specifib lib slightly fixed and rearranged
      * ROM building support
   * Rabbit Control Module
      * add flash handling


#### v1.10 - January 2010

   * crt0 (C startup code)
      * Move all library static variables to the crt0s
      * Redo crt0s so that all of them can easily separate data and code areas
   * stdio
      * Migrate all targets to the new stdio
      * Add floating point converters to new stdio
      * Integrate sockets API for new stdio
      * (?) Integrate FAT16/32 middleware for new stdio
      * (?) Add some intelligence to the compiler to that it can automatically select the minimal set of stdio features needed in a compile
   * math
      * Ensure that all floating point packages support the same group of mathematical functions
      * Perhaps standardize how each floating point package is called by the compiler
      * Make it easier and clearer as to how a new floating point package can be added to the compiler


#### Future

Features on the to-do list that may or may not be promoted to a release above.


*  z88dk package
     * CVS snapshots available on a frequent basis
     * Removing all the old documentation and ship a copy of the wiki with the kit
     * Updated examples

*  Compiler / Assembler / Linker
     * Clean up of the compiler
     * Some more magic in the compiler (not sure what yet!)
     * Cleanup some ugliness in the zcc driver
     * Migration to MPM as the assembler of choice
     * Give user control over declaring multiple code and data segments
     * Introduce a standard bank-switching model so that the compiler can automatically create projects larger than 64k in size

*  Miscellaneous
     * z88 spectrum screen emulation
     * Test framework in place for generic library functions
     * Cleanup the headers (similar to what Alvin has done to spectrum.h)

*  Abstract Data Types
      * add singly-linked list
      * add static hash table
      * add avl tree
      * add map type
      * check into whether it makes sense to follow C++'s STL model

*  Input
      * port to CPC, MSX

*  SP1 software sprite engine
      * port to CPC
      * port to MSX, Timex hi-colour mode

*  math
      * look into adding numerical algorithms

*  stdio
      * add compression / decompression filters to new stdio
      * grow device driver library

*  stdlib
      * evaluate quality and speed of prng described in this [http://www.jstatsoft.org/v08/i14/paper](paper) and use it in rand()
      * possibly provide rand() for different distributions
