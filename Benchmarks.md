_New benchmarks are being prepared_

# Compilers

In all cases a simple compile is done to verify the programs generate correct results. Then the programs are compiled for a minimal target that eliminates as much unnecessary code as possible; this includes elimination of stdio and as many device drivers as possible. Total program size is recorded (this includes the CODE, DATA and BSS sections but does not include the stack) and the execution time is measured by ticks. Ticks is a command line z80 emulator that comes with z88dk and can measure execution time of program fragments exactly.

## HITECH-C CPM v3.09

Hitech-C (CP/M-80) v 3.09 
One of the most capable native C compilers for CP/M. Runs under CP/M 2.2 and implements a large subset of C89.

## HITECH-C Z80 v7.50

The last z80 compiler from Hitech, cross compiles z80 code from MSDOS. Seems to be near complete compliance with C89.

## SDCC

sdcc 3.5.5 #9392 (MINGW64) 
sdcc is a current open source C cross compiler targetting several small CPUs including the z80. Its primary feature is that it supports a large subset of modern C standards (C89, C99, C11).

## Z88DK/SCCZ80_CLASSIC

(nightly build 10/NOV/2015) 
z88dk's native C compiler sccz80 using the classic C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive C library written in assembly language.

## Z88DK/SDCC_CLASSIC

(nightly build 10/NOV/2015) 
sdcc 3.5.5 #9392 is used to translate C code with z88dk supplying its (classic) C library and startup code for targets.


## Z88DK/SCCZ80_NEW

(nightly build 10/NOV/2015) 
z88dk's native C compiler sccz80 using the new C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive C library written in assembly language.

## Z88DK/SDCC_NEW

(nightly build 10/NOV/2015) 
sdcc 3.5.5 #9392 is used to translate C code with z88dk supplying its (new) C library and startup code for targets.


# Dhrystone 2.1

Dhrystone was a common synthetic benchmark for measuring the integer performance of compilers in the 1980s until more modern benchmarks replaced it. It attempts to simulate typical programs by executing a set of statements statistically determined from common programs in the wild.

The benchmark package is available for download.

|                   | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| DHRYSTONES/S| DMIPS  |
|-------------------|-----------|---------------|-----------------|-------------|--------|
|Hitech-C CPM v3.09 | 7809	| 376,240,194	| 94 sec	| 212.63	| 0.1210 |
|Hitech-C Z80 v7.50 | 7040	| 301,760,038	| 75 sec	| 265.11	| 0.1509 |
|SDCC	           | 7223	| 319,842,936	| 80 sec	| 250.12	| 0.1424 |
|Z88DK/SCCZ80_CLASSIC |          |               |               |               |        |							
|Z88DK/SCCZ80_NEW     |          |               |               |               |        |							
|Z88DK/SDCC	    | 7136	| 257,822,927	| 64 sec	| 310.29	| 0.1766 |

Notes:

* Hitech-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Hitech-C Z80 v7.50 must be compiled with global optimizer set to two; higher causes the program to hang.
* Dhrystone 1.1 is deprecated because optimizing compilers can eliminate redundant statements that were intended to add to execution time. However many z80-era compilers ran this benchmark so it is also available in the z88dk repository. Beginning at line 106 Dhry1.1 results can be found, at line 142 Dhry1.0 results and at line 394 a few results for the 6502.

# Pi

Pi.c computes pi to 800 decimal places. It is based on an implementation found at crypto.stanford.edu.

Pi.c measures 32-bit integer math performance. The computation can make good use of ldiv() but not all compilers supply this function so the program is run with and without ldiv() for comparison purposes.

