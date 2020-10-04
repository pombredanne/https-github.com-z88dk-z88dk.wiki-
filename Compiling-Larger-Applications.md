Most example compilations in the z88dk documentation and examples are single line examples which can be copy/pasted to generate the same result.

For smaller applications where all code is located in a single file, this is the easiest way of compiling, however for larger applications, especially those compiled with zsdcc, this can result in a long code-compile-run development cycle which isn't very productive.

Both the easiest and hardest way to achieve incremental compilation is using a `Makefile`, a simple example is below:

```
# Do not use this makefile, it has subtle problems
CFILES = $(wildcard *.c)
OFILES = $(CFILES:.c=.o)

PROGRAM = program.bin

all: $(PROGRAM)


program.bin: $(OFILES)
        zcc +zx -o $@ $(OFILES) -create-app

%.o: %.c
        zcc +zx -o $@ -c $^

clean:
        $(RM) *.o program.bin
```

### Pragma support

When a source file is processed, the pragmas are parsed and written out to the `zcc_opt.def` file. Unfortunately this file is cleaned on each invocation of zcc so using our makefile above will cause the pragmas to be in used at the linking stage.

zcc supports the `-pragma-include=` option which should be used to specify a file which contains all pragma directives needed by your application. We can now extend our makefile to use this file:

```
# Valid for classic compilations, newlib may need small tweaks
CFILES = $(wildcard *.c)
OFILES = $(CFILES:.c=.o)

PROGRAM = program.bin

all: $(PROGRAM)


program.bin: $(OFILES)
        zcc +zx -o $@ $(OFILES) -create-app -pragma-include:zpragma.inc

%.o: %.c
        zcc +zx -o $@ -c $^

clean:
        $(RM) *.o program.bin
```

### Output binary name

The output name specified with the `-o` option at the linking stage is the name of the file that is to be produced by the linker, not that generated by the `appmake` tool. 

**Note:** For newlib +zxn compilations the -o option should be given without the file extension (so `-o $(basename $@)` for the makefile above.

## Compiling for many targets (classic)

The makefile example detailed above is a minimal example to compile a project for a single z88dk target. However, many z88dk projects, in particular those that use classic, aim to write portable code that can be run on many targets. If we assume a project with the following structure:

```
main.c
file.c
zx/extra.c
zx/zpragma.inc
multi8/extra.c
```

That is common code in a root directory, with subdirectories and specific code for each target then we can see that we could create some generalised rules that could compile our application for any target. A usable makefile is located within `{z88dk}/support/multitarget_build`. This makefile has only 3 values that need to be modified:

```makefile
# Targets that you want to compile for
TARGETS ?= zx multi8

# Name of the output binary
APPNAME := program.bin

# Source files shared between all z88dk targets
COMMON_SRCS := $(wildcard *.c) $(wildcard *.asm)
```

Typically, you probably only need to change `TARGETS` and `APPNAME` if desired.

To build for all of your selected targets just type `make`. To print out the commands as they are running  then enter `make Q=`. If you wish to just build for a particular target then enter`make zx` or `make multi8` for example.

The following files are created in `build/`when make is invoked:

```
build/zx/main.o
build/zx/zx/extra.o
build/zx/program.map
build/zx/program_BANK_7.bin
build/zx/program.bin
build/zx/func.o
build/zx/program.tap
build/multi8/main.o
build/multi8/program.map
build/multi8/program.bin
build/multi8/program.cas
build/multi8/func.o
build/multi8/multi8/extra.o
```

### Advanced Tuning

Despite the best intentions, it's almost inevitable that extra options will need to be supplied at the compiler or linking stages. 

For `#define` values, if we look at the compilation line given for a .c file:

```shell
zcc +zx  -Izx -o build/zx/func.o -c func.c
```

Then we can see that the local target/ directory (in this case) `zx` is added to the include search path. Thus we can easily create a file named, say, `defines.h`, include a copy in each target directory and `#include` it.

Alternatively, and at the risk of complicating the makefile, any compilation flags can be supplied using makefile variables (where `{target}` is replaced by the appropriate target and `{basename}` is the `APPNAME` without an extension).

```makefile
CFLAGS_{target} = 
LDFLAGS_{target} =
CFLAGS_{basename} = 
LDFLAGS_{basename} =
CFLAGS_{target}_{basename} = 
LDFLAGS_{target}_{basename} =
```

### Multiple projects in the same makefile

Using this formula, it's possible to compile multiple projects for multiple targets using a single makefile. For an example see https://github.com/z88dk/z88dk/blob/master/examples/console/Makefile