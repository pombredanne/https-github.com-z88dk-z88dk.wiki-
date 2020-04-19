The crt is the startup code that runs before calling `main()`.  It is responsible for setting the memory map, instantiating device drivers on stdin/stdout/stderr, initializing the bss and data sections and calling any initialization code prior to calling main().  On return from `main()` it is responsible for closing open files, resetting the stack and preparing to return to the host.

In combination with the crt, the memory map and crt configuration completely determine the program's execution environment at compile time.  The memory map defines what goes where and the crt configuration sets defaults such as code origin and heap size.

The library supplies crts, memory maps and crt configurations for supported targets.  Usually a non-trivial target will have multiple crt choices that differ in what devices are instantiated on stdin, stdout, stderr and possibly more than one memory map if the target supports ram-resident programs, rom-resident programs or bankswitched memory.  Suggested combinations are condensed into a startup option selected on the compile line (`-startup=n`).  A default startup is chosen if no startup is specified.  In previous compile examples on this page, no startup was specified so a default was chosen by z88dk that would be considered most common for the target.

The startups and crts are target-specific of course so details should be gathered from the target's wiki entry.  We will look at the z80 target in some detail so that the options available are tangibly explained.

## target_crt.asm

The specific crt used in the compile is found from the target's `target_crt.asm` file.  For the z80 target this is [`z80_crt.asm.m4`](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/z80_crt.asm.m4).  The file is just a switch on the startup value specified on the compile line, eg "zcc +z80 -vn -startup=0 ....".  At the top of the file, if startup was not defined on the compile line, a default is selected for you (2 in this case).  An important value is -1 which allows the user to supply his own crt file.

For each startup value, a memory model is selected by number %%(__MMAP=n)%%, a crt configuation is chosen %%(__CRTDEF=n)%% and a real crt.asm file is included from the target's startup directory.

Let's choose startup=0.  This selects the "ram model" for the z80 target; the reason for the name will become evident shortly.

This sets up the following:

  * **%%__CRTDEF = 0%%** selects crt configuration number zero.
  * **%%__MMAP = 0%%** selects memory map number zero.
  * [startup/z80_crt_0.asm](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/startup/z80_crt_0.asm.m4) is the start-up code.

The actual start-up code contains static data structure definitions to satisfy stdio which is both difficult to read and difficult to understand so the original macro file it was generated from is preferable to refer to: [startup/z80_crt_0.asm.m4](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/startup/z80_crt_0.asm.m4).

Of particular interest are lines 32-43 which list statically instantiated devices.  The order of instantiation determines the file descriptor each instantiated driver will be associated with.  The z80 target is a general one so there are no drivers instantiated.

Lines 45 and up contains the first start-up assembly code.  The z80 start-up code accommodates two cases.  The first occurs when the code ORG is zero, in which case the crt sets up the z80 restarts in the bottom 100 bytes of memory.  The second occurs when the code ORG is not zero, in which case the restart page is not set up.  If you follow the startup code you will see how the crt performs initialization before it calls main() and what it does on return from main().

## crt configuration

The crt configuration defines properties of the execution environment.  The value of the **%%__CRTDEF%%** variable selects a configuration from a number of options in the target's "crt_target_defaults.inc".  For the z80 target this is [`target/z80/crt_target_defaults.inc`](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/config_z80_public.inc).

For **%%__CRTDEF = 0%%** the following defaults are set:

