# SG Tools library

For updated notes on the actual port for the Z88dk, see the [Commodore 128 target](platform/c128) page.



*  [CIA](library/c128/cia) ([cia.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/c128/cia.h))

*  [PCX image decoding](library/c128/pcx) ([pcx.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/c128/pcx.h))

*  [SID](library/c128/sid) ([sid.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/c128/sid.h))

*  [VDC](library/c128/sid) ([vdc.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/c128/vdc.h))


======= Original doc and credits follow. ======

Note that Steve Goldsmith's work is meant to run with the Hitech C compiler, don't bother him for z88dk related issues !

	
	___________________________________________________________________
	
	                         SG C Tools 1.7
	                    (C) 1994 Steve Goldsmith
	                       All Rights Reserved
	___________________________________________________________________
	
	
	OVERVIEW
	
	SG C Tools ANSI C library unlocks the power of the C128 running
	CP/M!  High level and low level functions are included to access
	the VDC, SID and CIA.  All source code is compatible with the
	Freeware version of Hi Tech C.  A demo is included to show you how
	to use most features.
	
	I have not had much time to add more features to SG C Tools.  I'm
	operating 3 AS/400s for 9 hours.  I also support several PC Pascal
	tool kits that make real money, so I've focused on them too.  What
	all this means is that you now have the complete source code to SG
	C Tools!  Hack it and crack it, but do not try to sell it.  Send me
	any cool programs you develop with it via Internet UU encoded E-
	mail or Mail to my address at end of doc.
	
	The ellipse functions and demo were the only modification between
	1.6 and 1.7.  Ellipses in 640 X 480 interlace mode no longer leave
	gaps with larger A,B values.  Just compile VDCELLPI.C and
	VDCELLPI.C, use LIBR to replace the .OBJ files in the C128LIB.LIB.
	
	
	REQUIREMENTS
	
	A C128 or C128D running CP/M 3.0 or a IBM PC running a CP/M
	emulator and DOS to CP/M file transfer software.
	
	At least one 1581, two 1571s, large RAM disk or hard drive to
	compile on a C128.
	
	80 column monitor for VDC specific routines.  640 X 480 interlace
	graphics requires a monitor that can handle this format.
	
	SG C Tools 1.6 (SGTOOL16.ARC).
	
	ANSI C programming experience.
	
	
	LICENSE AGREEMENT
	
	PLEASE FEEL FREE TO (i) UPLOAD THIS SOFTWARE TO ANY ELECTRONIC
	BULLETIN BOARD, (ii) DEMONSTRATE THE SOFTWARE AND ITS CAPABILITIES
	OR (iii) GIVE COPIES TO POTENTIAL USERS, SO THAT OTHERS MAY HAVE
	THE OPPORTUNITY TO OBTAIN A COPY FOR USE IN ACCORDANCE WITH THE
	LICENSE TERMS CONTAINED IN THIS FILE.
	
	NOTICE TO USER:  CAREFULLY READ THE FOLLOWING LEGAL AGREEMENT.
	
	1.   LICENSE GRANT.  Steven P. Goldsmith grants to you, as an
	individual, a non-exclusive right to use one copy of the SOFTWARE
	associated with this license for personal use on your computer. 
	This license to use the SOFTWARE is conditioned upon your
	compliance with the terms of this Agreement.  You are entitled to
	use the software in any way you see fit as long as there is NO
	charge.
	
	2.   COPYRIGHT.  The SOFTWARE is protected by United States
	copyright law and international treaty provisions.  You acknowledge
	that no title to the intellectual property in the SOFTWARE is
	transferred to you.  You further acknowledge that title and full
	ownership rights to the SOFTWARE will remain the exclusive property
	of Steven P. Goldsmith or its suppliers, and you will not acquire
	any rights to the SOFTWARE except as expressly set forth in this
	license.  You agree that any copies of the SOFTWARE will contain
	the same proprietary notices which appear on and in the SOFTWARE.
	
	3.   LIMITED WARRANTY.  Steven P. Goldsmith warrants that the
	SOFTWARE will perform substantially in accordance with the
	accompanying written materials for a period of ninety (90) days
	from the date of purchase.  Any implied warranties relating to the
	SOFTWARE are limited to ninety (90) days.
	
	4.   STEVEN P. GOLDSMITH DOES NOT WARRANT THAT THE SOFTWARE IS
	ERROR FREE.  STEVEN P. GOLDSMITH DISCLAIMS ALL OTHER WARRANTIES
	WITH RESPECT TO THE SOFTWARE, EITHER EXPRESS OR IMPLIED, INCLUDING
	BUT NOT LIMITED TO IMPLIED WARRANTIES OF MERCHANTABILITY, FITNESS
	FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT OF THIRD PARTY RIGHTS.
	
	SOME JURISDICTIONS DO NOT ALLOW THE EXCLUSION OF IMPLIED WARRANTIES
	OR LIMITATIONS ON HOW LONG AN IMPLIED WARRANTY MAY LAST, OR THE
	EXCLUSION OR LIMITATION OF INCIDENTAL OR CONSEQUENTIAL DAMAGES, SO
	THE ABOVE LIMITATIONS OR EXCLUSIONS MAY NOT APPLY TO YOU.  THIS
	WARRANTY GIVES YOU SPECIFIC LEGAL RIGHTS AND YOU MAY ALSO HAVE
	OTHER RIGHTS WHICH VARY FROM JURISDICTION TO JURISDICTION.
	
	5.   NO LIABILITY FOR CONSEQUENTIAL DAMAGES.  IN NO EVENT SHALL
	STEVEN P. GOLDSMITH OR ITS SUPPLIERS BE LIABLE TO YOU FOR ANY
	CONSEQUENTIAL, SPECIAL, INCIDENTAL OR INDIRECT DAMAGES OF ANY
	KIND ARISING OUT OF THE DELIVERY, PERFORMANCE OR USE OF THE
	SOFTWARE, EVEN IF STEVEN P. GOLDSMITH HAS BEEN ADVISED OF THE
	POSSIBILITY OF SUCH DAMAGES.  IN NO EVENT WILL STEVEN P.
	GOLDSMITH'S LIABILITY FOR ANY CLAIM, WHETHER IN CONTRACT, TORT OR
	ANY OTHER THEORY OF LIABILITY, EXCEED THE LICENSE FEE PAID BY YOU,
	IF ANY.
	
	6.   ENTIRE AGREEMENT.  This is the entire agreement between you
	and Steven P. Goldsmith which supersedes any prior agreement or
	understanding, whether written or oral, relating to the subject
	matter of this license.
	
	Commodore 128 is a trademark of Commodore Business Machines.  CP/M
	and CP/M 3.0 are trademarks of Digital Research.  MS-DOS and
	Windows are trademarks of Microsoft.  IBM PC,  PC-XT and PC-AT are
	trademarks of International Business Machines.
	
	
	COMPILING DEMO WITH HI-TECH C 3.09
	
	First you will need to have a copy of HI-TECH C 3.09 (CP/M-80). 
	You can download the following Freeware files from GEnie's CP/M RT
	(M685;3) or other source:
	
	 8149 LIBSRC.LZH               X BRIAN-CPM    930616   80384     39 
	 3
	      Desc: Source Code to Hitech C library
	 8148 Z80V309.LZH              X BRIAN-CPM    930616  194304     51 
	 3
	      Desc: Hitech C Compiler for CP/M
	 8147 Z80DOC.LZH               X BRIAN-CPM    930616   89088     55 
	 3
	      Desc: Documentation for Hitech C compiler
	
	You should look over HI-TECH C's manual and compile a simple "hello
	world" program before proceeding.  I installed all the compiler
	related files on my 1581.  My source and header files are on a
	1571.  You could get away with two 1571s, but a RAM disk or hard
	drive would be better.  You could also use a faster CP/M system or
	a Z80 emulator to edit and compile code on other platforms.
	
	To build LIBC128.LIB make sure all files in LIBC128.ARC reside in
	the same drive and user as HTC.
	
	SUBMIT LIB 
	
	Assembles, compiles and creates LIBC128.LIB.
	
	To compile demo program use:
	
	C -O -X DEMO.C -LC128
	
	
	PROGRAMMING CONSIDERATIONS
	
	The VDC should be configured to its default CP/M 3.0 settings at
	the start and end of each program.
	
	Display memory        0000h
	Attribute memory      0800h
	Character definitions 2000h
	
	This includes restoring character definitions if you use memory at
	2000h for bit maps, etc.  Setting the VDC to 64K mode wipes out
	memory used in 16K mode, so be sure to save the character
	definitions to a memory buffer or file before using 64K mode.  The
	VDC remains in 64K mode until you do a cold boot or warm boot with
	the C128's reset button.
	
	Most of the VDC functions do not check parameters for range
	violations.  Range checking should be preformed at the application
	level.  There are times when you may want to write to a off screen
	memory location.  For this reason there is no need to waste time
	and code doing range checks.  Just be aware of the implications. 
	A renegade program may accidentally wipe out character definitions
	or other important memory regions forcing you to reboot.
	
	Basically, just return to CP/M the way it was before your program
	ran.  See DEMO.C for a complete example of setting various VDC
	modes and exiting back to CP/M correctly.
	
	CP/M relies on the CIAs for communication to the outside world just
	like in native 64/128 modes.  With this in mind it is best not to
	change certain registers.  I have found it safe to use CIA #2's TOD
	clock, timers A and B and disable all interrupt sources.  You can
	also safely read the keyboard and joy sticks via CIA #1 if you
	disable interrupts.  My low level key scan function reads all key
	positions into an array of 11 bytes.  See page 642 of the Commodore
	128 Programmer's Reference for the key positions in the matrix. 
	You can tell joy stick signals from a key short because the joy
	stick shorts a whole row in the matrix instead of one bit like a
	key press.  See page 32 of the 1351 Mouse User's manual for more
	info.
	
	CP/M uses voice 1 of the SID to produce a key click.  You can use
	the SID just as you would in native 128 mode with one exception. 
	Since CP/M writes to the SID during key presses it may affect a
	sound in progress.  You can disable interrupts to eliminate this
	problem.
	
	The four bit Z Blaster engine requires the sound data's high
	nibbles to come first.  You can easily swap the nibbles once the
	sample is in memory if needed.  The maximum sample rate is about 15
	KHz.
	
	The PCX engines for 640 X 200 and 640 X 480 require you to toggle
	off the disk status line.  The status line updates during disk I/O
	and changes the VDC update address which throws off the engines.
	
	
	HOW TO CONTACT ME
	
	Please report any support questions, problems, suggestions, etc. to
	me via GEnie S.GOLDSMITH2, Internet S.GOLDSMITH2@GENIE.GEIS.COM,
	voice phone (813) 925-1064 or US Mail to:
	
	Steve Goldsmith
	2805 Jamaica Street
	Sarasota, FL 34231
	USA
	


