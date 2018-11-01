
# Quick start

    zcc +pc88 -create-app -Cz--audio program.c


This will generate three files, **a.bin** (a pure binary block), **a.t88** (a file valid on many emulators), and **a.wav**, never tested but meant to be loaded on the real hardware.



# Emulator notes

An emulator capable of loading the T88 cassette file format is necessary, the default ORG address is $8A00, it was chosen peeping into the original software written in the eighties.

To run an appmake created program, first start N88 BASIC (on the M88 emulator the cpu clock must be set to 8mhz) ot N80 BASIC, get to the prompt (answer to "files?") then:
MON
R   (or L if you are using the N80 BASIC)
G8A00

(CTRL-b would return to BASIC from monitor)