| CRT OPTION                 | crt_target_defaults.inc | meaning |
|---|---|---|
| CRT_ORG_CODE               |  0                      | ORG 0 |
| CRT_ORG_DATA               |  0                      | data section appends to code section |
| CRT_ORG_BSS                |  0                      | bss section appends to data section |
| CRT_MODEL                  |  0                      | ram model in effect |
| REGISTER_SP                |  0                      | crt will set SP=0 |
| CRT_STACK_SIZE             |  512                    | max stack size is 512 bytes |
| CRT_INITIALIZE_BSS         |  0                      | crt will not initialize BSS |
| CRT_ENABLE_COMMANDLINE     |  0                      | no command line |
| CRT_ENABLE_RESTART         |  0                      | do not restart on program exit; infinite loop or return to host if code org != 0  |
| CRT_ENABLE_CLOSE           |  1                      | close files on exit |
| CRT_ENABLE_RST             |  0                      | no user supplied rst vectors |
| CRT_ENABLE_NMI             |  0                      | no user supplied nmi |
| CLIB_EXIT_STACK_SIZE       |  2                      | two functions can be registered with atexit() |
| CLIB_QUICKEXIT_STACK_SIZE  |  0                      | no functions can be registered with at_quick_exit() |
| CLIB_MALLOC_HEAP_SIZE      |  -1                     | heap is automatically created and will lie between the end of BSS and the stack |
| CLIB_STDIO_HEAP_SIZE       |  128                    | size of stdio heap used to allocate FILE* and driver structures |
| CLIB_BALLOC_TABLE_SIZE     |  0                      | no block allocator table |
| CLIB_FOPEN_MAX             |  0                      | max number of open FILE* = 0 |
| CLIB_OPEN_MAX              |  0                      | max number of file descriptors = 0 |

As will be seen in the next section, the memory maps supplied by z88dk organize the output binary into CODE, DATA and BSS sections.  CODE contains read-only z80 code and read-only data.  DATA contains self-modifying code and initially non-zero variable data.  BSS holds space for variables that are zero on startup.

The crt options are explained below:

  * **CRT_ORG_CODE** The address of the CODE section.
  * **CRT_ORG_DATA** The address of the DATA section.  If 0 or -1, the DATA section immediately follows the CODE section.
  * **CRT_ORG_BSS** The address of the BSS section.  If 0 or -1, the BSS section immediately follows the DATA section.

