For a given program, compilation speed depends on several factors:

* Compiler used and flags
* Operating system and hardware
* Target library size

Taking the classic `adv_a.c` example (over 3000 lines) and compiling using the classic `+zxn` and `+primo` libraries. The `+zxn` library file is about 1.5MB, the `+primo` is about 1MB.


| Operating System | CPU | Compiler | Wall clock time | Compile line |
|---|---|---|---|---|
| MacOS 10.14 | i7-3557U | sccz80 |  1.49s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| MacOS 10.14 | i7-3557U | sccz80 |  0.9s | `zcc +primo adv_a.c -create-app -lndos -compiler=sccz80 --generic-console` |
| Ubuntu 18.10 | i5-6260U  | sccz80 |  1.45s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| Windows 10 (Virtualised) | i5-6260U | sccz80 | 2.5s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 7.5s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sdcc -lndos` |
| Windows 10 (Virtualised) | i5-6260U | zsdcc | 7.5s | `zcc +zxn adv_a.c -clib=classic -create-app -lndos -compiler=sdcc` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 7s | `zcc +primo adv_a.c -create-app -lndos -compiler=sccz80 --generic-console -compiler=sdcc` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 43s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sdcc -lndos --max-allocs-per-node200000` |
| Windows 10 (Virtualised) | i5-6260U | zsdcc | 36s | `zcc +zxn adv_a.c -clib=classic -create-app -lndos -compiler=sdcc --max-allocs-per-node200000` |

The largest component of compilation time is the compiler chosen and the flags applied. For development purposes it's quicker to use sccz80 as the underlying compiler, however if this is undesirable, then splitting the project into smaller compilation units and using a tool that can track dependencies (such as make) to run the compilation would be a good compromise that can speed up the development cycle.