Z88DK's new C library has a fast integer math option so the table below shows results for it as well as the normal build using the small integer math option.

        
|                      |Size  | Z80 Cycles    |Wall Clock @4Mhz| Size | Z80 Cycles    | Wall Clock @4MHz
|----------------------|------|---------------|----------------|------|---------------|---------------|
| Hitech-C CPM v3.09   | 6740 |	5,465,797,961 |	22 min 46 sec  |      |               |               |				
| Hitech-C Z80 v7.50   | 6332 |	5,520,762,227 |	23 min 00 sec  | 6473 |	5,884,343,627 |	24 min 31 sec |
| SDCC		       | 6805 |	8,892,037,196 |	37 min 03 sec  |      |		      |	              |
| Z88DK/SCCZ80_CLASSIC | 6076 |	5,391,413,260 |	22 min 28 sec  |      |		      |	              |
| Z88DK/SCCZ80_NEW     | 6149 |	5,246,696,144 |	21 min 52 sec  | 6182 |	3,773,744,792 |	15 min 43 sec |
| Z88DK/SDCC	       | 6154 |	5,285,278,076 |	22 min 01 sec  | 6165 |	3,786,981,324 |	15 min 47 sec |
| Z88DK/SCCZ80_NEW_FAST| 7166 |	1,953,856,481 |	8 min 08 sec   | 7199 |	1,455,531,292 |	6 min 04 sec  |
| Z88DK/SDCC_FAST      | 7171 |	1,990,813,171 |	8 min 18 sec   | 7182 |	1,467,142,582 |	6 min 07 sec  |

The first set of numbers are without the use of ldiv() and the second with using ldiv()

Notes:

* The HITECH-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Although HITECH-C Z80 v7.50 supplies ldiv(), it still performs two divisions to get quotient and remainder.
* SDCC's performance is hurt by having its 32-bit math routines implemented in C.
* Z88DK's fast integer math library is able to reduce most 32-bit divides to 16-bit divides. The loop unrolling option is not enabled.

# Sieve of Eratosthenes (Prime Numbers)

Sieve.c finds all the prime numbers in [2,7999]. The algorithm is known as the Sieve of Eratosthenes.

This is a popular benchmark for small machine compilers because just about every compiler is able to compile it. As a benchmarking tool it's mainly measuring loop overhead.

SIZE	TIME
Relative	Bytes	Relative	Z80 Cycles	Wall Clock @ 4MHZ

|                   | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|-------------------|-----------|---------------|-----------------|
| Hitech-C CPM v3.09|	8629	| 4,547,538	| 1.137 sec       |
| Hitech-C Z80 v7.50|	8203	| 3,672,107	| 0.918 sec       |
| SDCC	            |   8200	| 4,150,710	| 1.038 sec       |
| Z88DK/SCCZ80_CLASSIC|	8209	| 5,325,739	| 1.331 sec       |
| Z88DK/SCCZ80_NEW  |	8236	| 5,325,739	| 1.331 sec       |
| Z88DK/SDCC	    |   8175	| 3,691,568	| 0.923 sec       |

Notes:

* The HITECH-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Z88DK/SCCZ80 tries to generate small code by turning primitive compiler operations into subroutine calls. The additional call/ret overhead of these subroutine calls is significant in the small loop code and this is what hurts its performance in comparison to other compilers.

# Whetstone 1.2

Whetstone is a synthetic floating point benchmark. The benchmark package is available for download.

Floating point performance depends strongly on the number of mantissa bits in the float type.


|                     |Floatsize|Mantissa| Bytes| Z80 Cycles  | Wall Clock @4MHz | KWIPS | MWIPS    |
|---------------------|---------|--------|------|-------------|------------------|-------|----------|
| Hitech-C CPM v3.09  |	32	| 24	 | 7369	| 637,332,104 |	2 min 39 sec	 | 6.276 | 0.006276 |
| Hitech-C Z80 v7.50  |	32	| 24	 | 6872	| fail	      |                  |       |          |			
| SDCC	              | 32	| 24	 |14463	|2,821,354,806|	11 min 45 sec	 | 1.418 |0.001418  |
| Z88DK/SCCZ80_CLASSIC| 48	| 40	 | 5364	|1,295,331,166|	5 min 24 sec	 |3.088  |0.003088  |
| Z88DK/SCCZ80_NEW    |	48	| 40	 | 5300	|974,224,224  |	4 min 04 sec	 |4.106	 |0.004106  |
| Z88DK/SDCC	      | 32(48)	| 24(40) | 5914	|919,431,274  | 3 min 50 sec	 |4.351	 |0.004351 |

Notes:

* Hitech-C CPM v3.09 produces two results with excessive error.
* Hitech-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Hitech-C Z80 v7.50 produces incorrect results on all optimization levels.
* SDCC's peformance is hurt by a floating point package implemented in C.
* Z88DK/SCCZ80_CLASSIC uses the genmath float library while the other Z88DK compiles use math48.
* Z88DK/SDCC uses a 48-bit float internally but this is converted to 32-bit at the compiler-library interface since sdcc only understands a 32-bit float type.


