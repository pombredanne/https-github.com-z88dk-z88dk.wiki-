## Environment
[[Top](Tool---z80asm)]

*z80asm* reads the following environment variables, if present:

* **Z80ASM** - contains a space separated list of command line options that are passed to every invocation of *z80asm*. Useful to pass `-v` (verbose) to a tool that calls *z80asm*, e.g.

```
  Z80ASM=-v zcc [options] [files]
```

* **ZCCCFG** - points to the directory where `zcc` keeps its configuration files; *z80asm* searches the parent of this directory for libraries to link.

In the command line and in project files, *z80asm* can expand the contents of environment variables, e.g. `${ARCH}/printf`
tells *z80asm* to include an object module called `printf` from a directory pointed to by the environment variable `ARCH`.
