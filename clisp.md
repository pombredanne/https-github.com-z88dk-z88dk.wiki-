# CLISP.C

{{libnew:examples:clisp.png?312|clisp}}

Campus Lisp is a lisp interpreter with an optional scheme dialect selectable at compile time ("-DSCHEME").  It was written by Hirotsugu Kakugawa and expanded by Stefano Bodrato.


*  [Source Code](libnew/examples/clisp/source)

*  [Language Specification](Language Specification)

*  [Examples](Examples)

Unfortunately there is no mechanism to save and load defined words so it's best to run on a system that can save and load state.

## ZX Spectrum IF2 Cartridge

The target will be an if2 cartridge for the zx spectrum.

The zx port has crts available that target if2 cartridges.  If you consult [zx_crt.asm](zx_crt.asm), there is a section marked "if 2 cartridge" with startup numbers beginning at 32.  An interesting one for this project is startup=41 which instantiates an fzx output terminal (proportional fonts) that understands common control codes (tty_z88dk).  We need a terminal that understands code 12 (clear screen) as the clisp source prints that character code to clear the screen in main().

if2 cartridges are 16k and are mapped to address 0.  The if2 crts supplied by the library are ORGd to address 0 by default and behave very much like the generic embedded target.  In particular, if the ORG address remains at zero the crt will fill in the z80 rst locations as specified by crt option [CRT_ENABLE_RST](temp/front#pragmas1).  If the ORG is moved away from 0, the crt assumes that you will be supplying the code at address 0 with the restart locations separately.

All of this doesn't matter too much at this point as the first step is to find out if this program will fit on a 16k cartridge in the first place.  In anticipation that this is going to be a problem, we'll do some easy configurations that will minimize code footprint.

### Library Configuration

The zx's library configuration file **clib_cfg.asm** is found in {z88dk}/libsrc/_DEVELOPMENT/target/zx.

The defaults select most of the smallest options.  We'll make two changes:


*  ** defc CLIB_OPT_PRINTF = $1200 **.  A search through the [clisp source](libnew/examples/clisp/source) reveals that printf only uses %s and %ld.

*  ** defc CLIB_OPT_ERROR = $00 **.  Setting to zero will eliminate most of the stored errno strings.

With the file edited, change to directory {z88dk}/libsrc/_DEVELOPMENT and rebuild the zx libraries by running "Winmake zx" (windows) or "make TARGET=zx" (non-windows).

### Crt Configuration

As described in the [pragmas](temp/front#pragmas1) section of the wiki, the active crt configuration is determined by the defaults set by the library and the overrides set by the target.  When compiling using an if2 crt, the target overrides come from target/zx/config-zx-if2/crt_target_defaults.inc.  Let's have a look at the active crt configuration for this case:

 | CRT OPTION                | crt_defaults.inc | crt_target_defaults.inc | compile time setting | 
 | ----------                | ---------------- | ----------------------- | -------------------- | 
 | CRT_ORG_CODE              | 0                |                         | 0                    | 
 | CRT_ORG_DATA              | 0                | 32768                   | 32768                | 
 | CRT_ORG_BSS               | 0                | -1                      | -1                   | 
 | CRT_MODEL                 | 2                |                         | 2                    | 
 | CRT_INITIALIZE_BSS        | 1                |                         | 1                    | 
 | REGISTER_SP               | 0                |                         | 0                    | 
 | CRT_ENABLE_COMMANDLINE    | 1                | 0                       | 0                    | 
 | CRT_ENABLE_RESTART        | 1                |                         | 1                    | 
 | CRT_ENABLE_CLOSE          | 1                | 0                       | 0                    | 
 | CRT_ENABLE_RST            | 0                |                         | 0                    | 
 | CRT_ENABLE_NMI            | 0                |                         | 0                    | 
 | CLIB_EXIT_STACK_SIZE      | 32               | 2                       | 2                    | 
 | CLIB_QUICKEXIT_STACK_SIZE | 32               | 0                       | 0                    | 
 | CLIB_MALLOC_HEAP_SIZE     | 1024             |                         | 1024                 | 
 | CLIB_STDIO_HEAP_SIZE      | 256              |                         | 256                  | 
 | CLIB_BALLOC_TABLE_SIZE    | 0                |                         | 0                    | 
 | CLIB_FOPEN_MAX            | 8                | 0                       | 0                    | 
 | CLIB_OPEN_MAX             | 8                | 0                       | 0                    | 

The CODE ORG is 0, DATA and BSS will be placed at 32768 where RAM is present on 48k spectrums, and BSS will be output as a separate binary (ORG -1).  The stack pointer will be moved to the top of memory (=0) and the compressed ROM model is active.

**CRT_ENABLE_RESTART** is an important option to have enabled.  This instructs the crt to restart the program if it exits.  Campus Lisp allows the user to quit the interpreter and since there's no place to go from a cartridge, restarting is a good option.

The program does not use malloc, it does not register functions with atexit() and it isn't opening any files so we can add a few pragmas to the clisp source to get rid of these things:

	
	#pragma output CLIB_EXIT_STACK_SIZE  = 0    // no exit stack
	#pragma output CLIB_MALLOC_HEAP_SIZE = 0    // not using malloc
	#pragma output CLIB_STDIO_HEAP_SIZE  = 0    // not opening files


These pragmas have been added to the [source code](libnew/examples/clisp/source) already.

### The Compile Part 1

	
	zcc +zx -vn -startup=41 -clib=new clisp.c -o clisp


A directory listing afterward:

	
	13,505 clisp_BSS.bin
	21,549 clisp_CODE.bin
	   510 clisp_DATA.bin


Since the compressed ROM model is being used, the DATA section must be compressed and appended to the CODE section to generate the rom image.  At startup, the crt will decompress that section into RAM before main is called so that the program will have its variables properly initialized before starting.

	
	zx7 clisp_DATA.bin
	copy /B clisp_CODE.bin+clisp_DATA.bin.zx7 clisp_if2.img


(windows 'copy' is used to concatenate the two files)

A directory listing shows:

	
	13,505 clisp_BSS.bin
	21,549 clisp_CODE.bin
	   510 clisp_DATA.bin
	   341 clisp_DATA.bin.zx7
	21,890 clisp_if2.img


The next step would be to create a blank 16k ROM binary and insert the cartridge image into that at address 0 but perhaps you've already noticed that our image file is more than 16k and will not fit.

We could try to scrape together 5k in savings by improving the source code but there is an easier route that should be tried first.  We can compile the program to run in RAM, and then store a compressed version of the program on cartridge that is decompressed into ram before execution.

Whether this works or not will depend on how well zx7 can compress the resulting executable.

### The Compile Part 2

The only change that will be made is to move the CODE, DATA and BSS sections into RAM.  Even though the program will be run from ram, the compressed rom model will allow the DATA and BSS sections to be initialized at startup.  If the user quits the interpreter, the crt will restart the program (**CRT_ENABLE_RESTART** is enabled) and re-initialize DATA and BSS.  In a ram model, that initialization does not occur and the interpreter could only be run once with correct initialization.

From the last directory listing it can be seen that the uncompressed DATA and BSS sections add up to 510+13505 = 14015 bytes.  This is the amount of space needed at runtime for variables.  The CODE section plus compressed DATA is 21890 bytes.

Let's find a suitable placement for these items in RAM.  Since the spectrum ROM is not active, all of RAM is available to us except for the display file mapped at 16384-23295.

Begin with the stack, which is placed at the top of RAM (**REGISTER_SP** = 0).  A very generous space alotment is 1k.  Realistically no more than about 256 bytes would be needed but let's make this work the first time for sure.  The bottom of the stack is now at address 65536-1024 = 64512.

The DATA and BSS sections can be placed next at address 64512-14015 = 50497.

And finally the CODE+compressed_DATA is placed at address 50497-21890 = 28607.  Let's choose 28000 so there is some room for CODE to grow.

To make these changes, the following pragmas are added to the source:

	
	#pragma output CRT_ORG_CODE = 28000
	#pragma output CRT_ORG_DATA = 50497


(these pragmas are already present in the [source code](libnew/examples/clisp/source) but they have been commented out)

Recall that **CRT_ORG_BSS** is -1 by default.  This means BSS will follow DATA but a separate binary will be output by the assembler.

Compile again:

	
	zcc +zx -vn -startup=41 -clib=new clisp.c -o clisp


Directory listing:

	
	13,505 clisp_BSS.bin
	21,474 clisp_CODE.bin
	   510 clisp_DATA.bin


You'll note the CODE section has shrunk somewhat.  Now that the ORG has moved away from address 0, the crt is no longer supplying z80 restarts in the first 100 bytes of memory.

Form the image by appending the compressed DATA section to the CODE section:

	
	zx7 clisp_DATA.bin
	copy /B clisp_CODE.bin+clisp_DATA.bin.zx7 clisp_if2.img


Directory listing:

	
	13,505 clisp_BSS.bin
	21,474 clisp_CODE.bin
	   510 clisp_DATA.bin
	   345 clisp_DATA.bin.zx7
	21,819 clisp_if2.img


"clisp_if2.img" is what needs to be stored at address 28000 and the program is started by jumping to that address.

This would be a good time to verify that the memory map is viable:

 | Section                | Length | Address Range  | 
 | -------                | ------ | -------------  | 
 | CODE + compressed_DATA | 21819  | 28000 - 49818  | 
 | DATA                   | 510    | 50497 - 51006  | 
 | BSS (follows DATA)     | 13505  | 51007 - 64511  | 
 | stack                  | -      | 65535 and down | 

None of the sections overlap and there is plenty of space for the stack (65535 to 64512).  If there were a problem with the assignments, we'd have to choose new addresses for each section and compile again.

#### Tape File

The goal is still to generate a cartridge but we have a binary that can be loaded at address 28000 and run so it's rather easy to generate a tap file.

[APPMAKE](appmake) will take the binary and produce a tap file for us.  To find out what options are supplied specifically for the zx target enter this:

	
	appmake +zx


There are many options to specify what sort of tap file is being created.  We'll use a straightforward basic loader.  When using the basic loader we do have to make sure the code being loaded is not located so far down in memory that basic is unable to operate.  28000 leaves plenty of room for the basic loader to run.

	
	appmake +zx --binfile clisp_if2.img --org 28000 --blockname clisp


Directory listing:

	
	21,819 clisp_if2.img
	21,899 clisp_if2.tap


The tap file is ready to go!

#### Cartridge ROM

The moment of truth has come.  Compress the binary image and see if it fits in 16k.

	
	zx7 clisp_if2.img


Directory listing:

	
	21,819 clisp_if2.img
	11,187 clisp_if2.img.zx7


zx7 has managed to compress the image from 21k to 11k which fits into a 16k cartridge easily!

A short asm program at address 0 can uncompress this image and start it:

	
	; FILE "clisp_if2.asm"
	; z80asm -b -ic:\z88dk\libsrc\_DEVELOPMENT\lib\sccz80\zx.lib clisp_if2.asm
	
	org 0
	
	main:
	
	   di
	   
	   ld hl,clisp_image
	   ld de,28000
	
	   EXTERN asm_dzx7_standard
	   call asm_dzx7_standard
	   
	   jp 28000
	
	clisp_image:
	
	   BINARY "clisp_if2.img.zx7"


(notice that code is not assigned to any sections nor is a memory map specified.  code and data will appear in the resulting binary in the order they are encountered by the linker; this is the same behaviour as in assemblers that do not support sections)

Save as "clisp_if2.asm".

The program is very simple.  Interrupts are disabled because it's not known whether they are enabled when the cartridge starts.  There won't be any interrupt handler present so it's important they are disabled.  Then the image is decompressed to address 28000 by calling the zx7 decompress library routine "asm_dzx7_standard".  The program is started by jumping to address 28000.  The BINARY directive includes the executable "clisp_if2.img.zx7" at the end of this short program.

To assemble this program run:

	
	z80asm -b -ic:\z88dk\libsrc\_DEVELOPMENT\lib\sccz80\zx.lib clisp_if2.asm


"asm_dzx7_standard" is a library routine so the assembler must be told where to find the library.  You may need to change the path if z88dk was installed in a directory other than "c:\z88dk".

Directory listing:

	
	   297 clisp_if2.asm
	11,270 clisp_if2.bin
	 2,124 clisp_if2.map
	11,373 clisp_if2.obj
	   392 clisp_if2.sym


The output binary is "clisp_if2.bin" and this bit of code is run from address 0, which is where the spectrum will begin executing a cartridge.

A 16k ROM cartridge needs to be generated.  [APPMAKE](appmake) can manipulate binaries too:

Create a blank 16k ROM:

	
	appmake +rom -s 16384 -o if2_blank.rom


Insert "clisp_if2.bin" into this rom at address 0:

	
	appmake +inject -b if2_blank.rom -i clisp_if2.bin -s 0 -o if2_clisp.rom


Directory listing:

	
	16,384 if2_blank.rom
	16,384 if2_clisp.rom


And that's our cartridge ROM: "if2_clisp.rom"

Many emulators can run this rom by dragging and dropping it to the emulator window.

Don't forget to check that the crt restarts the interpreter when the user exits.  To exit the interpreter, generate an EOF on stdin (press the exit key so to speak).  CTRL-D generates the EOF and on the spectrum this is key combination CAPS+SYM_SHIFT+D.

## Other Things to Try

### Compile the Scheme Variant

The source code allows a Scheme variant to be compiled if "-DSCHEME" is added to the compile line.

### Compile the SpecLisp compatible Variant and/or add graphics

[SpecLisp by Serious Software](http://blog.funcall.org/lisp/2015/10/30/zx-spectrum-lisp/) had its own dialect probably derived from Lisp 1.5.   It also featured turtle graphics.
To enable such options: -DSPECLISP -DGRAPHICS

### Change the Font

Any [FZX font](FZX font) can be used by the fzx output terminal instantiated by the crt.  The default font used by the fzx terminals is "ff_ao_Soxz" but this can be changed by editing the crt being used.  The font I used is called "ff_ao_Napier".  

Navigate to the directory {z88dk}/libsrc/_DEVELOPMENT/target/zx/startup where all the crts are located.  Open the file "zx_crt_41.asm".  Replace all instances of "_ff_ao_Soxz" with "_ff_ao_Napier" and recompile.  That's it!

### SDCC Compile

sdcc can be used to compile the program as well.  If the sdcc_iy library is used the compile line becomes:

	
	zcc +zx -vn -startup=41 -SO3 -clib=sdcc_iy --max-allocs-per-node200000 clisp.c -o clisp


"-SO3" enables the [aggressive peephole rules](temp/front#sdcc2) that reduces code size but might also cause incorrect code to be generated.  It's optional so it can be removed.

"sdcc_iy" means the library uses the iy index register.  sdcc uses the ix register for stack frame.  This means the library does not need to preserve the ix register and that will lead to smaller code.  On the zx target, the ROM interrupt must be disabled if iy is used by the program because the rom interrupt expects iy to point into the system variables area but since interrupts are disabled here, that's not a concern.

Programs containing lots of longs like this one cause sdcc to generate larger code.  The result will be about 1.5k larger than sccz80's output but sdcc's code will be faster.

Be prepared to wait a while for this compile to complete!

