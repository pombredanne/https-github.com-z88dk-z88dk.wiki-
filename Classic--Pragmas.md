The classic library supports the use of pragmas to change the behaviour of the program at compilation time.

An example of a compiler-generated pragma with the classic C library is sccz80's scan of printf format strings.  sccz80 will keep track of what format specifiers are used and determine whether the compile can use a simple printf, medium printf or large printf implementation.  This way the compiler can automatically reduce the size of the output binary by selecting the smallest printf implementation allowable.

These pragmas are written to a file (temporary) "zcc_opt.def" as assembler define directives during compilation.  When an executable is generated, the crt is assembled as part of the last step of the compile and after "zcc_opts.def" is complete.  The crt includes the "zcc_opt.def" file and uses the options specified to perform whatever initialization is necessary to implement the options. 

This file is started afresh for each invocation of zcc, so if you are using a single line compilation then you can write pragmas within source files. However, when using a makefile the project is normally split into many source files that are individually compiled to object files, as a result it's recommended that all `#pragma` statements should be located into a single file, say `zpragma.inc` and an linker option added: `-pragma-include:zpragma.inc`.

The pragmas below are in the format specified on the command line, to place them in a source file, apply the following syntax:

* `#pragma define NAME = value`
* `#pragma redirect NAME = value`

## Controlling the memory map

| Option | Description |
|-|-|
| `-pragma-define:CRT_ORG_CODE=xxx` | Change the origin of the code. By default the classic library will use an appropriate location, but if targeting a non-standard machine configuration you may wish to change it. |
| `-pragma-define:CRT_ORG_BSS=xxx` | Change the location of the start of application RAM. This option only makes sense for ROM compiles. |
| `-pragma-define:REGISTER_SP=xxx` | Change the location of the stack pointer. By default the classic library will use an appropriate location, but if targeting a non-standard machine configuration you may wish to change it. |
| `-pragma-define:CRT_MAX_HEAP_ADDRESS=xxx` | The default for the maximum heap address (for AMALLOC) on most platforms will be the sp register. For platforms with a fragmented RAM model this may not appropriate and will need changing. The classic library should be setup with sensible defaults however. |
| `-pragma-define:CLIB_BALLOC_TABLE_SIZE=xx` | Configures the size of the block allocator queue. |
| `-pragma-define:CRT_MODEL=1` | Applicable for ROM compiles only. The data section is copied from ROM into RAM on program start. |
| `-pragma-define:CRT_MODEL=2` | Applicable for ROM compiles only. The data section is stored compressed in ROM and decompressed into RAM on program start. The compression used is zx7. |
| `-pragma-define:CRT_MODEL=3` | Applicable for ROM compiles only. The data section is stored compressed in ROM and decompressed into RAM on program start. The compression used is zx0. zx0 yields better compression results than zx7. |

## Controlling the output section names

The following pragmas are available in both sccz80 and zsdcc:

* `#pragma codeseg name`
* `#pragma constseg name`

