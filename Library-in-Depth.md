==== Creating a Target ====

There are two C libraries in z88dk that exist independently of each other.  This topic is concerned with creating a new target for the new C library only.

To make changes to an existing target, you can simply add a new CRT.  Adding a new CRT allows you to change the startup code, change the memory map, change the drivers instantiated for stdio, and change the default CRT options (these are things like code origin, heap size and so on).

If your target is completely new you can follow the steps below for creating a new target.  In addition to new CRTs, a new target defines what is included in the C library, can define architecture dependent functions and header files, and can define its own set of device drivers.

=== Adding CRTs to an Existing Target ===

This [[http://www.z88dk.org/forum/viewtopic.php?pid=13383#p13383|forum posting]] contains details for adding a CRT to the simple embedded target and can serve as instructions until the topic is properly filled out.

=== Creating a New Target ===

== 1. ZCC Configuration File ==

As with all grand projects, the first step in creating a target is settling on a name.  The name will appear in the compile line "**zcc +yourname .... test.c -o test**" so keeping it short will save a little typing but it must be long enough that it is descriptive of your target and that it won't collide with names for already existing targets in z88dk.  

The list of current targets can be found in [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/lib/config/|z88dk/lib/config]] where the name of each cfg file corresponds to a target name.  For purposes of this discussion we will create a target called "**temp**".

To make zcc aware of the target a new file "temp.cfg" must be added to this directory:

**z88dk/lib/config/temp.cfg**
<code>
#
# Target configuration file for z88dk
#

# Asm file which contains the startup code (without suffix)
CRT0		 DESTDIR/lib/temp_crt0

# Any default options you want - these are options to zcc which are fed
# through to compiler, assembler etc as necessary
OPTIONS		 -v -O2 -SO2 -I. -DZ80 -DTEMP -D__TEMP__ -D__TEMP -M -clib=default

CLIB     default -ltemp_clib -lndos
CLIB     new -D__SCCZ80 -Ca-D__SCCZ80 -Cl-D__SCCZ80 -nostdlib -IDESTDIR/include/_DEVELOPMENT/sccz80 -Ca-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -ltemp -LDESTDIR/libsrc/_DEVELOPMENT/lib/sccz80 -Cl-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -crt0=DESTDIR/libsrc/_DEVELOPMENT/target/temp/temp_crt
CLIB     sdcc_ix -compiler=sdcc -D__SDCC -D__SDCC_IX -Ca-D__SDCC -Ca-D__SDCC_IX -Cl-D__SDCC -Cl-D__SDCC_IX -nostdlib -IDESTDIR/include/_DEVELOPMENT/sdcc -Ca-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -ltemp -LDESTDIR/libsrc/_DEVELOPMENT/lib/sdcc_ix -Cl-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -crt0=DESTDIR/libsrc/_DEVELOPMENT/target/temp/temp_crt
CLIB     sdcc_iy -compiler=sdcc --reserve-regs-iy -D__SDCC -D__SDCC_IY -Ca-D__SDCC -Ca-D__SDCC_IY -Cl-D__SDCC -Cl-D__SDCC_IY -nostdlib -IDESTDIR/include/_DEVELOPMENT/sdcc -Ca-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -ltemp -LDESTDIR/libsrc/_DEVELOPMENT/lib/sdcc_iy -Cl-IDESTDIR/libsrc/_DEVELOPMENT/target/temp -crt0=DESTDIR/libsrc/_DEVELOPMENT/target/temp/temp_crt
</code>

  * **CRT0** holds the name of the crt file used by the classic C library.  This is ignored when the new C library is used.
  * **OPTIONS** lists the default compile line options.  These can be overridden and augmented by the following CLIBs.
  * **CLIB** lists options that are added to the compile when "-clib=???" appears on the compile line.  For compiles using the new C library, "-clib=new", "-clib=sdcc_ix" or "-clib=sdcc_iy" are used.  The first one sets up a compile using sccz80 while the latter two set up compiles using sdcc with the distinction being which index register the C library uses.

Together, **OPTIONS** and **CLIB** define a few constants which can be tested in user C or asm code, determine the default optimization levels, select the CRT to use, and set up the library and include paths.

In the file above, the target name used is "temp" so for your own target, you'll want to replace all instances of "temp" and "TEMP" with your target's name.

== 2. Create the Target Directory Structure and Add Initial Contents ==

All information concerning a target for the new C library is located in [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/|z88dk/libsrc/_DEVELOPMENT/target]].  Unfortunately the CVS view of the repository on the internet tends to show dead directories mixed with current ones and this is something that will have to be coped with.  The list of currently supported targets are **cpm**, **embedded** and **zx** and you can see that each of these have their own directory here.  **m** is a pseudo-target used to compile the floating point library.

Create a new directory for your target using its config name.  In this example, that's "**z88dk/libsrc/_DEVELOPMENT/target/temp**".

Inside the new temp directory, {{libnew:z88dk-target-temp.zip|unzip this package of files and directories}} to initially populate the target.  Spend a few minutes renaming files so that "temp" is replaced by your target name.  Then open files in the **temp** (your name) directory and **temp/startup** startup directory and replace instances of "temp" or "TEMP" with your target's name.

Here is a brief description of each file and subdirectory:

| d | driver   | contains the target's device drivers  |
| d | library  | contains list files specifying the contents of the target's C library  |
| f | library/temp_sccz80.lst  | lists contents of the target's C library when sccz80 is used as compiler  |
| f | library/temp_sdcc_ix.lst  | lists contents of the target's C library when sdcc is used as compiler and the library uses ix  |
| f | library/temp_sdcc_iy.lst  | lists contents of the target's C library when sdcc is used as compiler and the library uses iy  |
| d | startup  | contains the target's CRTs  |
| f | startup/temp_crt_0.asm  | CRT #0 generated from temp_crt_0.m4  |
| f | startup/temp_crt_0.m4  | Human readable and editable CRT macro that is expanded by m4  |
| | | |
| f | clib_cfg.asm  | selects library build-time options for the target's C library  |
| f | clib_target_cfg.asm  | selects library build-time options for the target-specific portion of the C library  |
| f | clib_target_constants.inc  | target-specific global constants defined at program compile time  |
| f | clib_target_variables.inc  | target-specific (memory occupying) variables defined at program compile time  |
| f | clib_target_defaults.inc  | crt options defined at program compile time  |
| f | memory_model.inc  | memory map defined at program compile time  |
| f | temp_crt.asm  | defines which crt to use at program compile time  |
| f | temp_crt.opt  | must include temp_crt.asm  |

== 3. Determine the Contents of the Target's C Library ==

