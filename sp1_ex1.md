# SP1_EX1.C

 | {{libnew:examples:sp1_ex1.png?316 | sp1_ex1}} | {{libnew:examples:sp1_ex1_2.png?316 | sp1_ex1_2}} | {{libnew:examples:sp1_ex1_bonus.png?315 | sp1_ex1_bonus}} | 
 | --------------------------------- | --------- | ----------------------------------- | ----------- | --------------------------------------- | --------------- | 

[SP1](SP1) is a software sprite engine for targets with bitmapped displays.  It is intended to be portable but at the moment it is only available on a limited number of targets, one of them being the zx spectrum.

This example is a simple one that creates six sprites on screen and moves them around at different speeds, changing direction when they hit the sides of the screen.  It is meant to show how to initialize and set up the sp1 library as well as how to customize it and reduce binary size.  We concentrate on compilation using sdcc as support for sdcc is new but the same source code can be compiled using sccz80 and the necessary compile line is listed as a comment at the top of the source code.

The original source code that follows should be enough to understand how to compile using sp1 with the new c library.  The following discussion quickly enters some more advanced topics.

## Original Source Code

The source code is split between two files.  One contains the C source code while the other contains assembly code defining the sprite graphics.  The complete separation of asm code and C code is the preferred way to organize source code for both practical and [technical reasons](technical reasons).

**File: "sp1_ex1.c"**

	:::c
 */*////////////////////////////////////////////////////////
	// EXAMPLE PROGRAM #1
	// 04.2006, 09.2015 aralbrec
	//
	// Six software-rotated masked sprites move in straight lines
	// at various speeds, bouncing off screen edges.  All six
	// share the same sprite graphic which is not animated in
	// this first test program.
 */*////////////////////////////////////////////////////////
	
	// One of:
	// zcc +zx -vn -clib=new -startup=31 sp1_ex1.c gr_window.asm -o sp1_ex1 -create-app
	// zcc +zx -vn -SO3 -clib=sdcc_ix -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm -o sp1_ex1 -create-app
	// zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm -o sp1_ex1 -create-app
	
	#include `<arch/zx/sp1.h>`
	#include `<arch/zx.h>`
	#include `<intrinsic.h>`
	
	#pragma output REGISTER_SP           = 0xd000    // place stack at $d000 at startup
	
	#pragma output CRT_ENABLE_RESTART    = 1         // not returning to basic
	#pragma output CRT_ENABLE_CLOSE      = 0         // do not close files on exit
	
	#pragma output CLIB_EXIT_STACK_SIZE  = 0         // no exit stack
	
	#pragma output CLIB_MALLOC_HEAP_SIZE = 5000      // malloc heap size (not sure what is needed exactly)
	#pragma output CLIB_STDIO_HEAP_SIZE  = 0         // no memory needed to create file descriptors
	
	#pragma output CLIB_FOPEN_MAX        = -1        // no allocated FILE structures, -1 for no file lists too
	#pragma output CLIB_OPEN_MAX         = 0         // no fd table
	
	// Clipping Rectangle for Sprites
	
	struct sp1_Rect cr = { 0, 0, 32, 24 };           // rectangle covering the full screen
	
	// Table Holding Movement Data for Each Sprite
	
	struct sprentry
	{
	   struct sp1_ss  *s;                            // sprite handle returned by sp1_CreateSpr()
	   char           dx;                            // signed horizontal speed in pixels
	   char           dy;                            // signed vertical speed in pixels
	};
	
	struct sprentry sprtbl[] =
	{
	   {0,1,0}, {0,0,1}, {0,1,2}, {0,2,1}, {0,1,3},
	   {0,3,1}, {0,2,3}, {0,3,2}, {0,1,1}, {0,2,2}
	};
	
	// A Hashed UDG for Background
	
	unsigned char hash[] = { 0x55,0xaa,0x55,0xaa,0x55,0xaa,0x55,0xaa };
	
	// Attach C Variable to Sprite Graphics Declared in ASM in Another File
	
	extern unsigned char gr_window[];      // gr_window will hold the address of the asm label _gr_window
	
	main()
	{
	   static struct sp1_ss *s;
	   static unsigned char i;
	   static struct sprentry *se;
	   
	   intrinsic_di();   // inline di instruction without impeding the optimizer
	
	   // Initialize SP1.LIB
	   
	   zx_border(INK_BLACK);
	   
	   sp1_Initialize(SP1_IFLAG_MAKE_ROTTBL | SP1_IFLAG_OVERWRITE_TILES | SP1_IFLAG_OVERWRITE_DFILE, INK_BLACK | PAPER_WHITE, ' ');
	   sp1_TileEntry(' ', hash);   // redefine graphic associated with space character
	
	   sp1_Invalidate(&cr);        // invalidate entire screen so that it is all initially drawn
	   sp1_UpdateNow();            // draw screen area managed by sp1 now
	   
	   // Create Six Masked Software-Rotated Sprites
	   
	   for (i=0; i!=6; ++i)
	   {
	      s = sprtbl[i].s = sp1_CreateSpr(SP1_DRAW_MASK2LB, SP1_TYPE_2BYTE, 3, 0, i);
	      sp1_AddColSpr(s, SP1_DRAW_MASK2, 0, 48, i);
	      sp1_AddColSpr(s, SP1_DRAW_MASK2RB, 0, 0, i);
	      sp1_MoveSprAbs(s, &cr, gr_window, 10, 14, 0, 4);
	   };
	   
	   while (1)
	   {
	      sp1_UpdateNow();                          // draw screen now
	      
	      for (i=0; i!=6; ++i)
	      {                                         // move all sprites
	         se = & sprtbl[i];
	         
	         sp1_MoveSprRel(se->s, &cr, 0, 0, 0, se->dy, se->dx);
	         
	         if (se->s->row > 21)                   // if sprite went off screen, reverse direction
	            se->dy = - se->dy;
	            
	         if (se->s->col > 29)                   // notice if coord moves less than 0, it becomes
	            se->dx = - se->dx;                  //   255 which is also caught by these cases
	      }
	   }
	}


**File: "gr_window.asm"**

	
	SECTION rodata_user
	
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	
	; ASM source file created by SevenuP v1.20
	; SevenuP (C) Copyright 2002-2006 by Jaime Tejedor Gomez, aka Metalbrain
	
	;GRAPHIC DATA:
	;Pixel Size:      ( 16,  24)
	;Char Size:       (  2,   3)
	;Sort Priorities: Mask, Char line, Y char, X char
	;Data Outputted:  Gfx
	;Interleave:      Sprite
	;Mask:            Yes, before graphic
	
	PUBLIC _gr_window
	
	_gr_window:
	
		DEFB	128,127,  0,192,  0,191, 30,161
		DEFB	 30,161, 30,161, 30,161,  0,191
		DEFB	  0,191, 30,161, 30,161, 30,161
		DEFB	 30,161,  0,191,  0,192,128,127
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,255,  0
		
		DEFB	  1,254,  0,  3,  0,253,120,133
		DEFB	120,133,120,133,120,133,  0,253
		DEFB	  0,253,120,133,120,133,120,133
		DEFB	120,133,  0,253,  0,  3,  1,254
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,255,  0


If you are familiar with the use of sp1 with the classic C library you will note one main difference: sp1 has been changed to implicitly use malloc for its memory allocations.  That means the special sp1 hooks to allocate and free memory are no longer required.  Further, the new C library automatically initializes malloc's heap in the CRT so heap initialization code is no longer required.

