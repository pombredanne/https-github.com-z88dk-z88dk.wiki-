Each target has a library configuration file that selects among various options when the target's library is built.  The defaults are suitable for most projects but if you would like to experiment with generating smaller or faster code the options can be edited and the library re-built.  Because the config file belongs to the target, only that target's library is affected.

All information concerning a particular port is found in [~/libsrc/_DEVELOPMENT/target](https://github.com/z88dk/z88dk/tree/master/libsrc/_DEVELOPMENT/target).   It directly supports eleven targets currently ([cpm](https://github.com/z88dk/z88dk/wiki/Platform---CPM), [hbios](https://github.com/wwarthen/RomWBW/blob/master/Doc/RomWBW%20Architecture.pdf), [rc2014](https://rc2014.co.uk/), [scz180](https://smallcomputercentral.wordpress.com/sc130-z180-motherboard/), [sega master system](https://en.wikipedia.org/wiki/Master_System), [vgl](https://hackaday.io/project/166921-v-tech-genius-leader-precomputer-hacking), [yaz180](https://github.com/feilipu/yaz180), [z80](https://www.z88dk.org/wiki/doku.php?id=libnew:target_embedded), z180, [zx spectrum](https://en.wikipedia.org/wiki/ZX_Spectrum), and [zx spectrum next](https://www.specnext.com/)).  We will use the "z80" target for discussion purposes since it's a generic target suitable for any z80 machine.

Two library configuration files along with backups with default settings can be found in the `target/z80/config` subdirectory:

  * [config_clib.m4](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/config/config_clib.m4)

  * [config_target.m4](https://github.com/z88dk/z88dk/blob/master/libsrc/_DEVELOPMENT/target/z80/config/config_target.m4)

The asm files contain the active configuration.

## clib_cfg.asm

Contains general library settings.

| Name | Description |
|---|---|
| CLIB_OPT_MULTITHREAD    | Leave disabled.  The library does not support multi-threading yet. |
| CLIB_OPT_IMATH          | Choose between small and fast integer library. |
| CLIB_OPT_IMATH_FAST     | If the fast integer library is selected you can enable leading zero elimination and loop unrolling.  Loops are unrolled a maximum of eight times.  The LIA-1 option is not fully supported by the compilers at this time. |
| CLIB_OPT_IMATH_SELECT   | Choose between small and fast integer shifting. |
| CLIB_OPT_TXT2NUM        | Enable specialized functions for ascii -> binary, octal, decimal and hex conversion. |
| CLIB_OPT_TXT2NUM_SELECT | For enabled specialized functions choose between small and fast implementations. |
| CLIB_OPT_NUM2TXT        | Enable specialized functions for binary, octal, decimal and hex -> ascii conversion. |
| CLIB_OPT_NUM2TXT_SELECT | For enabled specialized functions choose between small and fast implementations. |
| CLIB_OPT_STDIO          | Set this to one if you want stdio to check the validity of a FILE* before using it. |
| CLIB_OPT_PRINTF         | Individually enable or disable printf converters; this can help to reduce program size<sup>1</sup>. |
| CLIB_OPT_SCANF          | Individually enable or disable scanf converters; this can help to reduce program size<sup>2</sup>. |
| CLIB_OPT_FASTCOPY       | Options to increase the speed of memcpy and memset using unrolled ldir; some other library code will also benefit. |
| CLIB_OPT_STRTOD         | Enable parsing of nan/inf strings and hex floats in form -0xh.hhhhp-dd |
| CLIB_OPT_SORT           | Selects the sorting algorithm used by qsort(). The default is shellsort<sup>3</sup>. |
| CLIB_OPT_SORT_QSORT     | Various quicksort algorithm options. |
| CLIB_OPT_ERROR          | Determines how detailed error strings are; reducing detail can help save a few bytes. |

<sup>1</sup> All printf float converters "%aefg" are disabled by default.  stdlib's dtoa() family of functions can also be used to convert floats to text.

<sup>2</sup> scanf does not support "%aefg" at this time.  stdlib's strtod() and atof() can be used to convert text to floats.

<sup>3</sup> Shellsort is currently non-reentrant.

## clib_target_cfg.asm

Contains settings for target-specific portions of the library.  Options will vary according to architecture but these two will always be present:

| Name | Description |
|---|---|
| clock_freq   | The target's cpu clock rate in Hz<sup>1</sup>. |
| z80_cpu_info | Indicate whether a CMOS or NMOS cpu is used<sup>2</sup>. |

<sup>1</sup> The cpu clock rate is used to generate precise delays and to generate tones in the sound library.

<sup>2</sup> NMOS z80s have a bug that doesn't allow the current maskable interrupt state to be reliably determined with the "ld a,i" instruction.  If an NMOS z80 is indicated the library will build with more robust code to determine that information.  If the generated code should be correct for all z80s, NMOS should be chosen.

## Rebuilding the Library

Once the library's configuration has been edited, the target's library __must__ be rebuilt in order for changes to take effect.

### Windows ###

  * Navigate to {z88dk}/libsrc/_DEVELOPMENT.
  * Run Winmake
    * "`Winmake`" lists all targets
    * "`Winmake all`" builds all target libraries
    * "`Winmake {target}`" builds a specific target's libraries

To rebuild the z80 library, "Winmake z80" should be run.

### Non-Windows ###

  * Navigate to {z88dk}/libsrc/_DEVELOPMENT.
  * Invoke the Makefile with suitable target specified.
    * "`make all`" builds all target libraries
    * "`make job-{target}`" builds a specific target's libraries

To rebuild the z80 library, "make job-z80" should be run.