
![](images/platform/cpm.jpg)


The CP/M platform is a surprisingly useful port when used in combination with [ZXCC](https://www.seasip.info/Unix/Zxcc/), the two together can easily test whether an algorithm works, or whether there's an issue in the compiled output code.

ZXCC is trivial to extend, and you can easily permit access to serial ports, host system calls, advanced paging techniques etc testing out many areas of your code.

##  Quick Start 
	
	zcc +cpm -lm -o adventure.com adv_a.c

Without specifying a ''-o'' option, a file called ''a.com'' is produced, this can be run with zxcc as follows:

	zxcc a.com

## Creating a disc

appmake supports creating a number of CP/M disc formats that can be used directly by emulators or easily converted using 3rd party tools. Many of these can be produced as part of the compilation step using the `-subtype=XXXX -create-app` options.

To create a disc image with a binary, you can use the following command:

     appmake +cpmdisk -f [format] -b [binary file]

The file will be created on disc with a .COM extension.

The supported formats are displayed by omitting the -f option:

     appmake +cpmdisk -b [any file]



## Library Support

Besides the standard library routines, the **''bdos(int func,int arg)''** and the the **''bios(int func,int arg,int arg2)''** function calls allows direct access to the bios of the system.

## Optimization

There are a couple of #pragma commands which might be used to cut down the size of the resultant executable:

**#pragma output nostreams**      - No stdio disc files

**#pragma output nofileio**       - No fileio at all

**#pragma output noprotectmsdos** - strip the [MS-DOS protection header](platform/cpm#program_boot_protection)

**#pragma output noredir**        - do not insert the file redirection option while parsing the command line arguments (useless if "nostreams" is set)

**#pragma output nogfxglobals**   - No global variables for graphics (required to make the graphics library work on the TIKI-100)

## Hardware specific extensions

Some hardware specific functions have been adapted to run on both native platorm and generic CP/M environment (the 'gfx' prefix is used for library modules including graphics extensions only).


* **-laussie** ([Aussie Byte](Platform---Aussie))
* **-lcpccpm** ([Amstrad CPC](Platform---Amstrad-CPC))
* **-lc128cpm**, **-lgfx128**, **-lgfx128hr**, **-lgfx128hr480** ([Commodore 128](Platform---Commodore-c128))
* **-leinstein** ([Tatung Einstein](Platform---Tatung-Einstein))
* **-lpx4** ([Epson PX4 (HC-40)](Platform---Epson-px4))
* **-lmicrobee** ([MicroBEE](Platform---Microbee))
* **-lnbcpm** ([Grundy NewBrain](Platform---Grundy-Newbrain))
* **-ltiki100**, -startup=2 ([Tiki-100 (formerly Kon-Tiki)](Platform---Tiki100))
* **-ltrs80cpm**, **-lgfxtrs80**, **-lgfxeg2000** ([Tandy Radio Shack 80 and clones](Platform---TRS80))
* **-startup=3 -lzxcpm** ([Sinclair ZX Spectrum](Platform---Sinclair-ZX-Spectrum))
* **-lgfxzcn** ([Amstrad NC-100/NC-200](Platform---Amstrad-NC) 480x64 graphics on ZCN)
* **-lgfxkp**,**-lgfxkp83**  ([Kaypro](Platform---Kaypro) 160x100 graphics, 80x50 on earlier '83 models)
* **-lgfxattache**  ([Otrona Attachè](Platform---Otrona) 320x240 graphics)
* (untested) **-lgfxep**, **-lgfxephr** ([Enterprise 64 / 128](Platform---Enterprise64))
* (untested) **-lgfx9001**, **-lgfx9001krt** ([Robotron Z9001, KC85/1, KC87](Platform---Robotron-Z9001))


NOTE: for [MSXDOS](Platform---MSX), use the specific target instructions.

## Program boot protection

The code generated for CP/M based computers is protecting by default the MS-DOS environment.

In a similar way we can extend the protection to the 8080 based systems (which would crash otherwise).

"{z88dk}/lib/cpm_crt0.asm" needs to be modified as follows:


	start:
	IF !DEFINED_noprotectmsdos
		defb	$eb,$04		;DOS protection... JMPS LABE
		ex	de,hl
		jp	ckz80-start+$100
		defb	$b4,$09		;DOS protection... MOV AH,9
		defb	$ba
		defw	dosmessage	;DOS protection... MOV DX,OFFSET dosmessage
		defb	$cd,$21		;DOS protection... INT 21h.
		defb	$cd,$20		;DOS protection... INT 20h.
	
	dosmessage:
		defm	"This program is for a CP/M system."
		defb	13,10,'$'
	
	ckz80:
		ld	a,$7F			; 01111111 into accumulator
		inc	a			; make it overflow ie. 10000000
		jp	pe,begin-start+$100	; only 8080 resets for odd parity here
		ld	de,z80message		; output "Needs Z80" message
		ld	c,9
		jp	5			; BDOS
	
	z80message:
		defm	"Z80 CPU required."
		defb	13,10,'$'	
		
	begin:
	ENDIF


## CP/M related Links

http://www.seasip.info/Cpm/index.html

http://zxvgs.yarek.com/en-index.html   (ZXVGS is also natively supported)