When the target's C library is built, the list of files assembled into the library are read from the target's library subdirectory.  In the present example that is **z88dk/libsrc/_DEVELOPMENT/target/temp/library**.  Three libraries are built corresponding to the three list files found here:  sccz80, sdcc_ix and sdcc_iy.  The sccz80 library is used when sccz80 is chosen as compiler.  The sdcc_ix and sdcc_iy libraries are chosen when sdcc is the compiler and are selected between by either "-clib=sdcc_ix" or "-clib=sdcc_iy" on the compile line.  The difference between the two is which index register the C library uses.  "sdcc_ix" corresponds to the library using ix and "sdcc_iy" corresponds to the library using iy.  It's always preferable to use the "sdcc_iy" version of the library because this gives sdcc sole use of ix for its frame pointer while the library uses iy.  If "sdcc_ix" is selected, sdcc and the library must share ix which means the library must insert extra code to preserve the ix register when it is used.  This means the "sdcc_iy" compile will be smaller.  The choice is present because some targets reserve one of the index registers for themselves.

The new C library's source code is located just above the target directories in [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/|z88dk/libsrc/_DEVELOPMENT]].  Just to rule out the few dead directories, the currently valid source code directories are listed below.

| adt  | Abstract data types:  arrays, vectors, stacks, queues, lists  |
| alloc  | Memory allocators:  balloc (block allocator), heap (malloc), obstack  |
| arch  | Architecture-specific code  |
| compress  | Data compression  |
| ctype  | Character classification  |
| drivers  | Base classes for writing device drivers  |
| error  | Exit subroutines used by the C library to set errno  |
| fcntl  | I/O using file descriptors  |
| font   | Fixed width bitmap font definitions and fzx proportional fonts  |
| input  | Direct hardware access to keyboard, joystick, mouse  |
| inttypes  | Known names for some functions operating on largest integer type as defined by C standard  |
| l  | Library subroutines and compiler primitives  |
| locale  | A few functions defining locale-specific character comparison  |
| math  | Integer and float math functions  |
| network  | Network related  |
| setjmp  | Long jumps for C  |
| sound  | Music and sound effects for 1-bit devices  |
| stdio  | Standard C I/O using FILE*  |
| stdlib  | Standard C library includes number <-> text conversion, random numbers, sorting, etc  |
| string  | Standard C string and raw memory manipulation  |
| temp/sp1  | Software sprites for bitmapped displays  |
| threads  | (Incomplete) Mutex, critical section, spinlock, call_once  |
| z80  | Z80 related includes precise delay, port I/O, IM2 ISRs, interrupt state  |

Each source directory is structured to contain **list files** listing all source files, a **z80 directory** containing the asm source code, and a **c directory** containing preamble code for the C interface that gathers parameters from the stack and jumps into the asm implementation.

