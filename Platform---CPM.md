
![](images/platform/cpm.jpg)


The CP/M platform is a surprisingly useful port when used in combination with [ZXCC](http://www.seasip.demon.co.uk/Unix/Zxcc/), the two together can easily test whether an algorithm works, or whether there's an issue in the compiled output code.

ZXCC is trivial to extend, and you can easily permit access to serial ports, host system calls, advanced paging techniques etc testing out many areas of your code.

##  Quick Start 

	
	zcc +cpm -lm -o adventure.com adv_a.c


Without specifying a ''-o'' option, a file called ''a.com'' is produced, this can be run with zxcc as follows:

	
	zxcc a.com



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


      * **-laussie** ([Aussie Byte](platform/aussie))
      * **-lcpccpm** ([Amstrad CPC](platform/amstradcpc))
      * **-lc128cpm**, **-lgfx128**, **-lgfx128hr**, **-lgfx128hr480** ([Commodore 128](platform/c128))
      * **-leinstein** ([Tatung Einstein](platform/einstein))
      * **-lpx4** ([Epson PX4 (HC-40)](platform/px4))
      * **-lmicrobee** ([MicroBEE](platform/microbee))
      * **-lnbcpm** ([Grundy NewBrain](platform/newbrain))
      * **-ltiki100**, -startup=2 ([Tiki-100 (formerly Kon-Tiki)](platform/tiki100))
      * **-ltrs80cpm**, **-lgfxtrs80**, **-lgfxeg2000** ([Tandy Radio Shack 80 and clones](platform/trs80))
      * **-startup=3 -lzxcpm** ([Sinclair ZX Spectrum](platform/zx))
      * **-lgfxzcn** ([Amstrad NC-100/NC-200 480x64 graphics on ZCN](platform/amstradnc))
      * **-lgfxkp** ([Kaypro 160x100 graphics](platform/kaypro), excluding earlier '83 models)
      * (untested) **-lgfxep**, **-lgfxephr** ([Enterprise 64 / 128](platform/enterprise))
      * (untested) **-lgfx9001**, **-lgfx9001krt** ([Robotron Z9001, KC85/1, KC87](platform/z9001))


NOTE: for [MSXDOS](platform/msx), use the specific target instructions.

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


