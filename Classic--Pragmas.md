The classic library supports the use of pragmas to change the behaviour of the program at compilation time.

## Controlling the memory map

`-pragma-define:CRT_ORG_CODE=xxx`

Change the origin of the code. By default the classic library will use an appropriate location, but if targeting a non-standard machine configuration you may wish to change it.

`-pragma-define:CRT_ORG_BSS=xxx`

Change the location of the start of application RAM. This option only makes sense for ROM compiles.

`-pragma-define:REGISTER_SP=xxx`

Change the location of the stack pointer. By default the classic library will use an appropriate location, but if targeting a non-standard machine configuration you may wish to change it.

`-pragma-define:CRT_MAX_HEAP_ADDRESS=xxx`

The default for the maximum heap address (for AMALLOC) on most platforms will be the sp register. For platforms with a fragmented RAM model this may not appropriate and will need changing. The classic library should be setup with sensible defaults however.

`-pragma-define:CLIB_BALLOC_TABLE_SIZE=xx`

Configures the size of the block allocator queue.

`-pragma-define:CRT_MODEL=1`

Applicable for ROM compiles only. The data section is copied from ROM into RAM on program start.

`-pragma-define:CRT_MODEL=1`

Applicable for ROM compiles only. The data section is stored compressed in ROM and decompressed into RAM on program start.


## Startup behaviour

`-pragma-define:CRT_INITIALIZE_BSS=0`

Disable zero initialisation of the BSS memory. This can save a small amount of memory with the downside of global variables having unknown values on startup.

`-pragma-define:CLIB_EXIT_STACK_SIZE=xxx`

The classic library keeps a stack of function pointers registered with `atexit()`, depending on the target this may consume up-to 64 bytes of stack memory. If you don't use atexit() then it's quite safe to add the option `-pragma-define:CLIB_EXIT_STACK_SIZE=0` to disable it.

`-pragma-define:CRT_ENABLED_STDIO=0`

stdio is enabled by default for all targets. Enabling stdio reserves some memory for the file descriptors (stdout, stdin etc) and also utilises some memory to close all open files when the program exits. If you don't use stdio then this is a waste of memory.

NB. If the only part of stdio that you use is printf(), then you can utilise printk() which will avoid the stdio memory overhead. There are similar functions that bypass stdin for reading keypresses

`-pragma-define:CLIB_FOPEN_MAX=xx`

Changes the number of FILE structures that are allocated. The default value is 10, for systems without a storage driver this can be reduced to 3 with no ill effects.

## Generic console (VT52) behaviour

`-pragma-redirect:fputc_cons=fputc_cons_generic`

If not the default for a target, supply this pragma to enable support for the [Generic Console](Classic-GenericConsole)

`-pragma-redirect:CRT_FONT=llll`

On platforms that support it, using this pragma will allow the 32 column font used by the console to be reconfigured.

`-pragma-redirect:CRT_FONT_64=llll`

On platforms that support it, using this pragma will allow the 32 column font used by the console to be reconfigured.

`-pragma-define:CLIB_CONIO_NATIVE_COLOUR=1`

On platforms that support it, using this pragma will disable the mapping that takes place between VT52 colour sequences and the native palette. As a result, all colour parameters will refer to the native palette colour index.

# Console input

On most targets the classic library will call the firmware routines for reading the keyboard. Sometimes however, this may cause undesirable side effects or simply be too unresponsive. An increasing number of targets support `<input.h>` and the `inkey` methods for reading the keyboard

`-pragma-redirect:fgetc_cons=fgetc_cons_inkey`

Configures the `fgetc_cons()` function to use inkey() method rather than the platform firmware.

`-pragma-define:CLIB_FGETC_CONS_DELAY=xxx`

Polling the keyboard via inkey() can be a lot quicker and keys can repeat a lot quicker than usual. Configuring this option defines the number of milliseconds to wait before polling the keyboard.

`-pragma-redirect:getk=getk_inkey`

Configures the `getk()` to use the inkey() method rather than the platform firmware.

## ANSI (vt100) terminal

`-pragma-need=ansiterminal`

Enable the full VT100 terminal (if available for the target). This is normally configured using the `-clib=ansi` option. 

`-pragma-define:ansicolumns=nn`

On targets that support different numbers of columns, this option will allow compile time configuration of the number of columns to be shown on screen. Typical values are: 32, 42, 64

`-pragma-redirect:ansifont=lll`

`-pragma-define:ansifont_is_packed=0`

A font will be automatically configured, but if you wish to change it then you can use this pragma. Unless you have packed the font, then you should also supply the option `-pragma-define:ansifont_is_packed=0`