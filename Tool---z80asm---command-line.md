## Command Line
[[Top](Tool---z80asm)]

*z80asm* is invoked with:

```
  z80asm [options]... {file|@project}...
```

Command line options start with the dash `-`. The option name and the argument,
if any, are joined together without a separator, e.g. `-Idirectory`.

Files are either source or object file names, or project list files preceeded 
by `@`. 

An input file with a `.o` extension is assumed to be already in object file
format and is just read by the linker. Any other extension is considered an 
assembly source file (conventionally `.asm`). 

The file extension may be left out to mean either assemble the `.asm` file or 
link the `.o` file, depending on what file is found.

A project list file may be supplied in the command line prefixed by the at-sign
(e.g. `@project.lst`). The list file contains one input file name per 
line, or another project list prefixed with an at-sign, which is opened 
recursively. Hash signs (`#`) and semi-colons (`;`) may be used at the 
start of lines in the list files to include comments.

Both the command line and the list files support wild-cards to expand all the 
files that match the given pattern.  

*Note* that the Operating System may do its own wildcard expansion and the 
pattern may need to be quoted in the command line.

A single star in a file name (`*`) expands to the list of all 
files/directories that match the complete pattern, where the star represents 
any sequence of characters. A double-star in a directory name (`**`) 
expands to the complete directory sub-tree, when searched recursively.

Both the command line and the list files support the syntax `${ENVVAR}` to 
expand the given environment variable in the file name.



### Help options:

#### -h

Shows a help screen with a summary of the command line options.

#### -v

Print to standard output information about what the assembler is doing and 
which files it is processing.

### Preprocessor Options:

#### -IXIY

Swap all occurrences of the `IX` and `IY` registers, useful for architectures that reserve one of the index registers for the OS.

#### -IPATH

Add the directory `PATH` to the include search path from which included files are searched.

#### -DSYMBOL[=VALUE]

Define a symbol with the given optional value, or 1 if value is not supplied.

#### -ucase

Convert all identifiers to upper case, useful to assemble files intended for case-insensitive assemblers.
  
#### -float=FORMAT

Set the default floating point format to be used by the `FLOAT` directive, see [SETFLOAT](https://github.com/z88dk/z88dk/wiki/Tool---z80asm---directives#setfloat-type)

### Code Generation Options:

#### -mARCH

Assemble for the given architecture. The following architectures are supported:

| Option   | Description
|---       |---
|-mz80     |Z80
|-mz80n    |Z80 variant of the ZX Next
|-mgbz80   |GameBoy Z80
|-m8080    |8080 with Zilog or Intel mnemonics
|-m8085    |8085 with Zilog or Intel mnemonics
|-mz180    |Z180
|-mr2ka    |Rabbit 2000A
|-mr3k     |Rabbit 3000
|-mti83plus|TI83Plus
|-mti83    |TI83

#### -opt-speed

Optimize for speed. Currently only replaces `jr` by `jp`, as the later are faster.

#### -debug

Add debug information to the `.map` file.

### Libraries:

#### -LPATH

Add the directory `PATH` to the library search path from which library files 
are searched.

#### -xFILE

Create a library file named `FILE.lib` containing all the object files referred
in the command line and included project list files.

#### -lFILE

Add `FILE.lib` to the list of library files to be searched to resolve undefined
symbols.

### Binary Output:

#### -ODIR

Write output files in the directory `DIR`.

#### -oFILE

If assembling-only (i.e. no `-b` option), generate a consolidated object file
with the name `FILE`.

If assembling and linking (i.e. with `-b` option), name the output binary file
`FILE`.

#### -b

Assemble and link to binary files based on the base name of the first
object file and with the `.bin` extension.

#### -split-bin

Normally sections that have no defined `ORG` are concatenated to the previous 
section with a defined `ORG`, and only these generate binary files.

This option causes every section to be stored in its own binary file.

#### -d

Assemble only updated files, i.e. where the `.asm` file is newer than the `.o` 
file, otherwise reuse the `.o` file.

#### -rADDR

Relocate binary file to given address (decimal or hex). Changes the `ORG` of 
the first module to the value `ADDR`.

#### -R

Create relocatable code, i.e. a relocation routine at the start of the code and
a table of addresses to be fixed at the end of the code. 

#### -reloc-info

Generate a binary `file.reloc` with relocation information to be used by a 
loader.

#### -fBYTE

Set the default byte value to fill in DEFS (decimal or hex).

### Output File Options:

#### -s

Create the symbol table `file.sym` with the list of defined symbols.

#### -l

Create the listing file `file.lis` with the assembly source and the 
corresponding object code.

### -m

Create the address map file `file.map` with the list of defined symbols, the 
result values after linking, and the source location where they where defined.

### -g

Create a global definition file `file.def`. This file contains the global 
symbols and their values after linking in a format that can be included from 
another assembly file.

#### Appmake Options:

#### +ARCH

These options allow *appmake* to be called after the linking for a few 
common architectures.

| Option   | Description
|---       |---
|+zx81     |Generate ZX81 `file.P` file with the origin at 16514
|+zx       |Generate ZX Spectrum `file.tap` file with the origin default 23760 (in a `REM` statement), but that can also be set with `-rORG` >= 24000 for placement above `RAMTOP`

## File types

These are the file types recognized or generated by *z80asm*:

| Extension | File type
|---        |--- 
| .asm      | source file
| .o        | object file
| .lis      | list file
| .bin      | binary file
| .sym      | symbols file
| .map      | map file
| .reloc    | relocation information file
| .def      | global address definition file
