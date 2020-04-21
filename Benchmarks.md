# Benchmark Strategy

In all cases a simple compile is done to verify the programs generate correct results. Then the programs are compiled for a minimal target `+test` that eliminates as much unnecessary code as possible; this includes elimination of stdio and as many device drivers as possible. Total program size is recorded (this includes the `CODE`, `DATA` and `BSS` sections but does not include the stack) and the execution time is measured by ticks. `z88dk-ticks` is a command line z80 emulator that comes with z88dk and can measure execution time of program fragments exactly.

# Compilers

To put the current compilers and libraries for z88dk into context, the benchmarks are conducted against common alternatives.

## HITECH-C CPM v3.09

Hitech-C (CP/M-80) v 3.09 
Hitech's last CPM C compiler. One of the most capable native C compilers for CP/M. Runs under CP/M 2.2 and implements a large subset of C89. This compiler represents the best z80 native code generator. Hitech made this available for free many years ago.

## HITECH-C Z80 v7.50

The second last z80 compiler from Hitech (v7.80 was their last), cross compiles z80 code from MSDOS. Seems to be near complete compliance with C89.

## IAR Z80 V4.06A

IAR's last z80 compiler running under windows. Although it's not currently listed on their webpage, they are willing to sell it to anyone who has the cash.

## SDCC

sdcc 3.5.5 #9392 __UPDATE WITH 4.0.0 #11566__ (MINGW64) 
sdcc is a current open source C cross compiler targetting several small CPUs including the z80. Its primary feature is that it supports a large subset of modern C standards (C89, C99, C11).

## Z88DK/SCCZ80_CLASSIC

(nightly build 20 April 2020) 
z88dk's native C compiler sccz80 using the classic C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive (classic) C library written in assembly language.

## Z88DK/SDCC_CLASSIC

(nightly build 20 April 2020) 
sdcc 4.0.0 #11566 is used to translate C code with z88dk supplying its (classic) C library and startup code for targets.

## Z88DK/SCCZ80_NEW

(nightly build 20 April 2020) 
z88dk's native C compiler sccz80 using the new C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive (new) C library written in assembly language.

## Z88DK/SDCC_NEW

(nightly build 20 April 2020) 
sdcc 4.0.0 #11566 is used to translate C code with z88dk supplying its (new) C library and startup code for targets.


# Dhrystone 2.1

Dhrystone was a common synthetic benchmark for measuring the integer performance of compilers in the 1980s until more modern benchmarks replaced it. It attempts to simulate typical programs by executing a set of statements statistically determined from common programs in the wild.

The benchmark package is available for download.

|                   | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| DHRYSTONES/S| DMIPS  |
|-------------------|-----------|---------------|-----------------|-------------|--------|
|Hitech-C CPM v3.09 | 7471	| 354,120,220	| 88.53 sec	| 225.91	| 0.1286 |
|Hitech-C Z80 v7.50 | 7002	| 288,200,126	| 72.05 sec	| 277.58	| 0.1580 |
|IAR Z80 V4.06A     | 7371	| 306,860,580	| 76.72 sec	| 260.70	| 0.1484 |
|SDCC	            | 7013	| 292,880,320	| 73.22 sec	| 273.15	| 0.1554 |							
|Z88DK/SDCC_CLASSIC | 7344	| 248,080,263	| 62.02 sec	| 322.48	| 0.1835 |
|Z88DK/SDCC_NEW     | 7163	| 257,100,263	| 62.28 sec	| 311.16	| 0.1771 |

Notes:

* Hitech-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Hitech-C Z80 v7.50 must be compiled with global optimizer set to two; higher causes the program to hang.
* Dhrystone 2.1 is deprecated because optimizing compilers can eliminate redundant statements that were intended to add to execution time. However many z80-era compilers ran this benchmark so it is also available in the z88dk repository.

# Pi

Pi.c computes pi to 800 decimal places. It is based on an implementation found at crypto.stanford.edu.

Pi.c measures 32-bit integer math performance. The computation can make good use of ldiv() but not all compilers supply this function so the program is run with and without ldiv() for comparison purposes.

Z88DK's new C library has a fast integer math option so the table below shows results for it as well as the normal build using the small integer math option.

The first set of numbers are without the use of __ldiv()__ and the second with using __ldiv()__.

|                      |Size  | Z80 Cycles    |Wall Clock @4Mhz| Size | Z80 Cycles    | Wall Clock @4MHz
|----------------------|------|---------------|----------------|------|---------------|---------------|
| Hitech-C CPM v3.09   | 6793 |	5,531,933,581 |	23 min 03 sec  |      |               |               |				
| Hitech-C Z80 v7.50   | 6337 |	5,520,768,427 |	23 min 00 sec  | 6473 |	5,884,343,627 |	24 min 31 sec |
| IAR Z80 V4.06A       | 6789 |	8,762,223,085 |	36 min 31 sec  | 7006 |	8,799,503,282 |	36 min 40 sec |
| SDCC		       | 6844 |	8,700,157,418 |	36 min 15 sec  |      |		      |	              |
| Z88DK/SCCZ80_CLASSIC | 6508 |	4,012,440,830 |	16 min 43 sec  |      |	              |	              |
| Z88DK/SCCZ80_NEW     | 6269 |	4,012,440,735 |	16 min 43 sec  | 6182 |	2,576,381,983 |	10 min 44 sec |
| Z88DK/SCCZ80_NEW_FAST| 8999 |	1,696,878,309 |	7 min 04 sec   | 9131 |	1,301,832,933 |	5 min 25 sec  |
| Z88DK/SDCC_CLASSIC   | 6600 |	4,169,137,078 |	17 min 22 sec  |      |	              |	              |
| Z88DK/SDCC_NEW       | 6246 |	4,067,517,071 |	16 min 57 sec  | 6388 |	2,609,489,119 |	10 min 52 sec |
| Z88DK/SDCC_NEW_FAST  | 8997 |	1,756,864,232 |	7 min 19 sec   | 9097 |	1,339,849,656 |	5 min 35 sec  |

Notes:

* The HITECH-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Although HITECH-C Z80 v7.50 supplies ldiv(), it still performs two divisions to get quotient and remainder.
* SDCC's performance is hurt by having its 32-bit math routines implemented in C.
* Z88DK's small integer math library demotes long multiplies to integer where possible.
* Z88DK's fast integer math library is able to reduce most 32-bit divides to 16-bit divides. The loop unrolling option is not enabled.

# Sieve of Eratosthenes (Prime Numbers)

Sieve.c finds all the prime numbers in [2,7999]. The algorithm is known as the [Sieve of Eratosthenes](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes).

This is a popular benchmark for small machine compilers because just about every compiler is able to compile it. As a benchmarking tool it's mainly measuring loop overhead.

|                    | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|--------------------|----------|---------------|-----------------|
| Hitech-C CPM v3.09 |	8725	| 4,547,538	| 1.1369 sec      |
| Hitech-C Z80 v7.50 |	8243	| 3,672,107	| 0.9180 sec      |
| IAR Z80 V4.06A     |	8772	| 3,714,152	| 0.9285 sec      |
| SDCC	             |  8263	| 4,701,570	| 1.1754 sec      |
| Z88DK/SCCZ80_CLASSIC|	8589	| 4,957,733	| 1.2394 sec      |
| Z88DK/SCCZ80_NEW  |	8362	| 4,957,733	| 1.2394 sec      |
| Z88DK/SDCC_CLASSIC|   8558	| 4,510,806	| 1.1277 sec      |
| Z88DK/SDCC_NEW    |   8315	| 3,665,494	| 0.9163 sec      |

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


