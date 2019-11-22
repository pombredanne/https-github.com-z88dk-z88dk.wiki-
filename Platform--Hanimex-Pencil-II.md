# Machine Specifications

* CPU: Z80 @ 3.579 Mhz
* VDP: TMS9929, 16k VRAM
* PSG: SN76489
* RAM: 2k 
* ROM: 8k

# Compilation

    zcc +pencil2 world.c -create-app

A 32k ROM file will be generated that can be inserted into the emulator.

# Support

The hardware and port settings for this machine are virtually identical to the Colecovision, the key differences are:

- The Pencil II has a keyboard rather than joypads
- The BIOS ROM is completely different

The VDP used by the Pencil 2 is the same as the one in the MSX, as such many MSX examples will work. However, the machine is not particularly well equipped with memory so care must be taken.

You can reduce RAM usage by making static data `const` where possible - this will keep it ROM rather than copying to RAM.

The inbuilt font onboard the Pencil II is used by default by the console, it can be reconfigured using `-pragma-redirect:CRT_FONT=....`

# Keyboard

The keyboard is supported by an inkey driver and 4 keyboard joysticks are available. The delete key is mapped to Break which is the "End" key in the default Mame settings.

# Links

* Mame emulator
* https://forums.bannister.org/ubbthreads.php?ubb=showflat&Number=114969&page=5
* https://github.com/xmessner/pencil2