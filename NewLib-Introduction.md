The new C library aims to implement as large a subset of C11 as is reasonable on an 8-bit target.  The library does not confine itself to the standard and adds many non-standard functions drawn from BSD and GNU, as well as libraries aiming to support text, graphics and sound among other things.  The library is still under development but it is already extensive featuring more than 700 functions.

The library is unique in several ways:

  * **It is written in assembly language.**  The C compilers are currently generating code that is 3-5x slower and larger than human written assembly language.  By supplying an extensive library written in assembly language where most execution time is spent, compiled code can approach the performance of assembly language and can be several times smaller than it would be otherwise.
  * **It is extensive.**  Most z80 C libraries only implement a minimal subset of the C standard with many noticeable omissions.  Additionally, the non-standard libraries supplied by z88dk are things likely not seen before by many z80 programmers; it includes data compression, proportional fonts, music synthesis and some C++ STL containers.
  * **It is designed to be able to generate standalone code.**  The library is not dependent on any existing operating system to implement any features.  C code can be compiled, stored in ROM and run at boot-up without any fuss.  Of course the library can also use an existing operating system if one is present.
  * **It is highly configurable.**  Library build time options can select between fast+large and slow+small implementations of integer arithmetic, number<->ascii conversion, etc and can individually include or exclude printf and scanf converters to reduce final binary size.  Pragmas in C source can communicate various crt options at compile time; among these are selecting heap size, RAM or ROM model, code origin, stack location.
  * **It is designed with bankswitched memory in mind.**  Library code is assigned to sections at fine granularity which allows library code to be placed flexibly in any memory region.  By default the crts generate CODE, DATA and BSS blocks but this can be changed to store code and data in any user-defined memory sections.  Some library functions are extended to support bankswitched memory; an example is the malloc library which has been generalized to allocate memory out of multiple heaps, any of which could be located in different memory banks.
  * **It is intended to be highly portable.**  In keeping with z88dk's goal of directly supporting any z80 target, the library can be ported to any target by providing a small number of customized functions.  Because the library is more complicated under the hood, porting is not quite as easy as with z88dk's classic library and does take some expertise.
  * **It is reentrant.**  All code is written to be reentrant with few exceptions (these would include functions like strtok() which are non-reentrant by definition and a few functions that are unavoidably self-modifying like the tritone music player).
  * **It is easy to use.**  If your particular z80 machine is a supported target, you can begin compiling immediately.  If it is not, you can use the generic "embedded" target to compile with.

Some information on the library:

  * **All functions take advantage of callee and fastcall linkage**.  These linkages can reduce binary size by several hundred bytes in large programs.
  * **The library is written to use one index register (IX)**.  This makes it more compatible with some known z80 targets that reserve one index register for themselves.  An assembler option (-Ca-IXIY) can switch IX for IY when the library is built.
  * **The library takes advantage of the z80 architecture.**  The exx set is used to carry out parallel tasks within the library, to hold a 48-bit float and to speed up integer math, among other things.  On occasion, IX and IY are split into their 8-bit halves.  These instructions are undocumented but are reliable on all z80 variants.  Because it does make it more difficult to port the library to some targets, use of the z80's extra registers has not been done haphazardly.
  * **All functions are available from assembly language**.  The c library is written in assembly language and has both assembly and c entry points.
  * **Stdio is object oriented**.  This allows sophisticated drivers to be implemented using code inheritance from the library.  The base classes currently contain code to implement terminals and serial devices.

The library was written to be compatible with sdcc; some effort has been made to improve sdcc's output when used with z88dk:

  * **sdcc uses the z88dk libraries.**  The z88dk libraries are more complete, faster and smaller.
  * As mentioned above, **all functions take advantage of callee and fastcall linkage.**  They also include attributes that **inform the compiler when registers are unchanged by functions**.  These are new features in sdcc that are not used by the sdcc-supplied libraries.
  * Assembly output from sdcc is post-processed so that **sdcc's calls to its primitives also use callee linkage.**
  * **More than 600 peephole optimizer rules have been added** to correct a couple of code generation bugs and to reduce code size further.  To enable these rules add "-SO3" to the compile line.

**Notable omissions:**

  * Disk i/o is not integrated yet.  This also means opening and closing files is not complete and disk related C11 functions are not present.
  * Multi-byte characters will likely never be supported.
  * C11 threading is not integrated yet.  The library config file contains options to enable locking on files -- leave those disabled.