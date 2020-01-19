  * [[libnew:adt|adt.h]] - a collection of container classes modelled on C++ STL
    * [[libnew:adt/b_array|adt/b_array.h]] - array of bytes
    * [[libnew:adt/b_vector|adt/b_vector.h]] - vector of bytes
    * [[libnew:adt/ba_priority_queue|adt/ba_priority_queue.h]] - priority queue of bytes built on b_array
    * [[libnew:adt/ba_stack|adt/ba_stack.h]] - stack of bytes built on b_array
    * [[libnew:adt/bv_priority_queue|adt/bv_priority_queue.h]] - priority queue of bytes built on b_vector
    * [[libnew:adt/bv_stack|adt/bv_stack.h]] - stack of bytes built on b_vector
    * [[libnew:adt/p_forward_list|adt/p_forward_list.h]] - singly linked list of objects
    * [[libnew:adt/p_forward_list_alt|adt/p_forward_list_alt.h]] - singly linked list of objects with O(1) tail
    * [[libnew:adt/p_list|adt/p_list.h]] - doubly linked list of objects
    * [[libnew:adt/p_queue|adt/p_queue.h]] - queue of objects built on p_forward_list_alt
    * [[libnew:adt/p_stack|adt/p_stack.h]] - stack of objects built on p_forward_list
    * [[libnew:adt/w_array|adt/w_array.h]] - array of words
    * [[libnew:adt/w_vector|adt/w_vector.h]] - vector of words
    * [[libnew:adt/wa_priority_queue|adt/wa_priority_queue.h]] - priority queue of words built on w_array
    * [[libnew:adt/wa_stack|adt/wa_stack.h]] - stack of words built on w_array
    * [[libnew:adt/wv_priority_queue|adt/wv_priority_queue.h]] - priority queue of words built on w_vector
    * [[libnew:adt/wv_stack|adt/wv_stack.h]] - stack of words built on w_vector
  * [[libnew:alloc|alloc.h]] - a collection of memory allocators
    * [[libnew:alloc/balloc|alloc/balloc.h]] - fixed size block allocator
    * [[libnew:alloc/malloc|alloc/malloc.h]] - variable size allocation out of fixed size heap
    * [[libnew:alloc/obstack|alloc/obstack.h]] - variable size allocation in stack order
  * [[libnew:arch|arch.h]] - platform specific functions
    * [[libnew:arch/cpm|arch/cpm.h]] - cp/m specific functions
    * [[libnew:arch/zx|arch/zx.h]] - zx spectrum specific functions (arch/spectrum.h continues to work)
      * [[libnew:arch/zx/bifrost_l|arch/zx/bifrost_l.h]] - BIFROSTL Multicolour engine
      * [[libnew:arch/zx/bifrost_h|arch/zx/bifrost_h.h]] - BIFROSTH Multicolour engine
      * [[libnew:arch/zx/nirvanap|arch/zx/nirvana+.h]] - NIRVANA+ Multicolour engine
      * [[libnew:arch/zx/nirvanam|arch/zx/nirvana-.h]] - NIRVANA- Multicolour engine
      * [[libnew:games/sp1|arch/zx/sp1.h]] - SP1 software sprite engine for zx (games/sp1.h continues to work)
  * [[libnew:assert|assert.h]] - assert() macro
  * [[libnew:compress|compress.h]] - a collection of data compression tools
    * [[libnew:compress/aplib|compress/aplib.h]] - aPLib decompression 
    * [[libnew:compress/zx7|compress/zx7.h]] - optimal lz77 decompression
  * [[libnew:ctype|ctype.h]] - character classification
  * [[libnew:errno|errno.h]] - errno definitions
  * [[libnew:fcntl|fcntl.h]] - open posix level file desciptor
  * [[libnew:float|float.h]] - floating point characteristics
  * [[libnew:font|FONTS]]
    * [[libnew:font/fzx|font/fzx.h]] - fzx proportional fonts for bitmapped displays
  * [[libnew:games|GAMES]]
    * [[libnew:games/sp1|games/sp1.h]] - sp1 software sprite engine for bitmapped displays
  * [[libnew:input|input.h]] - direct interface with user-input devices (keyboard, joystick, mouse)
  * [[libnew:intrinsic|intrinsic.h]] - inline z80 instructions without impeding the optimizer
  * [[libnew:inttypes|inttypes.h]] - format conversion of integer types
  * [[libnew:iso646|iso646.h]] - alternative spellings for operators
  * [[libnew:limits|limits.h]] - sizes of integer types
  * [[libnew:alloc/malloc|malloc.h]] - standard C memory allocation
  * [[libnew:alloc/obstack|obstack.h]] - gnu obstack stack allocation
  * [[libnew:math|math.h]] - floating point math functions
  * [[libnew:rect|rect.h]] - points, intervals and rectangles
  * [[libnew:setjmp|setjmp.h]] - non-local jumps
  * [[libnew:sound|sound.h]] - a collection of audio functions
    * [[libnew:sound/bit|sound/bit.h]] - audio generation functions using a 1-bit device
  * [[libnew:stdalign|stdalign.h]] - alignment
  * [[libnew:stdarg|stdarg.h]] - variable arguments
  * [[libnew:stdbool|stdbool.h]] - boolean type
  * [[libnew:stddef|stddef.h]] - common definitions
  * [[libnew:stdint|stdint.h]] - standard names of integer types without ambiguity
  * [[libnew:stdio|stdio.h]] - stream input / output
  * [[libnew:stdlib|stdlib.h]] - general utilities (sorting, number<->ascii, random numbers, ...)
  * [[libnew:stdnoreturn|stdnoreturn.h]] - Noreturn
  * [[libnew:string|string.h]] - string and raw memory manipulation
  * [[libnew:stropts|stropts.h]] - ioctl of devices
  * [[libnew:threads|threads.h]] - threads
  * [[libnew:unistd|unistd.h]] - posix level input / output on file descriptors
  * [[libnew:z80|z80.h]] - z80 related functions (precise delay, interrupts, port i/o ...)