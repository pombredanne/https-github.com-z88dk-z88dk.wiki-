# The compiler frontend: ZCC

The frontend of z88dk is called zcc, it is this that you should call if you want to do any compilations. To invoke the frontend use the command:

        zcc [flags]  [files to be compiled/linked]

The files can be either C files (.c) , preprocessed C files(.i), compiled C files (.asm), optimised compiled file (.opt) or assembled files (.o or .obj), any combination of them can be mixed together and the relevant processed done on them.

Processing of a file list is done on each file in turn (i.e. preprocess, compile, optimise, assemble) at the end all files may be linked into a single executable if desired.

Options accepted by zcc can be reported using:

        zcc -h

Details on the configuration of a port can be revealed with:

        zcc +port -specs

### Options to control the action of the frontend

```
     +[file]       Name of alternate config file 
     		   (must be the first argument)
     -a            Produce .asm (or .opt) file only
     -S            Produce .asm (or .opt) file only
     -c            Do not link object files
     -E            Preprocess files only, leave output in .i file
     -o            Specify the output name for the stage that compiling will stop
                   at. For example "-a -o file.asm" will output an assembly file.
     -bn [file]    Specify output file for binary (default is a.bas for BASIC
                   programs and a.bin for application binaries)
     -On           Optimize compiler output (to .opt file)
                   n can be either 0 (none) 1,2,3, level 2 is recommended.
		   Level 3 is suitable for large programs (includes certain
		   lib functions to reduce size of code(!))
     -v            Verbose - echo commands as they are executed
     -vn           Don't be verbose
     -clib=[lib]   Switch to the specified standard library
     -subtype=[x]  Generate output for the platform subtype x
     -compiler=X   X=sdcc or X=sccz80
```


### Options to control library usage

```
     Parameters valid for the Z88 (see the platform sections for more options)

     -Lpath        Add to the library search path
     -l[name]      Link in a library - supply just the name (after placing them
                   in the correct directory)
     -lm           Link in the generic Z80 maths library

     -m            Generate .map files when assembling/linking
     -s            Generate .sym files when assembling/linking
     --list        Generate list files
```

Other libraries are available

### Options to control the type code produced

```
     -unsigned        Implicitly define everything as unsigned unless explicitly
                      told otherwise (sccz80)
     -create-app      Create a file suitable for running on an emulator/machine
     -startup=        This parameter affects the resulting code in a target
                      dependent way: in example, when used with the Cambridge Z88
                      the -startup=3 parameter instructs the compiler to produce 
                      standalone code that can be run from a set
                      address from BASIC. (Use -zorg= to change the address)

     -pragma-define   Define an option,for example:
     -pragma-bytes    Dump a string of bytes zcc_opt.def
     -pragma-redirect Redirect a function, for example:
                      -pragma-redirect:fputc_cons=xyz123
                      will make xyz123 the assembler label that is used for console 
                      output
```

### Miscellaneous options

```
     --c-code-in-asm Intersperse C code as comments in the assembler output, warning:
                     this *will* clobber some optimisations for sccz80
     -Cp[option]   Pass an option through to the pre-processor
     -Ca[option]   Pass an option through to the assembler
     -Cl[option]   Pass an option through to the linker
     -Cz[option]   Pass an option through to appmake
     -Cc[option]   Pass an option through to sccz80
     -Cs[option]   Pass an option through to sdcc
```


In addition, the flags, -D, -I, -U are passed through to the preprocessor.

Any unrecognised options are passed through to the compiler (to allow for improvements in the future.)




## Configuration files

In order for z88dk to work on as many platforms as possible and so that it can be easily tweaked, retargetted and generally mutilated, the frontend (zcc) consults a plain text configuration file which is in the directory pointed to be the ZCCCFG variable.

The config file is indicated by the first option to zcc which is `+[machine identifier]`

If you wish to use a config file located in the current directory or anywhere else on the system then specify the full path and filename - make sure the filename as the suffix .cfg.

The order of checking is as follows:

 1.  "Local" file (if exists) eg +temp.cfg 
 2.  ZCCCFG/[name].cfg eg +z88 