The final executable will consist of one binary for each section with its own ORG address.  There will always be a CODE binary, a DATA binary will exist separately from CODE if its org is non-zero and a BSS binary will exist separately from DATA if its org is non-zero.

  * **CRT_MODEL** 0 = RAM model, 1 = ROM model, 2 = compressed ROM model.
    * **RAM model**:  The crt will not initialize the data and bss sections on startup.  Normally this is coupled with zero ORGs specified for the DATA and BSS sections.  In this arrangement the output is a single binary containing CODE followed by the initialized variables in DATA followed by the zeroed variables in BSS.  Because the crt does not initialize DATA and BSS, the executable can only be run once with correct initial state.  This is the most common model used for systems running programs from ram and this is why it is called the RAM model.  In systems that are sensitive to large binaries (maybe long loading times or limited storage medium), the BSS section can be cut off by specifying a BSS ORG address of -1.  The special -1 value is recognized by z80asm to mean the section appends to the previous one but a separate binary should be produced.  The output will then be the CODE+DATA binary and a BSS binary.  The CODE+DATA binary can be used as the executable as long as RAM occupied by BSS is zeroed prior to execution.  You can get the crt to do this zero-initialization by setting CRT_INITIALIZE_BSS to 1.
    * **ROM model**:  The output binary is destined for rom.  In this case an ORG address for DATA in RAM must be specified (if not an error will be emitted).  If BSS's ORG is zero, the crt will modify this to -1 so that a separate BSS binary will be generated.  The output will be three separate binaries:  CODE, DATA and BSS.  In order for the crt to initialize properly, it needs to have a stored copy of the DATA section in ROM, and it expects to find it immediately following the CODE section.  So the final executable that is stored in ROM should consist of the CODE+DATA sections put together.  From a windows command prompt this can be done with "copy /B name_CODE.bin+name_DATA.bin rom.bin".  This file "rom.bin" is the executable that should be stored on ROM.  On startup the crt will copy the stored DATA section into ram, zero the BSS section and call main.  Note that the output BSS binary is only useful to indicate how much ram it occupies.  The total ram space required is the size of the DATA binary plus the BSS binary.
    * **Compressed ROM model**:  Again the output binary is destined for rom.  Everything mentioned for the rom model applies here except the stored data section will be compressed.  Once the three binaries are generated (CODE, DATA, BSS), the DATA section should be compressed with zx7.  "zx7 name_DATA.bin" will generate a compressed file "name_DATA.bin.zx7".  This file should be appended to the CODE binary and the result is what should be stored in ROM.  The crt will decompress the stored DATA section into RAM and zero the BSS section before calling main.  It's always preferable to use the compressed ROM model over the plain ROM model as the final rom image will be smaller.  Note that the output BSS binary is only useful to indicate how much ram it occupies.  The total ram space required is the size of the uncompressed DATA binary plus the BSS binary.
  * **REGISTER_SP** The crt will set the stack pointer to the indicated value before doing anything else.  0 indicates the top of ram and -1 is a special value that indicates the crt should not change the stack pointer.  This makes sense on some hosts which will set the stack pointer prior to starting the C program.
  * **CRT_STACK_SIZE** The maximum stack size.  This is only referenced if the heap is automatically created.  See CLIB_MALLOC_HEAP_SIZE.
  * **CRT_INITIALIZE_BSS** If non-zero the crt will zero the BSS section before calling main.  If one of the rom models is selected this always happens regardless.
  * **CRT_ENABLE_COMMANDLINE** If non-zero the crt will generate argc and argv.  Targets that don't have a command line get values that indicate an empty command line.
  * **CRT_ENABLE_RESTART** If non-zero the crt will restart the program if it exits.  This is especially helpful for a standalone program launched from ROM.  Note that RAM model programs will not have their bss and data sections properly initialized.
  * **CRT_ENABLE_RST** Some crts are intended to generate code for address 0 and will fill in the z80 restarts.  In these crts, code can be inserted at the z80 rst locations to jump to user supplied subroutines.  The CRT_ENABLE_RST value is eight bits with one bit corresponding to each rst location.  Rst 0 (bit 0) is ignored.  Rst 0x08 corresponds to bit 1 and so on up to Rst 0x38 at bit 7.  If any of these bits is set, a jump is inserted at the rst location to a user subroutine called "_z80_rst_xxh" where xx is the restart location in hex.  The rst subroutine can be written in C or assembly.  Note that rst 38h corresponds to the im1 interrupt routine.  The same mechanism applies -- you can implement an im1 service routine by supplying the subroutine "_z80_rst_38h" and setting bit 7 of CRT_ENABLE_RST.  Don't forget that an interrupt service routine needs to preserve register values and terminate with "ei; reti".  Where CRT_ENABLE_RST has zero bits, the crt will insert "ret" except for the im1 entry point where "ei; reti" will be inserted.
  * **CRT_ENABLE_NMI** In crts intended to generate code for address 0, a non-zero value will cause the crt to insert "jp _z80_nmi" at address 0x66 to execute a user-supplied routine on nmi.  Otherwise the crt inserts "retn".  The nmi service routine must preserve register values and terminate with "retn".
  * **CLIB_EXIT_STACK_SIZE** The maximum number of functions that can be registered by atexit().  The C standard calls for 32 but most programs don't use any so this is usually reduced by the target config.
  * **CLIB_QUICKEXIT_STACK_SIZE** The maximum number of functions that can be registered by atquickexit().  The C standard calls for 32 but most programs don't use any so this is usually reduced by the target config.
  * **CLIB_MALLOC_HEAP_SIZE** The size of the heap from which memory is allocated using malloc().  If zero, no heap is present.  If -1, the heap is automatically initialized to occupy the area between the end of the BSS section and the bottom of the stack.  The maximum stack size is assumed to be **CRT_STACK_SIZE** bytes (above).  (Note: if the max heap size computed is negative, which can happen if the stack is below the BSS section, the program will immediately exit without any indication).  Any other positive value larger than 14 bytes will cause a heap of that size to be create in the BSS section.
  * **CLIB_STDIO_HEAP_SIZE** The size of stdio's heap.  This heap is used to allocate driver structures when files are opened and FILE* when memstreams are opened.  If set to 0, the stdio heap will only be large enough to accommodate stdin, stdout, stderr if they are present.
  * **CLIB_BALLOC_TABLE_SIZE** The queue table for the block memory allocator.  If zero there is no table.
  * **CLIB_FOPEN_MAX** Max number of FILE* that can be simultaneously open.  This includes stdin, stdout, stderr if they are present.  If < =0, only FILE* for stdin, stdout, stderr will be created if demanded by crt options.  If -1, FILE* lists won't be created unless stdin, stdout or stderr exist.
  * **CLIB_OPEN_MAX** Size of the fd table and indicates how many files can be simultaneously open.  If 0, only space for stdin, stdout, stderr will be made if demanded by crt options. 