The program can be compiled using any of the compile lines listed at the top of the source code.  We will use this one for explanatory purposes:

	
	zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm -o sp1_ex1 -create-app


The output is a tap file "sp1_ex1.tap".


*  **-clib=sdcc_iy** selects sdcc as compiler and chooses the IY version of the library which grants ownership of IX to sdcc for its frame pointer and ownership of IY to the C library.  Selection of sdcc_iy also automatically adds "--reserve-regs-iy" to the compile line so that sdcc is not allowed to use IY.  In this arrangement the library does not have to preserve the IX register in library calls (it uses IY) and this leads to smaller code in sdcc compiles.  On the zx target, the penalty for using IY is that the ROM's interrupt routine must not be run.  The ROM isr uses IY to index into the system variables area so if it runs it will be poking around in random memory locations eventually causing a mysterious crash.  So the first thing main() does is disable interrupts.  This penalty is not really a penalty at all for a couple of reasons: (1) The ROM isr does not do anything useful for a C program so having it run only wastes CPU cycles and (2) the sp1 library has some functions using both IX and IY so it's not compatible with the ROM isr anyway.


*  [-SO3](temp/front#sdcc2) enables some aggressive peephole optimizer rules.  In this short program it manages to save around 40 bytes in comparison to sdcc's built-in rules.


*  **-startup=31** selects a bare-bones crt that does not instantiate any drivers on stdin, stdout, stderr.  This means there will be no terminals available and printf/scanf cannot be used.  The sprintf/sscanf families can still be used to operate on memory buffers if needed; the stdio size will be much smaller since none of the terminal code will be attached to the binary.  Smaller alternatives to sprintf/sscanf may be found among the [stdlib functions](libnew/stdlib).  If the program does need terminal i/o, another [suitable startup](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/zx/zx_crt.asm.m4) can be chosen.

Both files are listed on the compile line.  [zcc will compile both files to object files](temp/front#sdcc3) and then link them together with z80asm resolving any external labels.  In the C source, there is one external label in "extern unsigned char gr_window[];" which will be translated to "EXTERN _gr_window" by the compiler (note the name mangling with leading underscore) and this will be resolved to the same memory address as the publicly exported symbol "_gr_window" defined in the asm file.  This is how the [C source grabs an address in the asm source](https://www.z88dk.org/wiki/doku.php?id=libnew/target_embedded#mixing_c_and_assembly_language).

Some [CRT configuration](https://www.z88dk.org/wiki/doku.php?id=libnew/target_embedded#crt_configuration) is specified via pragmas in the source code.  The CRT (**C** **R**un**t**ime) is the short bit of code that sets up the C environment prior to calling main().  The CRT defaults specified for any target are intended to define a program environment expected by the C standard.  However not all programs require a standard conforming environment so the pragmas allow a program to eliminate unneeded elements to reduce program size further.

To understand the pragmas listed we need to look at what the defaults are for the zx spectrum RAM target:

 | CRT OPTION                | compile time setting | meaning                                                                                 | 
 | ----------                | -------------------- | -------                                                                                 | 
 | CRT_ORG_CODE              | 32768                | compile to address 32768                                                                | 
 | CRT_ORG_DATA              | 0                    | append DATA section to CODE section                                                     | 
 | CRT_ORG_BSS               | 0                    | append BSS section to DATA section                                                      | 
 | CRT_MODEL                 | 0                    | RAM model :- no initialization of DATA or BSS, can run once with correct initialization | 
 | CRT_INITIALIZE_BSS        | 0                    | no code to zero BSS in CRT                                                              | 
 | REGISTER_SP               | -1                   | do not modify stack pointer                                                             | 
 | CRT_ENABLE_COMMANDLINE    | 0                    | no command line: argc and argv not available                                            | 
 | CRT_ENABLE_RESTART        | 0                    | program exits to host                                                                   | 
 | CRT_ENABLE_CLOSE          | 1                    | close files on exit                                                                     | 
 | CRT_ENABLE_RST            | 0                    | z80 restart locations not linked to user code                                           | 
 | CRT_ENABLE_NMI            | 0                    | z80 nmi not linked to user code                                                         | 
 | CLIB_EXIT_STACK_SIZE      | 2                    | atexit() can register two functions                                                     | 
 | CLIB_QUICKEXIT_STACK_SIZE | 0                    | at_quick_exit() cannot register any functions                                           | 
 | CLIB_MALLOC_HEAP_SIZE     | -1                   | crt will create a heap occupying space between BSS and bottom of stack                  | 
 | CLIB_STDIO_HEAP_SIZE      | 256                  | stdio's heap is 256 bytes (for opening files)                                           | 
 | CLIB_BALLOC_TABLE_SIZE    | 0                    | block memory allocator's queue table not allocated                                      | 
 | CLIB_FOPEN_MAX            | 0                    | max number of open FILE* (stdin,stdout,stderr will be present if demanded)              | 
 | CLIB_OPEN_MAX             | 0                    | max number of open files (0,1,2 will be present if demanded)                            | 

The changes implemented by the pragmas:

 | #pragma output REGISTER_SP           = 53248  | CRT will set the stack pointer to $d000 (sp1 suggestion, below)  |       
 | ------------------------------------------------------------------------------------------------------------------       
 | #pragma output CRT_ENABLE_RESTART    = 1      | CRT will restart the program on exit (no need to save state for basic)  |
 | #pragma output CRT_ENABLE_CLOSE      = 0      | Do not close files on exit (there are none!)  |                          
 | #pragma output CLIB_EXIT_STACK_SIZE  = 0      | Eliminate the exit stack  |                                              
 | #pragma output CLIB_MALLOC_HEAP_SIZE = 5000   | Set malloc heap size to 5000 bytes (sp1 allocates from the heap)  |      
 | #pragma output CLIB_STDIO_HEAP_SIZE  = 0      | Eliminate the stdio heap  |                                              
 | #pragma output CLIB_FOPEN_MAX        = -1     | No FILE structures and -1 means no FILE lists either  |                  
 | #pragma output CLIB_OPEN_MAX         = 0      | No file descriptor table  |                                              

These pragmas are designed to reduce the binary size as much as possible.  The two exceptions are **REGISTER_SP** which is used to move the stack pointer to a location [suggested by sp1](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/zx/config_sp1.m4#L35) and **CLIB_MALLOC_HEAP_SIZE** which creates a heap large enough to satisfy sp1's needs while creating sprites.  The exact amount of memory needed was not calculated -- a chunk was thrown at it but this will be examined below.  Note that the **CRT_ENABLE_RESTART** option will cause the CRT to restart the program on exit but if this occurs, the second run will not see the C variables properly initialized and the program may not function properly.  To have the program's variables initialized properly on restart, a [ROM model](https://www.z88dk.org/wiki/doku.php?id=libnew/target_embedded#understanding_the_output_files_generated_by_the_compiler) compile should be used.  Here the restart option is only selected to so that the CRT does not have to insert code to return to basic safely.

With these settings, the compile comes to 8631 bytes which does not include the [top part of memory reserved by sp1](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/zx/config_sp1.m4#L35) for its data structures.  We can find out what is taking up that memory by asking z80asm to generate a separate binary for each section defined in the program.  Because the new C library defines finely grained sections for library code, it is easy to see at a glance what is using up the memory.  The necessary commandline flag is "-Cl--split-bin"

	
	zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm -o sp1_ex1 -Cl--split-bin


A following dir listed this:

 | 0     | sp1_ex1                             | non-zero size indicates code or data was not assigned to a section (this is an error!) | 
 | -     | -------                             | -------------------------------------------------------------------------------------- | 
 | 1     | sp1_ex1_BSS.bin                     | dummy byte to ensure BSS will be the name of a separate BSS binary                     | 
 | 5,000 | sp1_ex1_bss_alloc_malloc.bin        | space for 5000 byte heap                                                               | 
 | 5     | sp1_ex1_bss_compiler.bin            | C program's initially zero global variables                                            | 
 | 2     | sp1_ex1_bss_error.bin               | errno                                                                                  | 
 | 2     | sp1_ex1_bss_stdlib.bin              | ?                                                                                      | 
 | 2     | sp1_ex1_BSS_UNINITIALIZED.bin       | CRT saves stack pointer                                                                | 
 | 7     | sp1_ex1_CODE.bin                    | part of CRT                                                                            | 
 | 193   | sp1_ex1_code_alloc_malloc.bin       | malloc, free related functions                                                         | 
 | 16    | sp1_ex1_code_arch.bin               | zx_border and other zx specific functions                                              | 
 | 330   | sp1_ex1_code_compiler.bin           | C code generated by compiler                                                           | 
 | 9     | sp1_ex1_code_crt_init.bin           | part of CRT : user, library, compiler initialization code (call to heap_init here)     | 
 | 3     | sp1_ex1_code_crt_main.bin           | part of CRT                                                                            | 
 | 7     | sp1_ex1_code_crt_return.bin         | part of CRT                                                                            | 
 | 56    | sp1_ex1_code_error.bin              | code that sets errno and returns common values                                         | 
 | 35    | sp1_ex1_code_l.bin                  | library primitives : zero a structure, complement a long, etc                          | 
 | 2,748 | sp1_ex1_code_temp_sp1.bin           | sp1 code                                                                               | 
 | 31    | sp1_ex1_code_threads_mutex.bin      | mutex : the library is preparing for multi-threading so this code is present           | 
 | 1     | sp1_ex1_DATA.bin                    | dummy byte to ensure DATA will be the name of a separate DATA binary                   | 
 | 2     | sp1_ex1_data_alloc_malloc.bin       | address of malloc heap                                                                 | 
 | 52    | sp1_ex1_data_compiler.bin           | C program's initially non-zero global variables                                        | 
 | 1     | sp1_ex1_data_sound_bit.bin          | records state of speaker and border colour                                             | 
 | 1     | sp1_ex1_data_threads.bin            | thread id                                                                              | 
 | 16    | sp1_ex1_rodata_error_strings.bin    | errno strings                                                                          | 
 | 1     | sp1_ex1_rodata_error_string_end.bin | end of errno strings is 0 terminated                                                   | 
 | 110   | sp1_ex1_rodata_user.bin             | sprite graphics defined in "gr_window.asm"                                             | 
 | 1,071 | zcc_opt.def                         | pragmas found in program - does not add to program size                                | 

The main components of the 8631-byte program are the 5000-byte heap and the 2748 bytes for sp1's sprite functions.  The CRT startup code is reduced to just 26 bytes.  It may come as a surprise that the actual C code only occupies 330 bytes of the total.  With much of the required library code already included, if the program grows it is mainly this 330-byte total that will grow.

It's usually a good idea to construct a memory map to understand how things are laid out in memory.  This is how the program is currently laid out:

	
	; ADDRESS (HEX)   LIBRARY  DESCRIPTION
	;
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array
	; d200 - efff     SP1.LIB  update array for full size screen 32x24
	; d1ff - d1ff     SP1.LIB  attribute buffer
	; d1f7 - d1fe     SP1.LIB  pixel buffer
	; d1ed - d1f6     SP1.LIB  update list head - a dummy struct sp1_update acting as first in invalidated list
	;  * d1ef - d1f0  SP1.LIB  update list tail pointer (inside dummy struct sp1_update)
	; d1d4 - d1ec     --free-  25 bytes free
	; d1d1 - d1d3     -------  JP to im2 service routine (im2 table filled with 0xd1 bytes)
	; d101 - d1d0     --free-  208 bytes
	; d000 - d100     IM2.LIB  im 2 vector table (257 bytes)
	; ce00 - cfff     -------  z80 stack (512 bytes) set SP=d000
	; a1b7 - cdff     --free-  11337 bytes
	; 8000 - a1b6     C.PROGR  8631 bytes for the C program


The top portion of memory, from $ce00 and up, includes the default sp1 data structures and its suggestions for stack location and im2 vector table.  The latter is unused by this program and could be reclaimed as free memory by moving the stack further up in memory.

The suggested size of the stack itself (512 bytes) is also rather large; less than 256 bytes is probably more in line with what is required for most programs.  Stack space is occupied by parameters and local variables during function calls and is used by the library if it needs to allocate temporary workspace.  The maximum amount needed depends very much on how the program is structured.  If memory is in desperate shortage people will sometimes fill the stack area with $ff bytes, let the program run a while, then examine the stack area to see what bytes were modified to gain a more accurate read on how much memory the stack really needs.  This simple program can likely get away with a stack size less than 64 bytes.

## Changing the Memory Map

Z88dk gives the program very precise control over placement of data structures and code in memory.  The C compilers are lagging somewhat in their syntax such that there isn't complete control over placement (yet) but anything can be done with a little help from assembly language.

When writing games on a small uP, available memory is always a challenge.  You want to increase the amount of linear space for the compiler to generate code into and place data at available spots in memory to maximum RAM usage.  In this specific example, the 5000-byte heap is being added to the end of the CODE segment which reduces the amount of memory available for the compiler code to grow into.  So a first step in easing the memory problem is to place the heap somewhere else.

In the current memory map, code is being generated at address 32768 which is a popular location because it is the start address of uncontended memory (memory that is not shared with the display logic and therefore the z80 is not slowed while accessing it).  But the zx spectrum memory map has available RAM immediately following the display file starting at address 23296 which should not be wasted in tight memory conditions.  It can be a good place to store variables and other data.  The difficulty with placing data there is that it interferes with the Basic operating system and don't forget that Basic is used to load the compiled program into memory; having Basic load data there will clobber its memory space, causing it to crash before the program is started.  So what we want to place there is something that is initialized by the C program and doesn't need to be loaded prior to program start.  The heap is a candidate.

To move the heap down there we'll first tell the CRT not to make the heap by changing one of the pragmas:

	:::c
	#pragma output CLIB_MALLOC_HEAP_SIZE = 0         // do not create malloc's heap


If the program were compiled in this state, z80asm would complain that "%%__malloc_heap%%" was not found; this is the name of the heap used by malloc and related functions.  Instead we will allocate memory for the heap ourselves in a new SECTION org'd at address 23296.  Call this section LOWMEM to indicate it immediately follows the display file.  "%%__malloc_heap%%" must hold the address of this memory block and since this memory address is non-zero it needs to be placed in a data section.  The CRT should also initialize the heap so that it doesn't have to be done in the C program.  These things are done in the following code snippet.

**File: "heap.asm"**

	
	;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
	
	SECTION LOWMEM
	org 23296
	
	; allocate memory for the user heap
	
	defc __heap_size = 5000
	
	__malloc_block:  defs __heap_size
	
	;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
	
	SECTION data_user
	
	; place malloc pointer in data section
	
	PUBLIC __malloc_heap
	
	__malloc_heap:   defw __malloc_block
	
	;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
	
	SECTION code_crt_init
	
	; initialize the heap in the crt
	
	   ld hl,__malloc_block
	   ld bc,__heap_size
	   
	   EXTERN asm_heap_init
	   call asm_heap_init
	
	;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;


Since those fragments are in the same file all asm labels are locally visible and nothing needs to be exported unless it needs to be seen by the rest of the program.  "%%__malloc_heap%%" needs to be PUBLIC so that the malloc family of functions can find the heap.  "asm_heap_init" needs to be EXTERN because it is supplied externally by the library.

The SECTIONs determine where in memory the following code and data will be placed by the linker.  SECTION "data_user" is defined by the crt so that anything assigned here will be part of the main executable.  SECTION "code_crt_init" is defined by the crt as a place to put code that is executed just prior to calling main().  The heap initialization is placed there so that the heap is properly initialized before main runs.  This section will be part of the main executable. SECTION "LOWMEM" is not known to the crt.  The linker will emit a separate binary containing its contents.

Try another compile, remembering to add "heap.asm" to the list of files.

	
	zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm heap.asm -o sp1_ex1 -create-app


The output is:

 | 0     | sp1_ex1            | non-zero size indicates code or data was not assigned to a section (this is an error!) | 
 | -     | -------            | -------------------------------------------------------------------------------------- | 
 | 3,631 | sp1_ex1_CODE.bin   | the executable with ORG 32768                                                          | 
 | 5,000 | sp1_ex1_LOWMEM.bin | the 5000-byte heap with ORG 23296                                                      | 

The program that needs to be loaded into memory is the 3631-byte "sp1_ex1_CODE.bin".  "sp1_ex1_LOWMEM.bin" is thrown away -- it doesn't need to be loaded into memory because the program initializes it.  Load the smaller binary and verify that the program still works (the file will not contain the LOWMEM section but you can also load the binary directly to 32768 and execute it with RAND USR 32768).  The only difference is that the heap has been moved to address 23296 rather than being attached to the executable.

The memory map now looks like:

	
	; ADDRESS (HEX)   LIBRARY  DESCRIPTION
	;
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array
	; d200 - efff     SP1.LIB  update array for full size screen 32x24
	; d1ff - d1ff     SP1.LIB  attribute buffer
	; d1f7 - d1fe     SP1.LIB  pixel buffer
	; d1ed - d1f6     SP1.LIB  update list head - a dummy struct sp1_update acting as first in invalidated list
	;  * d1ef - d1f0  SP1.LIB  update list tail pointer (inside dummy struct sp1_update)
	; d1d4 - d1ec     --free-  25 bytes free
	; d1d1 - d1d3     -------  JP to im2 service routine (im2 table filled with 0xd1 bytes)
	; d101 - d1d0     --free-  208 bytes
	; d000 - d100     IM2.LIB  im 2 vector table (257 bytes)
	; ce00 - cfff     -------  z80 stack (512 bytes) set SP=d000
	; 8e2f - cdff     --free-  16337 bytes
	; 8000 - 8e2e     C.PROGR  3631 bytes for the C program
	; 6e88 - 7fff     --free-  4472 bytes
	; 5b00 - 6e87     LOWMEM-  5000 bytes for heap


By moving the heap down to 23296 ($5b00), space for the C program to expand has increased from 11337 bytes to 16337 bytes.

The next question is how big does the heap actually need to be?  5000-bytes was just thrown at it as a number that should be well over what was required.  There are three ways to figure out what the minimum size of the heap needs to be:


*  Reduce the heap size until the program cannot allocate memory.  If allocation failures are not tested for in the program (this means a return of 0 from malloc or the sprite create functions) the program will crash or show abberant behaviour.


*  Use [heap_info()](libnew/alloc_malloc#named_heap_-_information) to find out what the largest available memory block is.  That would indicate the amount of slack in the heap.


*  Calculate the amount of memory needed.

Keep in mind that if memory is frequently allocated and freed, the heap size needs to be larger than the sum of allocated memory because such actions fragment the heap over time.  In such situations the heap should be cleared (re-initialized) at prudent points in the program, for example between levels being loaded in a game.  But in this program, nothing is freed and sprites are simply allocated one after the other so there will be no holes in the heap.

Normally calculation of the amount of memory needed is not possible but it is doable for this program.  Each sprite allocates one [struct sp1_ss](https://github.com/z88dk/z88dk/blob/master/include/_DEVELOPMENT/clang/arch/zx/sp1.h#L56) (20 bytes) and a [struct sp1_cs](https://github.com/z88dk/z88dk/blob/master/include/_DEVELOPMENT/clang/arch/zx/sp1.h#L83) (24 bytes) for each character cell in the sprite.  Each memory allocation incurs six bytes of overhead.  In this program, six sprites of size 3x3 characters are created so the memory requirement is *%%6*((20+6) + 3*3*(24+6)) = 1776%%* bytes.  In addition to this the heap will contain a mutex so a little extra is required for that.  Maybe allocating 1800 bytes for the heap as opposed to 5000 is a nice round number.

## C is Assembly Language

Reading through this topic you should have the impression that these are very low level topics.  And they are -- in fact C is a very low level language that maps almost directly to assembly language.  The concepts of pointers, so called "unsafe" casting, adding and subtracting register-size integers, are all exactly assembly language concepts.  The only difference between the two is the compiler is worrying about register allocation and memory allocation for you.  But in resource constrained situations such as speed critical code (register allocation is done by the programmer directly by supplying asm code) and low memory situations (the programmer takes charge of the memory map) the programmer becomes involved in details.  The z80 is a very resource challenged platform and in z88dk the speed critical issue is solved by supplying asm library code whereas the memory situation (when it is a problem) is helped by the programmer creating sections and assigning code and data appropriately.

Before leaving this topic, it shouldn't be underestimated how important it is to keep track of the program's memory map when memory is tight.  Memory considerations should enter into the program design as well.  You may want to store compressed data for individual levels and only uncompress a current level into a live workspace.  Maybe that compressed level data is stored at address 23296.  Maybe level data is stored in the 128k's extra memory banks and is decompressed into a live workspace at 23296.  One of the main intentions of discussing SECTIONs here was to demonstrate how to create separate binary blocks that can be loaded into memory at appropriate addresses.  In the case of the 128k spectrum this could mean one section created per memory bank and all org'd at address $c000.  The compiler will not automatically bank for you so it is still the programmer's responsibility to page in the appropriate memory bank before using or copying data stored there.  (The spectrum target defines sections "BANK_00" through "BANK_07" all ORG'd to address 0xc000 for use by 128k spectrum programs).

## SP1 Library Configuration

The main C library can also be [configured](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/zx/config_clib.m4) but its default settings pretty much select smallest library code.  Perhaps the error strings could be eliminated to save a few bytes and if your program does use stdio be sure to [opt out](https://www.z88dk.org/wiki/doku.php?id=libnew/target_embedded#printf_and_scanf_configuration_the_easy_way) of any printf and scanf converters the program does not use.

The configuration of sp1 is a topic that is generally not well understood or it seems that way since no one does it.  SP1 is a memory hog -- the choices made in its implementation opted for speed and ease of use, both of which conspire to require large data structures in proportion to the display size.  The latter is the key -- to save on memory used by sp1 the main point of attack will be to reduce the screen area managed by sp1.  Most games only use a part of the full display for the action so it seems unnecessary that sp1 should be managing the full size display in all cases.

SP1's configuration is local to a target and is contained in the target's "config_sp.m4" file.  For the zx spectrum this file is in [{z88dk}/libsrc/_DEVELOPMENT/target/zx/config_sp1.m4](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/zx/config_sp1.m4).  "config_sp1.m4.bak" is a backup that can be used to restore defaults.

The portion configuring SP1 is this:

	
	;--------------------------------------------------------------
	;-- GAMES/SP1 -------------------------------------------------
	;--------------------------------------------------------------
	
	; display characteristics
	
	defc SP1V_DISPORIGX     = 0            ; x coordinate of top left corner of area managed by sp1 in characters
	defc SP1V_DISPORIGY     = 0            ; y coordinate of top left corner of area managed by sp1 in characters
	defc SP1V_DISPWIDTH     = 32           ; width of area managed by sp1 in characters (16, 24, 32 ok as of now)
	defc SP1V_DISPHEIGHT    = 24           ; height of area managed by sp1 in characters
	
	; buffers
	
	defc SP1V_PIXELBUFFER   = $d1f7        ; address of an 8-byte buffer to hold intermediate pixel-draw results
	defc SP1V_ATTRBUFFER    = $d1ff        ; address of a single byte buffer to hold intermediate colour-draw results
	
	; data structure locations
	
	defc SP1V_TILEARRAY     = $f000        ; address of the 512-byte tile array associating character codes with tile graphics, must lie on 256-byte boundary (LSB=0)
	defc SP1V_UPDATEARRAY   = $d200        ; address of the 10*SP1V_DISPWIDTH*SP1V_DISPHEIGHT byte update array
	defc SP1V_ROTTBL        = $f000        ; location of the 3584-byte rotation table.  Must lie on 256-byte boundary (LSB=0).  Table begins $0200 bytes ahead of this
	                                       ;  pointer ($f200-$ffff in this default case).  Set to $0000 if the table is not needed (if, for example, all sprites
	                                       ;  are drawn at exact horizontal character coordinates or you use pre-shifted sprites only).
	; sp1 variables
	
	defc SP1V_UPDATELISTH   = $d1ed        ; address of 10-byte area holding a dummy struct_sp1_update that is always the "first" in list of screen tiles to be drawn
	defc SP1V_UPDATELISTT   = $d1ef        ; address of 2-byte variable holding the address of the last struct_sp1_update in list of screen tiles to be drawn
	
	; note: SP1V_UPDATELISTT is located inside the dummy struct_sp1_update pointed at by SP1V_UPDATELISTH
	
	; default memory map
	
	; With these default settings the memory map is:
	;
	; ADDRESS (HEX)   LIBRARY  DESCRIPTION
	;
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array
	; d200 - efff     SP1.LIB  update array for full size screen 32x24
	; d1ff - d1ff     SP1.LIB  attribute buffer
	; d1f7 - d1fe     SP1.LIB  pixel buffer
	; d1ed - d1f6     SP1.LIB  update list head - a dummy struct sp1_update acting as first in invalidated list
	;  * d1ef - d1f0  SP1.LIB  update list tail pointer (inside dummy struct sp1_update)
	; d1d4 - d1ec     --free-  25 bytes free
	; d1d1 - d1d3     -------  JP to im2 service routine (im2 table filled with 0xd1 bytes)
	; d101 - d1d0     --free-  208 bytes
	; d000 - d100     IM2.LIB  im 2 vector table (257 bytes)
	; ce00 - cfff     -------  z80 stack (512 bytes) set SP=d000


The display surface is measured in characters and by default it is the full 32x24 size.  The vertical size can be anything from 1 to 24 but the horizontal size is limited to 16, 24 and 32.  There is no reason for this other than the library needs to supply fast multiplication code and it only currently contains code for those sizes; if you really need something else you can modify the SP1 code to include more multiply cases.  Keep in mind that this rectangular surface corresponds to SP1's coordinate system but you can move each character cell to different places on the screen so it doesn't have to correspond to a rectangular area on screen.

An appropriate size for the display is going to depend on the game.  Maybe it is full width and has 4 lines reserved at the bottom for a score and 2 lines removed from the top to keep the display vertically balanced on the TV.  In that case 6 lines out of 24 could be removed, a memory savings of 25%.  If the scoreboard is moved to the right side and the left side is kept a full 24 lines tall, there might be a 24x24 surface which would also correspond to a memory savings of 25%.  Since SP1 can only draw onto its surface the question then is how is the scoreboard going to be drawn.  Maybe the program supplies its own code for that but another method is to move some of SP1's display surface to certain areas on the scoreboard so that SP1 can draw on them.

For the sake of argument let's set up a 24x22 surface (24 chars wide, 22 chars tall).  If you look at the memory map above, what is going to change is the size of the update array.  Instead of *10*32*24 = 7680* bytes in size it will be *10*24*22 = 5280* bytes, making its start address *$d200* move upward by *(7680 - 5280) = 2400* bytes to *$db60*.  Everything below it will be moved upward in memory by the same amount.  Keep an eye on that memory map while editing the configuration.

The changes to the config file:

	
	;--------------------------------------------------------------
	;-- GAMES/SP1 -------------------------------------------------
	;--------------------------------------------------------------
	
	; display characteristics
	
	defc SP1V_DISPORIGX     = 1            ; x coordinate of top left corner of area managed by sp1 in characters
	defc SP1V_DISPORIGY     = 1            ; y coordinate of top left corner of area managed by sp1 in characters
	defc SP1V_DISPWIDTH     = 24           ; width of area managed by sp1 in characters (16, 24, 32 ok as of now)
	defc SP1V_DISPHEIGHT    = 22           ; height of area managed by sp1 in characters
	
	; buffers
	
	defc SP1V_PIXELBUFFER   = $d1f7+2400   ; address of an 8-byte buffer to hold intermediate pixel-draw results
	defc SP1V_ATTRBUFFER    = $d1ff+2400   ; address of a single byte buffer to hold intermediate colour-draw results
	; data structure locations
	
	defc SP1V_TILEARRAY     = $f000        ; address of the 512-byte tile array associating character codes with tile graphics, must lie on 256-byte boundary (LSB=0)
	defc SP1V_UPDATEARRAY   = $d200+2400   ; address of the 10*SP1V_DISPWIDTH*SP1V_DISPHEIGHT byte update array
	defc SP1V_ROTTBL        = $f000        ; location of the 3584-byte rotation table.  Must lie on 256-byte boundary (LSB=0).  Table begins $0200 bytes ahead of this
	                                       ;  pointer ($f200-$ffff in this default case).  Set to $0000 if the table is not needed (if, for example, all sprites
	                                       ;  are drawn at exact horizontal character coordinates or you use pre-shifted sprites only).
	; sp1 variables
	
	defc SP1V_UPDATELISTH   = $d1ed+2400   ; address of 10-byte area holding a dummy struct_sp1_update that is always the "first" in list of screen tiles to be drawn
	defc SP1V_UPDATELISTT   = $d1ef+2400   ; address of 2-byte variable holding the address of the last struct_sp1_update in list of screen tiles to be drawn
	
	; note: SP1V_UPDATELISTT is located inside the dummy struct_sp1_update pointed at by SP1V_UPDATELISTH


This will change the memory map to:

	
	; ADDRESS (HEX)   LIBRARY  DESCRIPTION
	;
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array
	; db60 - efff     SP1.LIB  update array for full size screen 24x22
	; db5f - db5f     SP1.LIB  attribute buffer
	; db57 - db5e     SP1.LIB  pixel buffer
	; db4d - db56     SP1.LIB  update list head - a dummy struct sp1_update acting as first in invalidated list
	;  * ???? - ????  SP1.LIB  update list tail pointer (inside dummy struct sp1_update)
	; d1d4 - db4c     --free-  2425 bytes free
	; d1d1 - d1d3     -------  JP to im2 service routine (im2 table filled with 0xd1 bytes)
	; d101 - d1d0     --free-  208 bytes
	; d000 - d100     IM2.LIB  im 2 vector table (257 bytes)
	; ce00 - cfff     -------  z80 stack (512 bytes) set SP=d000


This opens up 2400 bytes at address $d1d4.  The im2 table and stack can be rearranged like so:

	
	; ADDRESS (HEX)   LIBRARY  DESCRIPTION
	;
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array
	; db60 - efff     SP1.LIB  update array for full size screen 24x22
	; db5f - db5f     SP1.LIB  attribute buffer
	; db57 - db5e     SP1.LIB  pixel buffer
	; db4d - db56     SP1.LIB  update list head - a dummy struct sp1_update acting as first in invalidated list
	;  * ???? - ????  SP1.LIB  update list tail pointer (inside dummy struct sp1_update)
	; d9dc - db4c     -STACK-  z80 stack (369 bytes) set SP = $db4d
	; d9d9 - d9db     IM2.LIB  JP to im2 service routine (im2 table filled with 0xd9 bytes)
	; d901 - d9d8     --free-  216 bytes free
	; d800 - d900     IM2.LIB  im2 vector table (257 bytes)


The bottom of the area reserved by SP1 has moved from $ce00 to $d800, increasing available memory by 2560 bytes.

To take advantage of this configuration the zx libraries need to be rebuilt.  Do that from the commandline by changing to directory {z88dk}/libsrc/_DEVELOPMENT and running "Winmake zx" (windows) or "make TARGET=zx" (non-windows).

The program will have to be changed a bit.  First the pragmas:

	:::c
	#pragma output REGISTER_SP           = 0xdb4d    // place stack at $db4d at startup
	#pragma output CLIB_MALLOC_HEAP_SIZE = 0         // do not create malloc's heap


The stack is moved to its new location and we keep the moved heap applied in the last section.

The full size screen is now 24 chars wide and 22 chars high so the clipping rectangle will have to be modified:

	:::c
	// Clipping Rectangle for Sprites
	
	struct sp1_Rect cr = { 0, 0, 24, 22 };           // rectangle covering the full screen


In the source code we'll clear the screen to blue so that the area not managed by SP1 will be apparent.

	:::c
	   // Initialize SP1.LIB
	   
	   zx_border(INK_BLACK);
	   zx_cls(INK_BLUE | PAPER_BLUE);
	   
	   sp1_Initialize(SP1_IFLAG_MAKE_ROTTBL | SP1_IFLAG_OVERWRITE_TILES | SP1_IFLAG_OVERWRITE_DFILE, INK_BLACK | PAPER_WHITE, ' ');


The width of the area is 24 characters and the height is 22 so the direction reverse code has to be tweaked:

	:::c
	         if (se->s->row > 19)                   // if sprite went off screen, reverse direction
	            se->dy = - se->dy;
	            
	         if (se->s->col > 21)                   // notice if coord moves less than 0, it becomes
	            se->dx = - se->dx;                  //   255 which is also caught by these cases


Now compile using "heap.asm" as described in the last section:

	
	zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1.c gr_window.asm heap.asm -o sp1_ex1 -create-app


The managed screen size is reduced and the program has an extra 2560 bytes available.  A screenshot can be seen at the top of this page.

The size of the display surface is not the only place where memory can be reclaimed from SP1.  There are two other very large items at the top of the memory map:

	
	; f200 - ffff     SP1.LIB  horizontal rotation tables
	; f000 - f1ff     SP1.LIB  tile array


The horizontal rotation tables occupy 3584 bytes.  These are table lookups used to quickly shift sprite graphics data so that they can be placed with pixel precision on the screen.  The table consists of seven parts each 512 bytes in size corresponding to lookups for sprites shifted right by 1-, 2-, 3-, ..., -7 pixels.  Address range f200-f3ff will be the 1-pixel lookup, f400-f5ff will be the 2-pixel lookup and so on up to fe00-ffff for the 7-pixel lookup.

What would happen if sprites were only moved to even pixel locations horizontally?  (Vertical pixel placement is unaffected by the rotation tables).  All the odd pixel lookups would go unused.  This would correspond to address ranges f200-f3ff (1-pixel), f600-f7ff (3-pixel), fa00-fbff (5-pixel) and fe00-ffff (7-pixel).  In other words those parts of the table become available for other things, corresponding to a memory space of *4*512 = 2048* bytes.  Be aware that the sp1_Initialize() call with SP1_IFLAG_MAKE_ROTTBL flag indicated will create those tables including any unused ones so programs should only use that memory after SP1 is initialized.

The other large segment is the tile array.  The tile array holds pointers to the 8-byte UDG definition corresponding to character codes 0-255.  If your program doesn't use 256 tiles then some of the tile array is unused.  The tile array is actually two 256-byte tables with the LSB of the tile's UDG address stored in the first table and the MSB of the tile's UDG address stored in the second table.

# SP1_EX1_BONUS.C

Be sure the sp1 library configuration is back to defaults (copy from the backup file, rebuild the library with Winmake)

**File: "sp1_ex1_bonus.c"**

	:::c
	// By szeliga @ worldofspectrum.org forums
	
	// zcc +zx -vn -clib=new -startup=31 sp1_ex1_bonus.c graphics.asm collision.asm -o sp1_ex1_bonus
	// zcc +zx -vn -SO3 -clib=sdcc_ix -startup=31 --max-allocs-per-node200000 sp1_ex1_bonus.c graphics.asm collision.asm -o sp1_ex1_bonus
	// zcc +zx -vn -SO3 -clib=sdcc_iy -startup=31 --max-allocs-per-node200000 sp1_ex1_bonus.c graphics.asm collision.asm -o sp1_ex1_bonus
	
	#include `<arch/zx/sp1.h>`
	#include `<arch/zx.h>`
	#include `<input.h>`
	#include `<sound.h>`
	#include `<stdlib.h>`
	
	#pragma output REGISTER_SP           = 53248     // place stack at $d000 at startup
	
	#pragma output CRT_ENABLE_RESTART    = 1         // not returning to basic
	#pragma output CRT_ENABLE_CLOSE      = 0         // do not close files on exit
	
	#pragma output CLIB_EXIT_STACK_SIZE  = 0         // no exit stack
	
	#pragma output CLIB_MALLOC_HEAP_SIZE = 5000      // malloc heap size (not sure what is needed exactly)
	#pragma output CLIB_STDIO_HEAP_SIZE  = 0         // no memory needed to create file descriptors
	
	#pragma output CLIB_FOPEN_MAX        = -1        // no allocated FILE structures, -1 for no file lists too
	#pragma output CLIB_OPEN_MAX         = 0         // no fd table
	
	
	#define MAX_SPRITES  10
	#define DEAD_SPRITE  100
	
	#ifdef __SDCC
	extern int sp1_TestCollision(struct sp1_ss *s1, struct sp1_ss *s2, unsigned int tolerance) __z88dk_callee;
	#endif
	
	#ifdef __SCCZ80
	extern int __CALLEE__ sp1_TestCollision(struct sp1_ss *s1, struct sp1_ss *s2, unsigned int tolerance);
	#endif
	
	// Clipping Rectangle for Sprites
	
	struct sp1_Rect cr = {0, 0, 32, 24};            // rectangle covering the full screen
	
	// Table Holding Movement Data for Each Sprite
	
	struct sprentry
	{
	   struct sp1_ss  	*s;                     // sprite handle returned by sp1_CreateSpr()
	   char           	dx;                     // signed horizontal speed in pixels
	   char           	dy;                     // signed vertical speed in pixels
	   unsigned char        state;                  // frame state (MSS)
	};
	
	struct sprentry sprtbl[] = {
	   {0,1,0,0}, {0,0,1,0}, {0,1,2,0}, {0,2,1,0}, {0,1,3,0},
	   {0,3,1,0}, {0,2,3,0}, {0,3,2,0}, {0,1,1,0}, {0,2,2,0}
	};
	
	// A Hashed UDG for Background
	
	unsigned char hash[] = {127,255,255,255,223,255,251,255};
	
	// Attach C Variable to Sprite Graphics Declared in external ASM File
	
	extern unsigned char sprite01[];      // gr_window will hold the address of the asm label _gr_window
	extern unsigned char sprite02[];      // gr_window will hold the address of the asm label _gr_window
	extern unsigned char sprite03[];      // gr_window will hold the address of the asm label _gr_window
	extern unsigned char sprite04[];      // gr_window will hold the address of the asm label _gr_window
	
	// Callback Function to Colour Sprites Using sp1_IterateSprChar()
	
	unsigned char colour;
	unsigned char cmask;
	
	void colourSpr(unsigned int count, struct sp1_cs *c)
	{
	   c->attr_mask = cmask;
	   c->attr = colour;
	}
	
	main()
	{
	   static unsigned char i, j, deleteSpriteFlag, numLiveSprites;
	   static struct sp1_ss *s;
	   static struct sprentry *se;
	   
	   #asm
	   di
	   #endasm
	
	   // Initialize SP1.LIB
	   
	   zx_border(INK_BLACK);
	   
	   sp1_Initialize(SP1_IFLAG_MAKE_ROTTBL | SP1_IFLAG_OVERWRITE_TILES | SP1_IFLAG_OVERWRITE_DFILE, INK_BLACK | PAPER_WHITE, ' ');
	   sp1_TileEntry(' ', hash);   // redefine graphic associated with space character
	
	   sp1_Invalidate(&cr);        // invalidate entire screen so that it is all initially drawn
	   sp1_UpdateNow();            // draw screen area managed by sp1 now
	   
	   // Create and Colour Sprites
	
	   for (i=0; i!=MAX_SPRITES; ++i)
	   {
	      s = sprtbl[i].s = sp1_CreateSpr(SP1_DRAW_MASK2LB, SP1_TYPE_2BYTE,  3,  0,  i);
	      sp1_AddColSpr(s, SP1_DRAW_MASK2, 0, 48, i);
	      sp1_AddColSpr(s, SP1_DRAW_MASK2RB, 0, 0, i);
	      
	      colour = (i & 0x01) ? (INK_BLACK | PAPER_CYAN) : (INK_BLACK | PAPER_YELLOW);
	      cmask = SP1_AMASK_INK & SP1_AMASK_PAPER;
	      
	      sp1_IterateSprChar(s, colourSpr);
	   }
	
	   // Main Loop
	   
	   while (1)
	   {
	      // Move Sprites to Initial Location
	      
	      for (i=0; i!=MAX_SPRITES; ++i)
	      {
	         sp1_MoveSprAbs(sprtbl[i].s, &cr, sprite01, (rand() & 0x0f) + 4, (rand() & 0x0f) + 8, 0, 4);
	         sprtbl[i].state = 0;
	      }
	      
	      do
	      {
	         sp1_UpdateNow();
	      
	         numLiveSprites = 0;
	      
	         for (i=0; i!=MAX_SPRITES; ++i)
	         {
	            // move all sprites
	         
	            se = &sprtbl[i];
	            
	            if ((j = se->state) != DEAD_SPRITE)
	            {
	               s = se->s;
	               
	               sp1_MoveSprRel(s, &cr, (j `< 10) ? sprite01 : ((j < 20) ? sprite02 : ((j < 30) ? sprite03 : sprite04)), 0, 0, se->`dy, se->dx);
	               se->state = (j > 39) ? 0 : (j + 1);
	               
	               if (s->row > 21) se->dy = - se->dy;           // if sprite went off screen reverse direction
	               if (s->col > 29) se->dx = - se->dx;
	            
	               // check for collision with other sprites
	            
	               deleteSpriteFlag = 0;
	            
	               for (j=i+1; j!=MAX_SPRITES; ++j)
	               {
	                  if ((sprtbl[j].state != DEAD_SPRITE) && (sp1_TestCollision(s, sprtbl[j].s, 2)))    // 2 pixel tolerance
	                  {
	                     // move current 'j' sprite off screen and mark as removed
	                   
	                     sp1_MoveSprAbs(sprtbl[j].s, &cr, 0, 32, 0, 0, 0);
	                     sprtbl[j].state = DEAD_SPRITE;
	
	                     deleteSpriteFlag = 1;
	                  }
	               }
	            
	               if (deleteSpriteFlag)
	               {
	                  bit_beepfx(BEEPFX_ITEM_1);
	                  in_pause(500);                  // pause 500 ms
	               
	                  sp1_MoveSprAbs(s, &cr, 0, 32, 0, 0, 0);
	                  se->state = DEAD_SPRITE;
	               }
	               else
	               {
	                  ++numLiveSprites;
	               }
	            }
	         }
	      } while ((numLiveSprites) && (!in_test_key()));
	
	      // all sprites dead or key pressed
	      
	      zx_border(INK_RED);
	      
	      bit_beepfx(BEEPFX_ALARM_2);
	      in_pause(500);
	      
	      zx_border(INK_BLACK);
	   }
	}


**File: "graphics.asm"**

	
	SECTION rodata_user
	
	PUBLIC _sprite01, _sprite02, _sprite03, _sprite04
	
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	   defb @11111111, @00000000
	
	; ASM source file created by SevenuP v1.20
	; SevenuP (C) Copyright 2002-2006 by Jaime Tejedor Gomez, aka Metalbrain
	
	;GRAPHIC DATA:
	;Pixel Size:      ( 16,  24)
	;Char Size:       (  2,   3)
	;Sort Priorities: Mask, Char line, Y char, X char
	;Data Outputted:  Gfx
	;Interleave:      Sprite
	;Mask:            Yes, before graphic
	
	_sprite01:
	
		DEFB	0,255
		DEFB	  0,192,  0,160,  0,144,  0,136
		DEFB	  0,132,  0,130,  0,129,  0,129
		
		DEFB	  0,130,  0,132,  0,136,  0,144
		DEFB	  0,160,  0,192,  0,255,255,  0
		
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,  0,255
		
		DEFB	  0,  3,  0,  5,  0,  9,  0, 17
		DEFB	  0, 33,  0, 65,  0,129,  0,129
		
		DEFB	  0, 65,  0, 33,  0, 17,  0,  9
		DEFB	  0,  5,  0,  3,  0,255,255,  0
		
	
		
		DEFB	255,0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0
	
	_sprite02:
	
		DEFB	0,255
		
		DEFB	  0,128,  0,128,  0,128,  0,192
		DEFB	  0,176,  0,140,  0,131,  0,129
		
		DEFB	  0,129,  0,130,  0,130,  0,132
		DEFB	  0,132,  0,136,  0,255,255,  0
		
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,  0,255
		
		DEFB	  0, 17,  0, 33,  0, 33,  0, 65
		DEFB	  0, 65,  0,129,  0,129,  0,193
		
		DEFB	  0, 49,  0, 13,  0,  3,  0,  1
		DEFB	  0,  1,  0,  1,  0,255,255,  0
		
	
	
		DEFB	255,  0,255,  0,255,  0	
		DEFB	255,  0,255,  0,255,  0
	
	_sprite03:
	
		DEFB	0,255
		
		DEFB	  0,129,  0,129,  0,129,  0,129
		DEFB	  0,129,  0,129,  0,129,  0,255
		
		DEFB	  0,128,  0,128,  0,128,  0,128
		DEFB	  0,128,  0,128,  0,255,255,  0
		
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,  0,255
		
		DEFB	  0,  1,  0,  1,  0,  1,  0,  1
		DEFB	  0,  1,  0,  1,  0,255,  0,129
		
		DEFB	  0,129,  0,129,  0,129,  0,129
		DEFB	  0,129,  0,129,  0,255,255,  0
		
		DEFB	255,0,255,0,255,0,255,0,255,0,255,0,255,0
		
	_sprite04:
	
		DEFB 	0,255
		DEFB	  0,136,  0,132,  0,132,  0,130
		DEFB	  0,130,  0,129,  0,129,  0,131
		DEFB	  0,140,  0,176,  0,192,  0,128
		DEFB	  0,128,  0,128,  0,255,255,  0
		DEFB	255,  0,255,  0,255,  0,255,  0
		DEFB	255,  0,255,  0,255,  0,  0,255
		DEFB	  0,  1,  0,  1,  0,  1,  0,  3
		DEFB	  0, 13,  0, 49,  0,193,  0,129
		DEFB	  0,129,  0, 65,  0, 65,  0, 33
		DEFB	  0, 33,  0, 17,  0,255,255,  0
	
		DEFB	255,0,255,0,255,0,255,0,255,0,255,0,255,0	


**File: "collision.asm"**

	
	SECTION code_user
	
	PUBLIC _sp1_TestCollision
	
	EXTERN error_znc
	
	; int sp1_TestCollision(struct sp1_ss *s1, struct sp1_ss *s2, unsigned int tolerance)
	; callee linkage
	
	_sp1_TestCollision:
	
	IFDEF __SDCC
	
	   ; must preserve ix and iy
	   
	   pop af
	   pop bc
	   pop de
	   pop hl
	   push af
	   
	   push ix
	   push iy
	   
	   push bc
	   pop iy
	   
	   push de
	   pop ix
	   
	   call _sp1_TestCollision_0
	
	   pop iy
	   pop ix
	   ret
	
	_sp1_TestCollision_0:
	
	ENDIF
	
	IFDEF __SCCZ80
	
	   pop af
	   pop hl
	   pop ix
	   pop iy
	   push af
	
	ENDIF
	
	   ; ix = struct sp1_ss * (sprite #2)
	   ; iy = struct sp1_ss * (sprite #1)
	   ;  l = tolerance
	   
	   ; see if x-interval intersects
	   
	   ; sprite # 1
	   
	   ld a,(iy+1)                 ; column coordinate (in chars)
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   add a,(iy+5)                ; add horizontal rotation
	   ld b,a                      ; b = pixel x-coord of top left corner
	   
	   ld a,(iy+2)                 ; width (in chars)
	   dec a                       ; do not count blank column on right of sprite
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   sub (iy+17)                 ; subtract x-threshold
	   inc a
	   sub l                       ; build in the tolerance
	   ld c,a                      ; c = width of sprite in pixels
	
	   ; sprite #2
	
	   ld a,(ix+1)                 ; column coordinate (in chars)
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   add a,(ix+5)                ; add horizontal rotation
	   ld d,a                      ; d = pixel x-coord of top left corner
	   
	   ld a,(ix+2)                 ; width (in chars)
	   dec a                       ; do not count blank column on right of sprite
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   sub (ix+17)                 ; subtract x-threshold
	   inc a
	   sub l                       ; build in the tolerance
	   ld e,a                      ; e = width of sprite in pixels
	
	   call RIsIvalInIval8         ; do these two intervals intersect?
	   jp nc, error_znc            ; if not, no collision is detected
	   
	   ; see if y-interval intersects
	   
	   ; sprite #1
	   
	   ld a,(iy+4)
	   and $07
	   ld h,a                      ; h = vertical rotation 0-7
	   ld a,(iy+0)                 ; row coordinate (in chars)
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   add a,h                     ; add vertical rotation
	   ld b,a                      ; b = pixel y-coord of top left corner
	   
	   ld a,(iy+3)                 ; height (in chars)
	   dec a                       ; do not count blank row at bottom of sprite
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   sub (iy+18)                 ; subtract y-threshold
	   inc a
	   sub l                       ; build in tolerance
	   ld c,a                      ; c = height of sprite in pixels
	   
	   ; sprite #2
	
	   ld a,(ix+4)
	   and $07
	   ld h,a                      ; h = vertical rotation 0-7
	   ld a,(ix+0)                 ; row coordinate (in chars)
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   add a,h                     ; add vertical rotation
	   ld d,a                      ; d = pixel y-coord of top left corner
	   
	   ld a,(ix+3)                 ; height (in chars)
	   dec a                       ; do not count blank row at bottom of sprite
	   add a,a                     ; multiply by 8
	   add a,a
	   add a,a
	   sub (ix+18)                 ; subtract y-threshold
	   inc a
	   sub l                       ; build in tolerance
	   ld e,a                      ; e = height of sprite in pixels
	
	   call RIsIvalInIval8         ; do these two intervals intersect?
	   jp nc, error_znc            ; if not, no collision
	   
	   ld hl,1
	   ret
	
	
	
	RIsIvalInIval8:
	
	; Determine if two 8-bit intervals intersect.  Intervals
	; can wrap across 0-255 boundary.  Amazingly this test
	; reduces to detecting whether either of the start points
	; of each interval lie in the other interval.
	;
	; enter  :  b = interval 1 start coordinate
	;           c = interval 1 width
	;           d = interval 2 start coordinate
	;           e = interval 2 width
	; exit   : carry flag set = intersection detected
	; uses   : af
	
	   ld a,b
	   sub d
	   cp e
	   ret c
	
	   ld a,d
	   sub b
	   cp c
	   ret