The following are only available in sccz80 (see [Issue #156](https://github.com/z88dk/z88dk/issues/156))

* `#pragma bssseg name`
* `#pragma dataseg name`
* `#pragma initseg name`

## Startup behaviour

| Option | Description |
|-|-|
| `-pragma-define:CRT_INITIALIZE_BSS=0` | Disable zero initialisation of the BSS memory. This can save a small amount of memory with the downside of global variables having unknown values on startup. |
| `-pragma-define:CLIB_EXIT_STACK_SIZE=xxx` | The classic library keeps a stack of function pointers registered with `atexit()`, depending on the target this may consume up-to 64 bytes of stack memory. If you don't use atexit() then it's quite safe to add the option `-pragma-define:CLIB_EXIT_STACK_SIZE=0` to disable it. |
| `-pragma-define:CRT_ENABLE_STDIO=0` | stdio is enabled by default for all targets. Enabling stdio reserves some memory for the file descriptors (stdout, stdin etc) and also utilises some memory to close all open files when the program exits. If you don't use stdio then this is a waste of memory. If you don't need any conio screen output (for example if you're using the SP1 print routines) then `-pragma-define:fputc_cons=0` will be useful to prevent a screen printer being included. NB. If the only part of stdio that you use is printf(), then you can utilise printk() which will avoid the stdio memory overhead. There are similar functions that bypass stdin for reading keypresses |
| `-pragma-define:CLIB_FOPEN_MAX=xx` | Changes the number of FILE structures that are allocated. The default value is 10, for systems without a storage driver this can be reduced to 3 with no ill effects. |
| `-pragma-define:CLIB_OPEN_MAX=xx`. | When generating CP/M and MSXDOS1 applications configures the number of FCBs allocated by the program. This defaults to 3. If set to 0, then it's recommended that `-lndos` is added to the command line. |
| `-pragma-define:CRT_ENABLE_COMMANDLINE=0` | Disables gathering argc and argv for main on that targets that support command line arguments. When this pragma is supplied, argc=0 and argv=NULL are supplied to the main function. |
| `-pragma-define:CRT_COMMANDLINE_REDIRECTION=0` | Disables the file redirection ('<', '>') behaviour when parsing command line arguments.  |
| `-pragma-define:CLIB_DEFAULT_SCREEN_MODE=n` | Sets the startup screen mode, not available for all platforms |

## Generic console (VT52) behaviour

| Option | Description |
|-|-|
| `-pragma-redirect:fputc_cons=fputc_cons_generic` or `--generic-console` | If not the default for a target, supply this pragma to enable support for the [Generic Console](Classic-GenericConsole). The following command line alias may prove easier to type: `--generic-console` |
| `-pragma-redirect:CRT_FONT=llll` | On platforms that support it, using this pragma will allow the 8x8 pixel font used by the console to be reconfigured. |
| `-pragma-redirect:CRT_FONT_64=llll` | On platforms that support it, using this pragma will allow the 4x8 pixel font used by the console to be reconfigured. |
| `-pragma-define:CLIB_CONIO_NATIVE_COLOUR=1` | On platforms that support it, using this pragma will disable the mapping that takes place between VT52 colour sequences and the native palette. As a result, all colour parameters will refer to the native palette colour index. |

## Console input

On most targets the classic library will call the firmware routines for reading the keyboard. Sometimes however, this may cause undesirable side effects or simply be too unresponsive. An increasing number of targets support `<input.h>` and the `inkey` methods for reading the keyboard

| Option | Description |
|-|-|
| `-pragma-redirect:fgetc_cons=fgetc_cons_inkey` or `--hardware-keyboard` | Configures the `fgetc_cons()` function to use inkey() method rather than the platform firmware. |
| `-pragma-define:CLIB_FGETC_CONS_DELAY=xxx` | Polling the keyboard via inkey() can be a lot quicker and keys can repeat a lot quicker than usual. Configuring this option defines the number of milliseconds to wait before polling the keyboard. |
| `-pragma-redirect:getk=getk_inkey` | Configures the `getk()` to use the inkey() method rather than the platform firmware. |
| `-pragma-define:CLIB_DISABLE_FGETS_CURSOR=1` | Configures the `fgets_cons()` function to not print it's soft cursor (`_`) - you may need to set this if you're using firmware routines that implement their own cursor (for example on CP/M, MSX). |
| `-pragma-define:CLIB_KBHIT_DELAY=nnn` | Depending on how they are used, the kbhit/getch combination may return the same key in quick succession. This option is useful for slowing down the sequence, particularly on targets where the keyboard routines are `in_Inkey` based. |

## ANSI (vt100) terminal

| Option | Description |
|-|-|
| `-pragma-need=ansiterminal` |  Enable the full VT100 terminal (if available for the target). This is normally configured using the `-clib=ansi` option.  |
| `-pragma-define:ansicolumns=nn` | On targets that support different numbers of columns, this option will allow compile time configuration of the number of columns to be shown on screen. Typical values are: 32, 42, 64 |
| `-pragma-redirect:ansifont=lll` | |
| `-pragma-define:ansifont_is_packed=0` | A font will be automatically configured, but if you wish to change it then you can use this pragma. Unless you have packed the font, then you should also supply the option `-pragma-define:ansifont_is_packed=0` |


## Configuring printf and scanf converters

Both newlib and classic link in only the required converters for printf and scanf. If you compile using sccz80, then it will endeavour to configure the converters automatically for single line compilations, however if you use incremental builds or zsdcc then you will need to configure the list of converters that are required.

    #pragma printf = "<list printf converters here including l or ll for long and longlong>"
    #pragma scanf  = "<list scanf converters here including l or ll for long and longlong>"

Example:

    #pragma printf = "%s %c %u %f"     // enables %s, %c, %u, %f only
    #pragma scanf  = "%s %lx %lld %["  // enables %s, %lx, %lld, %[ only

The converter string content is flexible. You can insert spaces, do away with quotes, include % signs and so on. ???%s%c???, ???s c???, ???%0d %llx??? are all valid. Digits and flag specifiers (+, -, *) have no effect on the new c library but the classic c library will distinguish between ???%0d??? and ???%d??? with the latter meaning use the simple %d without any formatting. 

## Configuring rst $XX behaviour

For ROM and RAM targets that generate code starting from address $0000 it is possible to configure the behaviour the z80 restarts at (0x08, 0x10, 0x18, 0x20, 0x28, 0x30), the im1 interrupt service routine at (0x38) and the nmi interrupt service routine at (0x66). 

The default behaviour for the restarts is simply "ret". Either the im1 or the nmi will be hooked to provide a raster interrupt against which handlers can be registered with `add_raster_int()`. However, **suitably setting bits in CRT_ENABLE_RST and CRT_ENABLE_NMI will indicate to the crt that the program will implement one or more of these restarts or isrs.**  Where relevant bits are set, the crt will generate jumps to a user-supplied function implementing the restart or interrupt routine.

| CRT CONFIG OPTION | BITMASK                   | C                      | ASM          | COMMENT                                                                                                                                |
| --- | --- |--- |--- |--- |
| CRT_ENABLE_RST    | 0x02                      | void z80_rst_08h(void) | _z80_rst_08h | User implements rst08h                                                                                                                 |
|                   | 0x04                      | void z80_rst_10h(void) | _z80_rst_10h | User implements rst10h                                                                                                                 |
|                   | 0x08                      | void z80_rst_18h(void) | _z80_rst_18h | User implements rst18h                                                                                                                 |
|                   | 0x10                      | void z80_rst_20h(void) | _z80_rst_20h | User implements rst20h                                                                                                                 |
|                   | 0x20                      | void z80_rst_28h(void) | _z80_rst_28h | User implements rst28h                                                                                                                 |
|                   | 0x40                      | void z80_rst_30h(void) | _z80_rst_30h | User implements rst30h                                                                                                                 |
|                   | 0x80                      | void z80_rst_38h(void) | _z80_rst_38h | User implements rst38h aka the im1 isr.  If used as an im1 isr the user routine must preserve register values and exit with "ei; reti" |
|                   |                           |                        |              |                                                                                                                                        |
| CRT_ENABLE_NMI    | 0x02                      | void z80_nmi(void)     | _z80_nmi     | User implements the nmi isr.  The user routine must preserve register values and exit with "retn".

These **restarts and isrs can be implemented in either C or asm using the function names shown** in the table.  

**SDCC and sccz80 implement a C extension that allows C subroutines to be declared as interrupt service routines.**  The three examples below show how to implement the im1 and nmi isrs along with the generated code using this extension.

| c code                                                                   | output asm                                                                           | comments                         |
|---|---|---|
| `void z80_rst_38h(void) __critical __interrupt(0)  {   ...   }` | _z80_rst_38h:  \\ push used-registers  \\ ...  \\ pop used-registers  \\ ei  \\ reti | im0, im1 or im2                  |
| `void z80_rst_38h(void) __interrupt   {   ...   }`               | _z80_rst_38h:  \\ ei  \\ push used-registers  \\ ...  \\ pop used-registers  \\ reti | im2 with prioritized daisy chain |
| `void z80_nmi(void) __critical __interrupt   {   ...   } `       | _z80_nmi:  \\ push used-registers  \\ ...  \\ pop used-registers  \\ retn            | nmi                              |

**IMPORTANT NOTE:  The compiler generated routines only preserve the main register set and index registers. However the library takes full advantage of the z80 architecture, including the exx set, so if the isr calls a library function that modifies the exx set, the generated code will not preserve those registers. It may be tempting to inline some asm that will push and pop the exx set registers inside the function but this will not work if there are local variables present because this will alter their offsets on the stack without sdcc's knowledge.                                   |
