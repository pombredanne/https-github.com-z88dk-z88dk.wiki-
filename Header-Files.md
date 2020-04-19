* adt.h - a collection of container classes modelled on C++ STL
  * adt/b_array.h - array of bytes
  * adt/b_vector.h - vector of bytes
  * adt/ba_priority_queue.h - priority queue of bytes built on b_array
  * adt/ba_stack.h - stack of bytes built on b_array
  * adt/bv_priority_queue.h - priority queue of bytes built on b_vector
  * adt/bv_stack.h - stack of bytes built on b_vector
  * adt/p_forward_list.h - singly linked list of objects
  * adt/p_forward_list_alt.h - singly linked list of objects with O(1) tail
  * adt/p_list.h - doubly linked list of objects
  * adt/p_queue.h - queue of objects built on p_forward_list_alt
  * adt/p_stack.h - stack of objects built on p_forward_list
  * adt/w_array.h - array of words
  * adt/w_vector.h - vector of words
  * adt/wa_priority_queue.h - priority queue of words built on w_array
  * adt/wa_stack.h - stack of words built on w_array
  * adt/wv_priority_queue.h - priority queue of words built on w_vector
  * adt/wv_stack.h - stack of words built on w_vector
* alloc.h - a collection of memory allocators
  * alloc/balloc.h - fixed size block allocator
  * alloc/malloc.h - variable size allocation out of fixed size heap
  * alloc/obstack.h - variable size allocation in stack order
* arch.h - platform specific functions
  * arch/cpm.h - cp/m specific functions
  * arch/zx.h - zx spectrum specific functions (arch/spectrum.h continues to work)
    * arch/zx/bifrost_l.h - BIFROSTL Multicolour engine
    * arch/zx/bifrost_h.h - BIFROSTH Multicolour engine
    * arch/zx/nirvana+.h - NIRVANA+ Multicolour engine
    * arch/zx/nirvana-.h - NIRVANA- Multicolour engine
    * arch/zx/sp1.h - SP1 software sprite engine for zx (games/sp1.h continues to work)
* assert.h - assert() macro
* compress.h - a collection of data compression tools
  * compress/aplib.h - aPLib decompression
  * compress/zx7.h - optimal lz77 decompression
* ctype.h - character classification
* errno.h - errno definitions
* fcntl.h - open posix level file desciptor
* float.h - floating point characteristics
* font/fzx.h - fzx proportional fonts for bitmapped displays
* games/sp1.h - sp1 software sprite engine for bitmapped displays
* input.h - direct interface with user-input devices (keyboard, joystick, mouse)
* intrinsic.h - inline z80 instructions without impeding the optimizer
* inttypes.h - format conversion of integer types
* iso646.h - alternative spellings for operators
* limits.h - sizes of integer types
* malloc.h - standard C memory allocation
* obstack.h - gnu obstack stack allocation
* math.h - floating point math functions
* rect.h - points, intervals and rectangles
* setjmp.h - non-local jumps
* sound.h - a collection of audio functions
  * sound/aywyz.h - WYZ tracker for AY819x sound chips
  * sound/bit.h - audio generation functions using a 1-bit device
* stdalign.h - alignment
* stdarg.h - variable arguments
* stdbool.h - boolean type
* stddef.h - common definitions
* stdint.h - standard names of integer types without ambiguity
* stdio.h - stream input / output
* stdlib.h - general utilities (sorting, number↔ascii, random numbers, …)
* stdnoreturn.h - Noreturn
* string.h - string and raw memory manipulation
* stropts.h - ioctl of devices
* threads.h - threads
* unistd.h - posix level input / output on file descriptors
* z80.h - z80 related functions (precise delay, interrupts, port i/o …)