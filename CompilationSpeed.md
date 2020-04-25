For a given program, compilation speed depends on several factors:

* Compiler used and flags
* Operating system
* Target library size

Taking the classic `adv_a.c` example (over 3000 lines) and compiling using the classic `zxn` library:


| Operating System | CPU | Compiler | Wall clock time | Compile line |
|---|---|---|---|---|
| MacOS 10.14 | i7-3557U | sccz80 |  1.49s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| MacOS 10.14 | i7-3557U | sccz80 |  0.9s | `zcc +primo adv_a.c -create-app -lndos -compiler=sccz80 --generic-console` |
| Ubuntu 18.10 | i5-6260U  | sccz80 |  1.45s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| Windows 10 (Virtualised) | i5-6260U | sccz80 | 4.6s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sccz80 -lndos` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 8.1s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sdcc -lndos` |
| Windows 10 (Virtualised) | i5-6260U | zsdcc | 19.2s | `zcc +zxn adv_a.c -clib=classic -create-app -lndos -compiler=sdcc` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 7.4s | `zcc +primo adv_a.c -create-app -lndos -compiler=sccz80 --generic-console -compiler=sdcc` |
| | | | | |
| MacOS 10.14 | i7-3557U | zsdcc | 43.6s | `zcc +zxn -clib=classic adv_a.c --generic-console -create-app -compiler=sdcc -lndos --max-allocs-per-node200000` |
| Windows 10 (Virtualised) | i5-6260U | zsdcc | 171s | `zcc +zxn adv_a.c -clib=classic -create-app -lndos -compiler=sdcc --max-allocs-per-node200000` |

The largest component of compilation time is the compiler chosen and the flags applied. For development purposes it's quicker to use sccz80 as the underlying compiler, however if this is undesirable then splitting the project into smaller compilation units and using a tool that can track dependencies (such as make) to run the compilation would be a good compromise that can speed up the development cycle.

It appears that compilation on windows performs significantly worse than Linux *on the same hardware*. (Anecdotally) this tends to also be observed using other toolchains so this may be as expected.