As seen in the table above, the library chooses sensible defaults suitable for the target but your program can override these defaults using `#pragma` in your C source or from the command line.

## pragma overrides

Pragmas embedded in the C source can override the crt configuration.  Pragmas can be located in any C source file in your project but it's best to keep them confined either to your main.c or to a dedicated file in projects that use makefiles and consist of many source files.

Overriding is done by name.  An example will illustrate:

```
#include <stdio.h>
#include <stdlib.h>

#pragma output CRT_ORG_CODE          = 30000  // org 30000
#pragma output CLIB_MALLOC_HEAP_SIZE = 4096   // 4k heap needed

main()
{
   ...
}
```

The code origin is moved to address 30000 and the heap size is made 4096 bytes.  As described in the last section, the heap will be created in the BSS section.

If you find that you are overriding many defaults you may want to edit the target defaults to something more suitable for your projects so you can do away with these pragmas.

## memory map

The memory map is defined in the target's "memory_model.inc".  For the z80 target this is [`target/z80/crt_memory_map.inc`](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/crt_memory_map.inc) which includes [`target/crt_memory_model.inc`](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/crt_memory_model_z80.inc).  If you recall, the selection of "startup=0" on the compile line as discussed above set the variable **%%__MMAP = 0%%**.  This selects the one and only memory map defined in the memory model file.  This memory map is almost universally used and would only need to be different for bankswitched targets.  The model sets up the standard CODE/DATA/BSS sections.

## user initialization and exit code

The crts create two sections that allow programs to place initialization and cleanup code into the crt. The intialization code is run just before main() is called and the exit code is run just after registered atexit() functions are called but before files are closed.

The section names are "**code_crt_init**" and "**code_crt_exit**".

This example shows how the clib initializes the user heap.

```
; some leading underscores removed from labels
; so as not to disturb wiki formatting

SECTION bss_alloc_malloc
   
malloc_block:             defs clib_malloc_heap_size
   
SECTION code_crt_init
   
ld hl,malloc_block
ld bc,clib_malloc_heap_size
   
EXTERN asm_heap_init
call asm_heap_init
```

First the memory region reserved for the heap is placed into section bss_alloc_malloc (this will be part of the program's BSS section).  Then the initialization code is placed into section code_crt_init.

## User-Supplied Crt

You don't have to use the library-supplied crts nor do the crts have to be as sophisticated as the above.  You may prefer to have something very simple or something with a different memory map.  If the compile line contains "-startup=-1" a local file "crt.asm" will be taken as the crt.  The crt must set up the environment and call _main at minimum.  If no memory map is set up, the output will be a single binary blob with CODE, DATA, BSS items mixed in the order the linker encounters them.  Ideas can be taken from the library's crts, specifically have a look at the m4 macros in the target's startup subdirectory which are the crts before devices are instantiated (these files end in *.m4).