# Tandy Radio Shack TRS-80 Model 100

## Hardware summary

* OKI 80C85 @ 2.4Mhz
* 81C55 PIO
* IM6402 UART
* 8...32k contiguous RAM
* 32k system ROM/firmware
* 32k optional ROM for user software

## Compatibility note
A few machines very similar in hardware and software to M100 were produced:
- Kyotronic 85
- Olivetti M10
- NEC PC-8201
- European version of Model 100 with different PCB revision

Only **American TRS-80 Model 100 and Model 102** are supported at the moment.
Model 102 is simply a slightly slimmed down, modernized, functionally compatible redesign of Model 100.

Other types of machines (including European Model 100), have differently laid out RAM areas and system ROMs, which is currently unsupported.

## Classic library support (`+m100`)

* [x] Native console output
* [x] Native console input
* [ ] ANSI vt100 engine
* [x] Generic console
    * [ ] Redefinable font
    * [ ] UDG support
    * [ ] Paper colour
    * [ ] Ink colour
    * [ ] Inverse attribute
    * [ ] Bold attribute
    * [ ] Underline attribute
* [ ] Lores graphics
* [x] Hires graphics
* [ ] PSG sound
* [ ] One bit sound
* [ ] Inkey driver
* [ ] Hardware joystick
* [ ] File I/O
* [ ] Interrupts
* [ ] RS232


## Compilation

### Machine code RAM-based program

    zcc +m100 -subtype=default program.c -create-app

This generates `A.CO` binary to be loaded into and ran from M100 RAM.

.CO file can be loaded into VirtualT emulator through "File > Load file from HD" menu item.

.CO file can be loaded onto real hardware in different ways:
- it can be downloaded from PC to real M100 over RS232 null modem cable and [special software](http://bitchin100.com/wiki/index.php?title=LaddieCon&oldid=2975);
- Alternatively, it can be put onto an external storage device such as [NADSBox](http://www.kenpettit.com/nadsbox.html);
- There's a possibility of generating a BASIC loader from .CO file - this feature exists in VirtualT IDE for machine code development, but it doesn't (?) exist anywhere separately.

Once the program is in memory, enter BASIC and give the following command:

    CLEAR 0,49999

now the program can be loaded, either via the BASIC command `RUNM "A.CO` or by pressing F8 to get back to the menu and choosing it with the cursors.

### Option ROM

    zcc +m100 -subtype=optrom program.c -create-app

This generates `a.rom` binary ROM image to be used as M100 optional ROM.


ROM image can be loaded into VirtualT emulator through "Emulation > Option ROM" menu.

ROM image can be put on real hardware by different means:
- it can be used with some memory upgrade options, both obsolete and available, such as [ReMem](http://bitchin100.com/wiki/index.php?title=Remem&oldid=2936) or [REX](http://bitchin100.com/wiki/index.php?title=REX&oldid=3251);
- it can be burned onto some kind of ROM and put into an "option ROM" socket (or into [system ROM adapter](http://bitchin100.com/wiki/index.php?title=M100ROM&oldid=2938)). It most likely will require an adapter PCB as M100 option ROM socket doesn't accept standard 28-pin 32k JEDEC pinout and has its own proprietary pinout.

Once the ROM is attached/loaded, it can be launched from BASIC using: 

    CALL63012

_**NOTE:** Only 320 bytes of RAM space is available directly for DATA and BSS section for option ROM software at the moment.
CODE segment doesn't exist in RAM on this target, therefore space for other sections is not being reserved anymore by the mere presence of the entire program image in RAM. Absolute memory space for DATA and BSS segments would need to be claimed through interaction with OS, which is not implemented yet._
Those 320 bytes are located in ALTLCD area and should not cause any unintended consequences to M100 OS
workings - this area usually keeps the text of TELCOM's previous screen and can be safely overwritten when it's not in use.

# Links

* [Club 100: A Model 100 User Group](http://www.club100.org/)
* [Bitchin100 DocGarden](http://bitchin100.com/wiki/index.php?title=Bitchin100_DocGarden)
* [VirtualT emulator](https://sourceforge.net/projects/virtualt/)
* [CloudT online emulator](http://bitchin100.com/CloudT)




