# Hardware summary

* Z80 @ 4 Mhz
* 64k RAM, 32k ROM 
* MC6847
* AY-3-8192

_This target is currently being brought up, it's not yet in the master branch_

# Compilation

    zcc +spc1000 world.c -create-app

The resulting .rom file can be loaded into espc1000.exe emulator:

1. Tape->Play select .spc file
2. Type `LOAD`
3. Tape->Play Button

The file will load and autorun.

# Console Features

The generic console supports both text mode (32x16) eand hires graphics (32x24). You can use the ioctl and switch between mode 0 and 1 respectively.

# Screenshots


# Limitations


# Links

* [Technical Docs](https://github.com/meesokim/spc1000/tree/master/docs)