While a large portion of the library will use an 8080 subset of the instruction set, the library is written to be as compact and fast as reasonable and does use all z80 features, including the EXX set when appropriate.  This does cause problems for some targets and the solution that will be supplied in z88dk is to offer alternate implementations for some functions in a subdirectory other than z80.  But those alternate implementations are not present at this time so if your target cannot allow use of the EXX set, you will have to either supply alternate implementations for affected functions or you will have to exclude some of the functions from your target's library.  If you have such a difficult target it's probably best to inquire about porting in the [[http://www.z88dk.org/forum/forums.php|z88dk forums]].

Next it's time to list what is in your target's C library.  Three files have already been provided in **z88dk/libsrc/_DEVELOPMENT/target/temp/library** that include all of the C library minus target-specific code.  For example, the file "temp_sccz80.lst" contains this:

**library/temp_sccz80.lst**
<code>
@adt/adt_sccz80.lst
@alloc/alloc_sccz80.lst
@compress/compress_sccz80.lst
@ctype/ctype_sccz80.lst
@drivers/drivers.lst
@error/error.lst
@fcntl/fcntl_sccz80.lst
@font/font_4x8/fonts.lst
@font/font_8x8/fonts.lst
@font/fzx/fzx_sccz80.lst
@inttypes/inttypes_sccz80.lst
@l/l.lst
@l/sccz80.lst
@locale/locale.lst
@math/math_integer.lst
@math/math_float_sccz80.lst
@network/network_sccz80.lst
@setjmp/setjmp_sccz80.lst
@stdio/stdio_sccz80.lst
@stdlib/stdlib_sccz80.lst
@string/string_sccz80.lst
@threads/threads_sccz80.lst
@z80/z80_sccz80.lst
</code>

The paths are relative to the source code base directory **z88dk/libsrc/_DEVELOPMENT**.  The "@" symbol means the indicated file is a list file rather than an asm file.  You can modify these files to exclude portions of the library as you like or to be more choosy about which functions make it into the library by replacing list files with a more specific list of functions.  But unless your target is restricted by being unable to use certain registers or you want to purposely exclude some functionality, there is no reason to restrict what goes into the target's C library.  Targets without bitmapped displays may want to exclude the font related things and I suppose this will reduce library size and library build time.

There are some functions that will not work without further defines or code supplied for the target.  These include functions in **font/fzx** (a single character putchar must be written to complete support for proportional fonts), **input** (the C library defines standard functions for direct access to keyboards / joysticks / mice hardware but this must be written for each target), **sound** (targets must define how the state of a 1-bit speaker is toggled) and **temp/sp1** (bitmap software sprite engine requires much customization to suit display resolution).  If you would like to add these features you can ask how in the [[http://www.z88dk.org/forum/forums.php|z88dk forums]].

== 4. Set the Default Library Configuration ==

The C library allows the user to customize it for speed and size.  These customizations are specified in the target's **clib_cfg.asm** file located in **z88dk/libsrc/_DEVELOPMENT/target/temp**.  The selections already supplied are typical but if you would like to change them you can.  In particular, printf has the float converters %aefg disabled so that compiles don't involve the float library.  When they are enabled, any use of printf drags in the float library and the compile line must include "-lm".  scanf does not yet support %aefg but stdlib does include atof() and strtod() which can be used for ascii -> float conversion.  I'd suggest using %[ to scan a float string and then convert that using one of these functions.

Keep the **clib_cfg.bak** file consistent as it serves as backup if the user makes his own customizations later.

== 5. Set the Target Library Configuration ==

The target library configuration serves the same function as the regular library configuration but it applies to target-specific code only.  The target-specific customizations are found in **clib_target_cfg.asm**.  The file provided defines two things that all targets should define:  the z80 clock rate in Hz and some bit flags that define whether the z80 is nmos or cmos.  The latter is important because the nmos z80 has a bug that makes the determination of interrupt state (enabled or disabled) more difficult.  The library can be compiled to use the simpler cmos code or the more complicated nmos code as needed.  If your code must run on all manner of z80s, choose nmos.

If you will be writing target-specific library code, this is the place to put any required configuration information that will be available when the library code is built.  As an example, you can have a look at the **zx** target's [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/zx/clib_target_cfg.asm?content-type=text%2Fplain|clib_target_cfg.asm]] file.  It has the same architecture information at the top but it also has more defines that customize the sp1 sprite library and define how 1-bit sound is generated.

Keep the **clib_target_cfg.bak** file consistent as it serves as backup if the user makes his own customizations later.

== 6. Build the Z80 Library ==

The z80 libraries are built from **z88dk/libsrc/_DEVELOPMENT** using either **Winmake** (windows) or the **Makefile** (non-windows).

First add the new "temp" target to Winmake by changing this line:

**z88dk/libsrc/_DEVELOPMENT/Winmake.bat**
<code>
set alltargets= embedded cpm m temp zx 
</code>

This list of targets must have a space before the first target name and a space after the last target name.  Because this is a .bat file, Windows makes it difficult to edit it.  To edit, right click on "Winmake.bat" and choose "Send To" from the context menu.  Then select a text editor from the options.

Next add the new "temp" target to the Makefile by changing this line:

**z88dk/libsrc/_DEVELOPMENT/Makefile**
<code>
TARGET ?= embedded zx m cpm temp
</code>

Let's build the target library.  From **z88dk/libsrc/_DEVELOPMENT**, run "Winmake temp" (windows) or "make TARGET=temp" (non-windows).

Three libraries should be built without error and they should appear as **temp.lib** in **z88dk/libsrc/_DEVELOPMENT/lib/sccz80**, **lib/sdcc_ix** and **lib/sdcc_iy**.  It may take 5-10 minutes to finish.

== 7. Create the CRT(s) ==

**CRT** stands for C Runtime.  It is a short bit of code that sets up the execution environment before main() is called.  It will take care of things like initializing the heap, parsing the command line, setting the stack pointer's initial value and, on program exit, running the exit stack and preparing to return to the host.

In z88dk, two other entities help define the environment.

  * The **CRT Options** are a collection of defines that let the user program customize its environment at compile time.  The C library defines defaults which can be optionally overridden by the target and they in turn can be optionally overridden by pragmas embedded in a program's C source.  Some of these CRT options are implemented by the C library but some are the responsibility of the CRT.

  * The **memory map** determines where the linker places code and data in memory.  z88dk defines a default memory map that consists of CODE, DATA and BSS sections.  For most targets this default memory map will be adequate.

Combinations of CRT, CRT Options and memory map are summarized by a single numerical **startup** value.  The user will specify a startup in the compile line:

<code>
zcc +embedded -vn -startup=0 -SO3 -clib=sdcc_iy --max-allocs-per-node200000 test.c -o test
</code>

If a startup is not specified, you will be providing a default value.  This startup value will be used in a switch to enable the relevant CRT, CRT Options and memory map.

Implied here is that targets can have any number of CRTs defined for them.  In this instruction just one CRT will be created but you can create as many as are needed to suit different types of output.

CRTs are actually defined as m4 macros.  As will be seen in the device driver section, this makes it easy to create stdin, stdout, and stderr.  So the m4 macro is what is edited by people but the expanded macro -- an associated .asm file -- is what is used in the compile as the active crt.  This means all CRTs come in pairs:  an .m4 created by a person and an .asm which is the m4 expansion of that file.  These pairs are named and numbered as in "temp_crt_0.m4" and "temp_crt_0.asm", here corresponding to CRT #0 (recall "temp" is the name of the target).  Some of the targets supplied by z88dk actually have one m4 file associated with several .asm files but we've since figured out this is a maintenance problem so having a 1:1 relationship between the two is highly recommended.

To build CRTs [[#m4|m4 must be installed]].  m4 is the common macro processor found in Unix and Linux so only Windows users are likely to have to install it separately.

CRTs are defined in the target's startup directory, in this case **z88dk/libsrc/_DEVELOPMENT/target/temp/startup**.  A simple CRT was included in the bundle of files used to initially populate the target.  You can edit this file as it is discussed below to create your target's first CRT.

\\ 
----
\\ 

**z88dk/libsrc/_DEVELOPMENT/target/temp/startup/temp_crt_0.m4**
<code>
dnl############################################################
dnl##           TEMP_CRT_0.M4 - EXAMPLE TARGET               ##
dnl############################################################
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;                  temp standalone target                   ;;
;;      generated by target/temp/startup/temp_crt_0.m4       ;;
;;                                                           ;;
;;                  flat 64k address space                   ;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; CRT AND CLIB CONFIGURATION ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

include "../crt_defaults.inc"
include "crt_target_defaults.inc"
include "../crt_rules.inc"
</code>

This is where the final state of the **CRT options** is determined.  The CRT options are a list of defined constants that indicate options to the C library and the CRT.  It is the responsibility of the CRT to implement some of these options, as appropriate, for the target.  All options were briefly described earlier under [[#crt_configuration|crt configuration]] and it will be mentioned here where CRT options will apply.

First the default settings of all CRT options are included.  Then the settings overridden by the target are included.  Finally, rules are executed to determine the final state of those options.  The rules are simple:  they give highest priority to pragmas in the C source, next priority to the target overrides and lowest priority to the default settings.

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; SET UP MEMORY MODEL ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

include "memory_model.inc"
</code>

The memory map is defined here by communicating to the linker where the various sections are assigned in memory.

Applicable CRT options consumed by the memory model:

| %%__crt_org_code%%  | sets the address of the CODE section  |
| %%__crt_org_data%%  | sets the address of the DATA section  |
| %%__crt_org_bss%%   | sets the address of the BSS section  |
| %%__crt_model%%     | chooses between ram model, rom model or compressed rom model  |

If you make your own memory maps it will be up to you to apply these options there.

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; GLOBAL SYMBOLS ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

include "../clib_constants.inc"
include "clib_target_constants.inc"
</code>

Defines various global constants required by the C library.

  * "clib_constants" correspond to general library defines like error numbers and ioctls.
  * "clib_target_constants" correspond to target-specific defines like device error numbers.

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; INSTANTIATE DRIVERS ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

; No device drivers available yet.
; sprintf/sscanf + memstreams can still be used

include(../../clib_instantiate_begin.m4)
include(../../clib_instantiate_end.m4)
</code>

Device drivers are assigned to FILEs and file descriptors between the "begin" and "end" macros.  This target does not have any device drivers yet so nothing is instantiated here.

**The first actual code begins here:**

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; STARTUP ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

SECTION CODE

PUBLIC __Start, __Exit

EXTERN _main

__Start:

   nop
</code>

The "nop" is in place to control the name of the output file.  The final output is one or more binaries corresponding to non-empty sections with independent ORG addresses.  The binary filenames are created from the output filename with the section name concatenated.  For example, if the output filename is "test" and the first non-empty section is "CODE" then one of the binaries will be named "test_CODE.bin".

In the CRT written here, it is possible that no code will be generated until after the "code_crt_main" section below.  This means the output filename may be "test_code_crt_main.bin" when it's expected the filename will be "test_CODE.bin".  The "nop" is guaranteeing that the CODE section will not be empty.  If you add code to this section you can eliminate the "nop".

<code>
   ; set stack address
   ; (optional)
</code>

Applicable CRT options:

| %%__register_sp%%  | if -1 the user is asking not to change SP else the user is supplying an initial stack pointer value.  |

Some hosts will set the stack pointer before starting a machine code program so it can make sense not to alter the stack pointer value.

See lines [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/zx/startup/zx_crt_ram.m4?revision=1.7&view=markup|110-116 of this CRT]] for an example.

<code>
   ; parse command line
   ; (optional)
</code>

Applicable CRT options:

| %%__crt_enable_commandline%%  | if non-zero the user is requesting the command line to be parsed and argc + argv to be generated.  |

If the target does not have a means to communicate a command line, a course of action you can take is to generate an empty command line.  See [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/zx/startup/zx_crt_ram.m4?revision=1.7&view=markup|lines 118-142 of this CRT]] for an example.

The library supplies two functions to help parse command lines:

  * [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/l/z80/l_command_line_parse.asm?content-type=text%2Fplain|l_command_line_parse]]  This function is intended for parsing command lines from a region in memory that is likely to be overwritten during program execution.  An example is the cp/m target which leaves the command line at address 0x80 which also happens to be the location of the primary FCB.  The first time a file is read or written from disk, the command line will be overwritten.  What this function does is copy the words to the stack as they are parsed so that the command line will be available throughout program execution.  See [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/cpm/startup/cpm_crt.m4?revision=1.7&view=markup|lines 110-164 of this CRT]] for an example.

  * [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/l/z80/l_command_line_parse_in_place.asm?content-type=text%2Fplain|l_command_line_parse_in_place]]  This function is intended for parsing command lines from a region in memory that will not be overwritten during program execution.  The command line will be parsed into words in place, with terminating NUL bytes written into the command line as needed.  See [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/symbos/startup/symbos_crt.m4?revision=1.2&view=markup|lines 162-194 of this CRT]] for an example.  At the time of writing this CRT is only partially implemented.

Both these functions stop parsing when a file re-director ("<" or ">") is encountered.  In the future this part of the command line will be interpretted to redirect i/o to files.  Neither function currently groups quoted strings into a single argument.  This will also change in a future update.

<code>
   ; initialize data section

   include "../clib_init_data.inc"

   ; initialize bss section

   include "../clib_init_bss.inc"
</code>

The C library will zero the BSS section and initialize the DATA section here.  If the ram model is active, neither initialization will take place.

<code>
SECTION code_crt_init          ; user and library initialization
</code>

This is a point where the library or user can insert initialization code.  For example, if a heap is created the library will insert heap initialization code here.

<code>
SECTION code_crt_main

   ; call user program
   
   call _main                  ; hl = return status

   ; run registered exit() functions

   IF __clib_exit_stack_size > 0
   
      EXTERN asm_exit
      jp asm_exit              ; exit function jumps to __Exit
   
   ENDIF
</code>

main() is called and if exit() functions can be registered, they are executed.

<code>
__Exit:

   ; abort(), exit(), quick_exit() arrive here and can be called from anywhere in the program
   ; this means the stack may be unbalanced

   ; hl = return status

   push hl
</code>

'hl' holds the return value.  You can choose to save it or throw it away.  This code saves it.

<code>
SECTION code_crt_exit          ; user and library cleanup
</code>

This is a point where the library or use can insert clean-up code.

<code>
SECTION code_crt_return

   ; close files
   
   include "../clib_close.inc"

   pop hl                      ; hl = return status
</code>

The C library closes open files and then the return value is recovered.

<code>
   ; exit program

   jr ASMPC                    ; infinite loop (ASMPC means current address)
</code>

At this point you must decided what to do about a program exit.  In this example, an infinite loop is entered.

Applicable CRT options:

| %%__crt_enable_restart%%  | if non-zero the user is indicating the program should restart on exit.  |

If the program exits to the host, the original stack pointer must be recovered so that the return address is available.  At "%%__Start%%" the host's original stack pointer can be saved and then at program exit, this value can be recovered and then "ret" executed to return.  A good place to save the stack pointer value is in section BSS_UNINITIALIZED which is described below.

If the program is to be restarted, keep in mind that the ram model does not initialize the BSS or DATA sections.  This means ram model programs can only execute once with correct initial state.  The CRT will always initialize BSS and DATA for rom model programs so there is no issue with restarting those.  This is discussed in more detail [[#crt_configuration|here]].  Other issues to keep in mind include what to do about command line parsing on second runs (the command line can be modified by the program; one suggestion is to generate an empty command line for second runs) and resetting the stack (the stack can be unbalanced at program exit so the stack pointer should be reset to an appropriate address before restart).  You may need to add a "%%__Restart%%" label to define a point to restart the program and you may have to restructure the example CRT somewhat.

On program exit, consider what should happen to interrupts.

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; RUNTIME VARS ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

SECTION BSS_UNINITIALIZED

; place any uninitialized data here (eg saved stack pointer)
; bss and data section initialization will not touch it
</code>

Data defined in section "BSS_UNINITIALIZED" will not be zeroed by the CRT's BSS initialization code and it will persist across program restarts.

<code>
include "../clib_variables.inc"
include "clib_target_variables.inc"
</code>

Definitions of global library data that occupies memory space.

  * "clib_variables.inc" adds general global variables including the heap, exit stacks and thread id.
  * "clib_target_variables.inc" adds global variables for target-specific library code.

<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; CLIB STUBS ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

include "../clib_stubs.inc"
</code>

Labels defined for incomplete portions of the library that need to be present for successful compiles.

=== NOTE FOR CRTS INTENDED FOR ADDRESS 0x0000 ===

The example CRT was written assuming an ORG address that is not zero.  If the ORG address will be zero, the CRT must fill in the z80 restarts and the nmi entry point at 0x66.

Applicable CRT options:

| %%__crt_enable_rst%%  | the lower eight bits indicate which rst locations are to be implemented by user code.  |
| %%__crt_enable_nmi%%  | non-zero indicates the nmi will be implemented by user code.  |

Further discussion of these CRT options can be found [[#crt_configuration|here]].

The embedded target has a [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/embedded/startup/embedded_crt_rom.m4?revision=1.11&view=markup|CRT which will generate the z80 restarts]] if the ORG address is 0.  The CRT is written to apply to both 0 ORG and non-zero ORG so there is conditional code present for the 0 ORG case.  You can see how the restarts were implemented by examining that code.

\\ 
----
\\ 

Congratulations on completing your first CRT.  Now it's time to **generate the .asm file that will be used in compiles**:

From **z88dk/libsrc/_DEVELOPMENT/target/temp/startup**:

m4 temp_crt_0.m4 > temp_crt_0.asm

== 8. Define the Default CRT Options ==

The CRT options overridden by the target during compilation are defined in **z88dk/libsrc/_DEVELOPMENT/target/temp/crt_target_defaults.inc**.  Although only CRT options that the target actually overrides need to be listed here, in reality all the CRT options are listed here in order to document them for each compile.

As for CRTs, there can be any number of different CRT options settings in this file and they are distinguished by the constant "%%__CRTDEF%%".  This file is a simple switch based on the value of %%__CRTDEF%%.

The example creates a single CRT option selection for "%%__CRTDEF = 0%%" that will be used with the CRT created in the last section.  You can make changes to these options as required for your target.  The options are more completely described in the [[#crt_configuration|crt configuration]] topic.

<code>
IF __CRTDEF = 0

   ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
   ;; temp ram model ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
   ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

   defc TAR__crt_org_code              = 32768
   defc TAR__crt_org_data              = 0
   defc TAR__crt_org_bss               = 0

   defc TAR__crt_model                 = 0
   
   defc TAR__register_sp               = 0
   defc TAR__crt_stack_size            = 512
      
   defc TAR__crt_initialize_bss        = 0
   
   defc TAR__crt_enable_commandline    = 0
   defc TAR__crt_enable_restart        = 0
   defc TAR__crt_enable_close          = 1
   
   defc TAR__crt_enable_rst            = 0
   defc TAR__crt_enable_nmi            = 0
   
   ; clib defaults
   
   defc TAR__clib_exit_stack_size      = 2
   defc TAR__clib_quickexit_stack_size = 0
   
   defc TAR__clib_malloc_heap_size     = -1
   defc TAR__clib_stdio_heap_size      = 128
   
   defc TAR__clib_balloc_table_size    = 0
   
   defc TAR__clib_fopen_max            = 0
   defc TAR__clib_open_max             = 0

ENDIF
</code>

For a minimum binary size you will also want to set **%%TAR__clib_exit_stack_size = 0%%** (no functions can be registered with atexit()), **%%TAR__clib_malloc_heap_size = 0%%** (the heap will not be created) and **%%TAR__clib_stdio_heap_size = 0%%** (no files can be opened; for a target without drivers this affects whether [[http://www.gnu.org/software/libc/manual/html_node/String-Streams.html|memstreams]] can be created).

The 0 values for DATA and BSS origin mean they will append to the CODE section.  The ram model is in effect (**%%TAR__crt_model = 0%%**) so these settings mean the output will be a single binary "name_CODE.bin" containing the CODE,DATA,BSS sections concatenated together and the DATA,BSS sections will not be initialized by the CRT (they will hold their initial values in the binary image).  If your output is destined for ROM, you will have to change to the compressed ROM model and set the DATA section's ORG to the first RAM address.  The CRT will have to ensure that the stack pointer points into RAM as well.

If **%%TAR__clib_malloc_heap_size = -1%%** then the heap will be automatically located and sized such that it occupies the space between the end of the BSS section and the bottom of the stack.  The value of **%%TAR__crt_stack_size%%** is used to indicate the maximum size of the stack when the heap size is calculated at runtime.  Should a negative size be computed for the heap (size = SP - stack_size - BSS_END - 14), the program will simply exit just after starting up without any indication.  The other way to automatically create a heap is to specify a size > 14 bytes.  Such a heap will be allocated in the BSS section and it will be automatically initialized by the CRT.  The heap can also be dynamically created at runtime or statically created at a fixed address as was done in this [[http://www.z88dk.org/wiki/doku.php?id=libnew:examples:sp1_ex1#changing_the_memory_map|example C program]].

These options can, of course, be overridden by pragmas inserted by the user into C source.

Keep the **crt_target_defaults.bak** file consistent as it serves as backup if the user makes his own customizations later.

== 9. Define the Default Memory Map ==

The memory map instructs the linker where to place code and data in memory.  z80asm defines containers called "sections" to which code and data can be added from any source file in the project.  In the linking stage these sections are sequenced according to the memory map and then one or more binaries are produced as output, depending on whether the memory map creates disjoint regions in memory.  There is more discussion of this in the [[#mixing_c_and_assembly_language|mixing C and assembly language]] topic.

The C library defines sections for each module in the library.  For example strings, stdlib and stdio all have their own sections.  This was done to assist users in assigning library code to different regions in memory at a fine grain level.  But perhaps a more interesting application is that by including "-Cl--split-bin" on the compile line, every section is output in its own binary file.  This allows users to find out at-a-glance what is occupying the memory in output binaries.

Small talk aside, as with CRTs and CRT options, many memory maps can be defined and these are identified by the label "%%__MMAP%%".  A default memory map is defined by the C library and this memory map is what is used by this example target:

**z88dk/libsrc/_DEVELOPMENT/target/temp/memory_model.inc**
<code>
IF __MMAP = 0

   ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
   ;; standard CODE/DATA/BSS memory map ;;;;;;;;;;;;;;;;;;;;;;;
   ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

   INCLUDE "../crt_memory_model.inc"

   ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
   
ENDIF
</code>

The include actually defining the memory map is found one directory above:
[[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/crt_memory_model.inc?content-type=text%2Fplain|z88dk/libsrc/_DEVELOPMENT/target/crt_memory_model.inc]]

Three major sections are defined (CODE, DATA, BSS) with optional ORG addresses and many smaller sections are sequenced such that they merge into these bigger ones.

This memory map is suitable for most targets and will work for programs destined for RAM or ROM.  You may need a different one if you do something non-trivial with bankswitched memory or if you have a target that defines non-traditional memory regions.

It's unlikely you will have to create your own memory map but you can by adding another %%__MMAP%% case.

Keep the **crt_memory_model.bak** file consistent as it serves as backup if the user makes his own customizations later.

== 10. Startup ==

At compile time, the runtime environment is initialized and defined by the **CRT**, the **CRT options** and the **memory map**.  These three parameters are summarized by a single **startup** value on the compile line.

<code>
zcc +embedded -vn -startup=0 -SO3 -clib=sdcc_iy --max-allocs-per-node200000 test.c -o test
</code>

In this example the user has selected **startup #0**, the meaning of which is defined in the master crt file.

**z88dk/libsrc/_DEVELOPMENT/target/temp/temp_crt.asm**
<code>
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
; SELECT CRT0 FROM -STARTUP=N COMMANDLINE OPTION ;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

INCLUDE "zcc_opt.def"

IFNDEF startup

   ; startup undefined so select a default
   
   defc startup = 0

ENDIF


;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; user supplied crt ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

IF startup = -1

   INCLUDE "crt.asm"

ENDIF

;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;; ram model ;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;
;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;;

IF startup = 0

   ; generic temp startup

   IFNDEF __CRTDEF
   
      defc __CRTDEF = 0
   
   ENDIF
   
   IFNDEF __MMAP
   
      defc __MMAP = 0
   
   ENDIF

   INCLUDE "startup/temp_crt_0.asm"

ENDIF
</code>

The include of "zcc_opt.def" pulls in any pragmas defined in the C source.  A default startup value is defined if the user didn't specify one and then, for **startup=0**, which **CRT**, **CRT options** and **memory map** to use are defined.  Modify this as you like or add more startup cases as needed.

The file **temp_crt.opt** in the same directory must include **temp_crt.asm**.

=== THE TARGET IS NOW FUNCTIONAL ===

Try a simple compile:

<code>
#include <stdio.h>

unsigned char buffer[100];

main()
{
   sprintf(buffer, "Hello World!\n")
}
</code>

zcc +temp -vn -clib=new test.c -o test

The output will be one or more binaries depending on the options you defined but for a ram model compile, there will be one binary file generated "test_CODE.bin".

To use printf and scanf, device drivers will have to be written and instantiated on stdin and stdout.  See the **Device Drivers** section below.

== 11. Optionally Add Target-Specific Library Code to Your Target ==

== 12. Cooperating with Third Party Libraries ==

== 13. Creating a ROM Resident Monitor, Operating System or Shell ==

== 14. Bankswitched Memory ==

==== Device Drivers ====

Device drivers are functions assigned to file descriptors that implement a small set of device-independent stdio messages for communicating with some sort of i/o device.  By implementing that set of messages, the stdio library will be able to perform i/o using the device.  For targets, these drivers can be instantiated on stdin, stdout and/or stderr in the CRT to allow printf and scanf to function normally.  You can also invent other streams at driver instantiation time in the CRT.  For example, the cp/m target defines additional streams (stdpun, stdrdr, stdlst) that are connected to devices defined by cp/m.  You could also define multiple terminal windows and assign different streams to each so that you could print output into two or more independent windows on screen.  But we are getting a little bit ahead of ourselves.

There are three broad categories of device drivers defined by the C library:  character, terminal and disk.

  - **Character** refers to devices that generate output or receive input one character at a time.  This would include things like printers or serial ports.
  - **Terminal** refers to a console where an input stream and an output stream are tied together so that the user can enter editable text.  Normally a terminal has some kind of output screen with dimensions measured in characters or pixels but this isn't a requirement.  You could have input coming from a serial device and output delivered to another serial device with the terminal driver interpretting backspace characters mediating in between.
  - **Disk** refers to devices that do i/o in blocks or sectors.  The library caches disk sectors to improve runtime performance on real devices.  Unfortunately the disk portion of the library is not ready so it is unavailable.

These categorizations are artificial because you can write a driver that implements the stdio messages using your own code without caring what kind of device is attached.  The categorizations come into play if you use library code to help implement a driver.  There are three types of library driver code that you can inherit from while implementing your driver and, yes, they are character, terminal and disk.

Before getting started, let's lay out some of the reasons behind the driver model used by the C library.

  * **The C library must be a complete implementation**.  z88dk aims to be a complete C implementation and similar in functionality to C compilers found on 32-bit machines as far as that can be taken.  This means stdio must work in a device independent manner and file descriptors, defined by POSIX and not the C standard, must be present.  Stdio must be able to connect to any sort of i/o device.
  * **Many devices must be supported in the same compile in a space efficient manner**.  Some targets have a dozen or more different disk devices that programs may want to copy between.  They can have a half dozen different display modes and the program may want to have terminals using more than one display mode at once.  
  * **Device drivers should not be a burden to implement**.  The library should provide code to take care of complex behaviours required by some types of devices.  Drivers should be writable using library code to implement most functionality with the author only supplying a small amount of custom code to complete it.
  * **Stdio should be reasonably fast** without consuming too much memory.  

We've solved these problems by making the drivers object oriented.

  * **Code inheritance** from the library allows complex drivers to be written using mainly library code.
  * **Space efficiency** is achieved through code reuse aided by the message-passing method used to implement the object model.  The message-passing method allows all open terminals to share the same library code and all disk devices to share the same disk code.
  * **Achieving reasonable speed** without consuming a lot of memory is more difficult since the strategy used in standard C implementations is to create large buffers in user-space for each open FILE in order to service character-at-a-time i/o.  Extra buffers are hard to accommodate in a small 64k (or less) address space.  Instead, on the input side, z88dk's stdio pushes a state machine to the driver that indicates what input it will accept from the driver.  Then only the driver's buffers are used for i/o, if it needs buffering.  On the output side, z88dk tries to send strings of characters instead of individual chars.

Deliberately, stdio is not the only means to read/write to the screen or devices inside the library.  More direct and low level functions are always present so that the user has the choice to use something simpler if speed or program size is a concern.  If you are the originator of a target, it is up to you to supply this functionality and normally the drivers would be implemented in terms of these low level functions themselves. The stdio implementation in z88dk is written in assembly language so it is smaller than those available from other z80 C compilers but 64k is a restrictive environment for C programs nevertheless.  For the cp/m target the option is always there to do i/o directly through bdos.  Similarly for the zx target, low level functions are available for output to the screen or for reading keys directly from the hardware.  You can bypass device drivers entirely and you can still use stdio by confining your program to the sprintf/sscanf families or [[http://www.gnu.org/software/libc/manual/html_node/String-Streams.html|memstreams]] and doing i/o directly from buffers in memory.  However things are more convenient and fun to program with a proper stdio implementation present, especially if it's easily affordable in the user program and target.

=== Directory Structure ===

The target's device drivers are located in **z88dk/libsrc/_DEVELOPMENT/target/TARGETNAME/driver**.

^  d  ^ driver  ^ contains the target's device drivers  ^
^  f  ^ driver/driver.lst  ^ lists source files of all drivers  ^
^  d  ^ driver/character  ^ contains character device drivers  ^
|  d  | driver/character/cpm_00_input_reader  | directory holding "cpm_00_input_reader" driver's message implementation  |
|  f  | driver/character/cpm_00_input_reader.asm  | device driver "cpm_00_input_reader"  |
|  f  | driver/character/cpm_00_input_reader.lst  | lists all source files related to "cpm_00_input_reader"  |
|  f  | driver/character/cpm_00_input_reader.m4  | macro for statically instantiating the "cpm_00_input_reader" driver in the CRT  |
^  d  ^ driver/disk  ^ contains disk device drivers  ^
^  d  ^ driver/terminal  ^ contains terminal device drivers  ^
^  d  ^ driver/raw  ^ contains raw device drivers  ^

The files corresponding to an example "cpm_00_input_reader" driver are in the un-highlighted portion above.  The driver has a directory where its message functions reside.  The driver itself is the .asm file, the .lst file lists all source files connected to the driver and the .m4 is the instantiation macro for the CRT.

The name "cpm_00_input_reader" is composed of three parts:  "cpm" is the target name (this is a real driver for the cp/m target), "00_input" indicates it derives from the library's "character_00_input" base class and "reader" is a name connected to the device being driven.

When creating your device driver, copy this structure and place files in the relevant location.  Once the driver is written, its list file should be included in the master list **driver/driver.lst**.  If this is the first driver for the target, be sure that this master list file is included in the target's libraries.  It must be listed in all three list files in the directory **target/TARGETNAME/library**.

The driver code itself should be placed in the most appropriate code section:

  * **code_driver**
  * **code_driver_character_input**
  * **code_driver_character_output**
  * **code_driver_terminal_input**
  * **code_driver_terminal_output**

Once the device driver is ready, the target's libraries will need to be rebuilt by running "Winmake TARGETNAME" or "make TARGET=TARGETNAME" from z88dk/libsrc/_DEVELOPMENT.

The [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/cpm/|cp/m target's driver directory]] can be used as a model.

=== The Raw Device Driver ===

On entry to a device driver, IX points at a file descriptor data structure (aka FDSTRUCT).  This data structure contains fields used by the library at the top but it can be extended by any number of bytes by your driver.  What is stored there is up to you.

^  FDSTRUCT                     ^^^
^  Offset  ^ Name      ^ Purpose  ^
|  0       | JP        | jump instruction (195)  |
|  1..2    | driver    | address of driver  |
|  3       | flags     | F0D0 0TTT (F=1 if filter, D=0 if stdio handles unget, TTT=ioctl message category)  |
|  4       | ref       | reference count  |
|  5       | mode      | 0TXC BAWR (mode byte supplied to open)  |
|  6       | ioctl_f0  | bit flags automatically managed by ioctl  |
|  7       | ioctl_f1  | bit flags automatically managed by ioctl  |
|  8..13   | mutex     | used by library to block other threads  |
|  14..?   | ddata     | your driver's data  |

An FDSTRUCT is created dynamically when **open()** is called but it can also be created statically by instantiating the driver in the CRT.  Dynamically opening files will not be available in the library until disk i/o is complete so in the current z88dk, static instantiation is the only means to instantiate drivers.

Some of the flag bits managed by the library in the upper portion of the FDSTRUCT impact on your driver and you will determine what those flag settings are when the driver is instantiated.

In **flags** (offset 3)

  * **F=0**.  A set bit indicates the driver is a filter.  Filters forward the stream to another driver after processing it somehow (for example, character set conversion or stream decompression might be done by a filter).  Filters are not fully implemented yet.
  * **D=0**.  One stdio message requires the driver to implement an unget character.  If this bit is set, the driver indicates that it will implement ungetc otherwise stdio will take care of it.
  * **TTT**.  The ioctl message category:  000=n/a, 001=terminal input, 010=terminal output, 011=character input, 100=character output.  The library defines many ioctls to modify driver behaviour and the ioctl() function is written to reject ioctls not applicable to the driver in order to simplify driver code.  If the driver declares itself an input terminal, attempts by the user to send output terminal ioctls will result in an error.  Type 000 drivers will not receive any library ioctls.

In **mode** (offset 5).

  * **RW** bits indicate if the driver is open for read and/or write.  These bits are tested on every read() and write() call but at the FILE* level they are only test when the FILE is opened.
  * **BA** bits indicate if the driver is open in binary mode and append mode.  These bits can be used or ignored by your driver.  When open() is implemented, the driver will have an opportunity to reject opens if the mode bits are not supported.

In **ioctl_f0** and **ioctl_f1** (offsets 6 and 7).  In an effort to reduce binary size, **ioctl()** will automatically manage bit flags corresponding to bits 13..3 of these two bytes.  **ioctl()** supplies commands to set, reset or read these bit flags but before it makes changes it will ask your driver if the changes are acceptable.  If your driver implements runtime options, the bit flags are a cheap way to keep track of them.  Bits 15..14 and 2..0 cannot be altered by ioctl().

**ddata**.  Any amount of per-file ram data that your driver requires can be appended here.

**Stdio communicates with the driver by sending messages to it**.  The message type is held in the "A" register while other registers will hold parameters.  Drivers are therefore structured like switch statements, testing the value of "A" and jumping to implementation code if a match is found.  Not all stdio messages have to be implemented.  If your driver only does output it can ignore input related messages.  If you don't care to support ioctls you can ignore those messages too.  Ignoring a message will normally mean exiting from the end of your switch statement via the library's **enotsup_zc** function.  "enotsup_zc" will set errno=ENOTSUP, hl=0 and carry flag set.  The carry flag being set indicates an error to the caller.  At FILE* level this will place the FILE structure in an error state that will prevent any further i/o until the error is cleared.  At POSIX level, the caller will only be informed that an error occurred; further i/o is not actually prevented so if you want that behaviour you will have to implement that in your driver.

^  STDIO MESSAGES                         ^^^
^ A=  ^  Type  ^ Request                    ^
| **STDIO_MSG_PUTC**  |  **Out**  | **Write a single char to the stream multiple times.**  |
|  IN:| E'=char code, BC'=HL=number of chars to output > 0  ||
|  OUT:| HL=num chars successfully written, carry set if error  ||
| **STDIO_MSG_WRIT**  |  **Out**  | **Write a buffer to the stream.**  |
|  IN:| HL'=void *buffer, BC'=HL=length of buffer > 0  ||
|  OUT:| HL'=void *buffer + num chars written, HL=num chars successfully written, carry set if error  ||
| **STDIO_MSG_GETC**  |  **In**  | **Read a single char from the stream.**  |
|  IN:| none  ||
|  OUT:| A=HL=char, if error: carry set and HL=0 for general error or HL=-1 for eof  ||
| **STDIO_MSG_READ**  |  **In**  | **Read multiple characters from the stream into a buffer.**  |
|  IN:| DE'=void *buffer, BC'=HL=length of buffer > 0  ||
|  OUT:| DE'=void *buffer + num chars read, BC=num chars successfully read, carry set if error with HL=0 general or HL=-1 eof  ||
| **STDIO_MSG_EATC**  |  **In**  | **Read chars from the stream until stopped by the state machine.**  |
|  IN:| HL'=bool (*qualify)(char c), HL=length of buffer >= 0  ||
|  OUT:| BC=num chars read from stream, HL=next unconsumed char, carry set if error with HL=0 general or HL=-1 eof  ||
|  RESERVED:| EXX set is owned by the qualify function  ||
|  NOTE:| Characters are read from the stream until either the buffer is filled or the qualify function disqualifies a char.  ||
|       | qualify() is called by setting A=char; exx; call l_jphl; exx, if carry is set the char is rejected.  ||
|       | The char returned in HL is a peek at the next char in the stream; it must be ungot by either the driver or stdio.  ||
| **STDIO_MSG_SEEK**  |  **In/Out**  | **Move file pointer to a new position.**  |
|  IN:| C'=C=STDIO_SEEK_SET (0);STDIO_SEEK_CUR (1);STDIO_SEEK_END (2), DEHL=signed 32-bit offset  ||
|  OUT:| DEHL=new file position, carry set if error with HL=0 general or HL=-1 eof, BC' = 0 or num chars sought forward from current ||
| **STDIO_MSG_ICTL**  |  **In/Out**  | **Program sends an ioctl() to driver.**  |
|  IN:| DE=ioctl request, BC=first parameter, HL=va_arg* (must mind L->R sccz80 or R->L sdcc order on stack)  ||
|  OUT:| carry reset if ok with HL=return value!=-1, carry set if error  ||
|  RESERVED:| EXX set must not be altered  ||
| **STDIO_MSG_FLSH**  |  **In/Out**  | **Flush any buffers (terminals clear input buffers too).**  ||
|  IN:| none  ||
|  OUT:| carry set on error  ||
| **STDIO_MSG_CLOS**  |  **In/Out**  | **The file is being closed.**  ||
|  IN:| none  ||
|  OUT:| carry set on error (likely ignored)  ||
|  NOTE:| A flush message is always sent ahead of close.  Deallocate any resources this driver has allocated.  ||

These messages, error numbers and library ioctls are defined in [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/target/clib_constants.inc?content-type=text%2Fplain|z88dk/libsrc/_DEVELOPMENT/target/clib_constants.inc]] which is included into every CRT.

While writing device drivers or any other code with z88dk, it can be very helpful to be familiar with the library code supplied by z88dk.  Using it can keep binary sizes down and speed up development time substantially.  The root source directory is **z88dk/libsrc/_DEVELOPMENT**.  Many of the subdirectories will have familiar names as they implement portions of the C standard.  Two other important subdirectories might be "**l**" which contains many small subroutines and "**error**" which provides exit points for functions.  These exit points can be used to balance the stack and return after optionally setting errno and a return value appropriately.

Given the above, a driver might be structured like this:

<code>
SECTION code_driver
SECTION code_driver_character_input

PUBLIC character_00_input

EXTERN STDIO_MSG_EATC, STDIO_MSG_READ, STDIO_MSG_SEEK
EXTERN STDIO_MSG_FLSH, STDIO_MSG_CLOS, STDIO_MSG_GETC
EXTERN STDIO_MSG_ICTL

EXTERN character_00_input_stdio_msg_getc, character_00_input_stdio_msg_eatc
EXTERN character_00_input_stdio_msg_read, character_00_input_stdio_msg_seek
EXTERN character_00_input_stdio_msg_ictl, error_znc, error_enotsup_zc

character_00_input:

   cp STDIO_MSG_GETC
   jp z, character_00_input_stdio_msg_getc   ;; jump to function implementing getc

   cp STDIO_MSG_EATC
   jp z, character_00_input_stdio_msg_eatc   ;; jump to function implementing eatc
   
   cp STDIO_MSG_READ
   jp z, character_00_input_stdio_msg_read   ;; jump to function implementing read
   
   cp STDIO_MSG_SEEK
   jp z, character_00_input_stdio_msg_seek   ;; jump to function implementing seek

   cp STDIO_MSG_FLSH
   jp z, error_znc                           ;; do nothing and report no error
   
   cp STDIO_MSG_ICTL
   jp z, character_00_input_stdio_msg_ictl   ;; jump to function implementing ioctl
   
   cp STDIO_MSG_CLOS
   jp z, error_znc                           ;; do nothing and report no error
   
   jp error_enotsup_zc                       ;; hl = 0 puts FILE stream in error state
</code>

Drivers are allowed to change any registers except ix and iy unless otherwise documented.

The above is an abstract base class driver implemented in the library.  The entire driver can be found in [[http://z88dk.cvs.sourceforge.net/viewvc/z88dk/z88dk/libsrc/_DEVELOPMENT/drivers/character/character_00/input/|z88dk/libsrc/_DEVELOPMENT/drivers/character/character00/input]].

If your driver properly ignores messages that do not apply and implements messages from the list above that do apply, your driver is finished.  This is the .asm file mentioned in the last section.  The message functions are placed in the driver's subdirectory and the list file will list all of the driver's source files.  The last piece is the .m4 macro that will allow the driver to be instantiated as a file in the CRT.  This is discussed in the **Driver Instantiation** topic below.

=== The Character Device Driver ===

The library supplies code to implement character device drivers.  Character device drivers are intended for performing i/o on devices like serial or parallel ports that generate or consume a stream of individual characters.  Since the library's character driver base classes change stdio's multiple character messages to single character messages, it is likely a raw device driver will have higher throughput if it is able to read or write multiple bytes at a time.  On the other hand, with the aid of the library base classes, your driver will automatically (and optionally) do CRLF conversions and can be written in as little as a dozen lines of code.

There are three character device drivers your driver can inherit from:  **character_00_input**, **character_00_input_bin**, and **character_00_output**.  If your driver will do both input and output on the same file descriptor, you can inherit from both an input driver and an output driver.  The difference between **character_00_input** and **character_00_input_bin** is that the former can have an option set that will do CRLF conversion whereas the latter cannot.  The latter driver corresponds to opening files in binary "b" mode.  On the output side, **character_00_output** can also be configured to do CRLF conversion but since the code for binary mode is not impacted by additional code for CRLF, a separate binary driver is unnecessary.

== Driver Inheritance -- How it Works ==












=== The Terminal Device Driver ===

=== The Disk Device Driver ===

=== IOCTLs for Drivers ===

=== Driver Instantiation in the CRT ===

==== Interrupts ====

==== I/O ====

==== C++ STL Containers ====

==== Integer Math ====

==== Floating Point Math ====

==== Memory Allocation ====