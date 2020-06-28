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

(Nightly build 20 April 2020) z88dk's native C compiler sccz80 using the classic C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive (classic) C library written in assembly language.

## Z88DK/SDCC_CLASSIC

(Nightly build 20 April 2020) sdcc 4.0.0 #11566 is used to translate C code with z88dk supplying its (classic) C library and startup code for targets.

## Z88DK/SCCZ80_NEW

(Nightly build 20 April 2020) z88dk's native C compiler sccz80 using the new C library in z88dk. sccz80 is a derivative of small C with most small C limitations eliminated. Its primary feature is a comprehensive (new) C library written in assembly language.

## Z88DK/SDCC_NEW

(Nightly build 20 April 2020) sdcc 4.0.0 #11566 is used to translate C code with z88dk supplying its (new) C library and startup code for targets.

# Binary-Trees

The purpose of this benchmark is to verify that malloc/free function trouble free and to measure the speed of malloc/free with allocations done in the context of constructing binary trees.

The work is to create binary trees - composed only of tree nodes all the way down-to depth 0, before any of those nodes are GC'd - using at-minimum the number of allocations of Jeremy Zerfas's C program. Don't optimize away the work.

|                    | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|--------------------|----------|---------------|-----------------|
| Hitech-C CPM v3.09 |	4165	| DISQ  	|                 |
| Hitech-C Z80 v7.50 |	4121	| 243,708,728	| 60.93 sec       |
| IAR Z80 V4.06A     |	4525	| 7,358,336,547	| 30 min 40 sec   |
| SDCC	             |  8626	| 203,788,182	| 50.95 sec       |
| Z88DK/SCCZ80_CLASSIC|	2924    | 153,408,086	| 38.52 sec       |
| Z88DK/SCCZ80_NEW  |	2711	| 6,582,763,903	| 27 min 25 sec   |
| Z88DK/SDCC_CLASSIC|   2978	| 150,508,687	| __37.63 sec__   |
| Z88DK/SDCC_NEW    | __2689__	| 6,576,349,618	| 27 min 24 sec   |

Notes:

* NEW library [Issue #113](https://github.com/z88dk/z88dk/issues/113) Library optimization for fast realloc causes slow free block search when a thousand blocks are allocated in this benchmark.
* IAR is likely implementing a heap similar to z88dk's new c library where an emphasis is placed on the speed of realloc().

# Dhrystone 2.1

Dhrystone was a common synthetic benchmark for measuring the integer performance of compilers in the 1980s until more modern benchmarks replaced it. It attempts to simulate typical programs by executing a set of statements statistically determined from common programs in the wild.

The benchmark package is available for download.

|                   | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| DHRYSTONES/S| DMIPS  |
|-------------------|-----------|---------------|-----------------|-------------|--------|
|Hitech-C CPM v3.09 | 7471	| 354,120,220	| 88.53 sec	| 225.91	| 0.1286 |
|Hitech-C Z80 v7.50 | __7002__	| 288,200,126	| 72.05 sec	| 277.58	| 0.1580 |
|IAR Z80 V4.06A     | 7371	| 306,860,580	| 76.72 sec	| 260.70	| 0.1484 |
|SDCC	            | 7013	| 292,880,320	| 73.22 sec	| 273.15	| 0.1554 |							
|Z88DK/SDCC_CLASSIC | 7344	| 248,080,263	| 62.02 sec	| __322.48__	| 0.1835 |
|Z88DK/SDCC_NEW     | 7163	| 257,100,263	| 62.28 sec	| 311.16	| 0.1771 |

Notes:

* Hitech-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Hitech-C Z80 v7.50 must be compiled with global optimizer set to two; higher causes the program to hang.
* Dhrystone 2.1 is deprecated because optimizing compilers can eliminate redundant statements that were intended to add to execution time. However many z80-era compilers ran this benchmark so it is also available in the z88dk repository.
# Fannkuch

The fannkuch benchmark is defined by programs in Performing Lisp Analysis of the FANNKUCH Benchmark, Kenneth R. Anderson and Duane Rettig. FANNKUCH is an abbreviation for the German word Pfannkuchen, or pancakes, in analogy to flipping pancakes. The conjecture is that the maximum count is approximated by n*log(n) when n goes to infinity.

|                    | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|--------------------|----------|---------------|-----------------|
| Hitech-C CPM v3.09 |	1218	| 56,667,034  	| 14.17 sec       |
| Hitech-C Z80 v7.50 |	__716__	| 49,858,382	| __12.46 sec__   |
| IAR Z80 V4.06A     |	1347	| 56,708,022	| 14.18 sec       |
| SDCC	             |  1196	| 67,174,167	| 16.79 sec       |
| Z88DK/SCCZ80_CLASSIC|	1178	| 77,386,481	| 19.34 sec       |
| Z88DK/SCCZ80_NEW  |	957	| 77,386,481	| 19.35 sec       |
| Z88DK/SDCC_CLASSIC|   1304	| 59,756,269	| 14.94 sec       |
| Z88DK/SDCC_NEW    |   1070	| 56,090,095	| 14.02 sec       |

# Fasta

The program should:
 - generate DNA sequences, by copying from a given sequence.
 - generate DNA sequences, by weighted random selection from 2 alphabets.
 - convert the expected probability of selecting each nucleotide into cumulative probabilities.
 - match a random number against those cumulative probabilities to select each nucleotide (use linear search or binary search).
 - use this naïve linear congruential generator to calculate a random number each time a nucleotide needs to be selected (don't cache the random number sequence).

|                    | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|--------------------|----------|---------------|-----------------|
| Hitech-C CPM v3.09 |	4056	| 188,751,954  	| 47.19 sec       |
| Hitech-C Z80 v7.50 |	4121	| DISQ   	|                 |
| IAR Z80 V4.06A     |	6041	| 223,805,149	| 55.95 sec       |
| SDCC	             |  6947	| 488,970,702	| 122.24 sec      |
| Z88DK/SCCZ80_CLASSIC|	3291	| 243,021,012	| 60.76 sec       |
| Z88DK/SCCZ80_CLASSIC/MATH32|	3978	| 136,057,474	| 34.01 sec       |
| Z88DK/SCCZ80_NEW  |	__2998__	| 204,281,085	| 51.07 sec       |
| Z88DK/SCCZ80_NEW/MATH32|	3729	| 136,057,141	| __34.01 sec__   |
| Z88DK/SDCC_CLASSIC|   3583	| 248,331,410	| 62.08 sec       |
| Z88DK/SDCC_NEW    |   3171	| 245,055,005	| 61.26 sec       |

Notes:

* Hitech-C Z80 v7.50 produces incorrect results on all optimization levels.
* SDCC's performance is hurt by a floating point package implemented in C.
* Z88DK/SCCZ80_CLASSIC uses the `genmath` float library while the other Z88DK compiles use `math48`.
* Z88DK/SDCC uses a 48-bit float internally but this is converted to 32-bit at the compiler-library interface since sdcc only understands a 32-bit float type.
* Z88DK/SCCZ80/MATH32 uses the `math32` 32-bit IEEE-754 floating point package.

# n-Body

Model the orbits of Jovian planets, using the same simple symplectic-integrator. Thanks to Mark C. Lewis for suggesting this task.

Useful symplectic integrators are freely available, for example the HNBody Symplectic Integration Package.

|                    | SIZE	| Z80 Cycles    | Wall Clock @4Mhz| 
|--------------------|----------|---------------|-----------------|
| Hitech-C CPM v3.09 |	4056	| DISQ  	|                 |
| Hitech-C Z80 v7.50 |	4121	| DISQ   	|                 |
| IAR Z80 V4.06A     |	4084	| 2,331,516,019	| 9 min 43 sec    |
| SDCC	             |  9233	| 5,306,393,684	| 22 min 07 sec   |
| Z88DK/SCCZ80_CLASSIC|	3814	| 3,624,577,433	| 15 min 06 sec   |
| Z88DK/SCCZ80_NEW  |	__3608__	| 2,372,283,755	| 9 min 53 sec    |
| Z88DK/SCCZ80_NEW/MATH32|	5264	| 1,025,105,884	| __4 min 16 sec__  |
| Z88DK/SCCZ80_NEW/MATH16|	3227	| 0,384,230,543	| __1 min 36 sec__  |
| Z88DK/SDCC_CLASSIC|   4770	| 2,253,531,346	| 9 min 23 sec    |
| Z88DK/SDCC_NEW    |   4356	| 2,247,600,377	| 9 min 22 sec    |

Notes:

* Hitech-C CPM v3.09 can't compile the source code.
* Hitech-C Z80 v7.50 produces incorrect results on all optimization levels.
* SDCC's performance is hurt by a floating point package implemented in C.
* Z88DK/SCCZ80_CLASSIC uses the `genmath` float library while the other Z88DK compiles use `math48`.
* Z88DK/SDCC uses a 48-bit float internally but this is converted to 32-bit at the compiler-library interface since sdcc only understands a 32-bit float type.
* Z88DK/SCCZ80_NEW/MATH32 uses the `math32` 32-bit IEEE-754 floating point package.
* Z88DK/SCCZ80_NEW/MATH16 uses the `math16` 16-bit IEEE-754 floating point package.

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
| Z88DK/SCCZ80_NEW_FAST| 8999 |	1,696,878,309 |	__7 min 04 sec__   | 9131 |	1,301,832,933 |	__5 min 25 sec__  |
| Z88DK/SDCC_CLASSIC   | 6600 |	4,169,137,078 |	17 min 22 sec  |      |	              |	              |
| Z88DK/SDCC_NEW       | __6246__ |	4,067,517,071 |	16 min 57 sec  | 6388 |	2,609,489,119 |	10 min 52 sec |
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
| Hitech-C Z80 v7.50 | __8243__	| 3,672,107	| 0.9180 sec      |
| IAR Z80 V4.06A     |	8772	| 3,714,152	| 0.9285 sec      |
| SDCC	             |  8263	| 4,701,570	| 1.1754 sec      |
| Z88DK/SCCZ80_CLASSIC|	8589	| 4,957,733	| 1.2394 sec      |
| Z88DK/SCCZ80_NEW  |	8362	| 4,957,733	| 1.2394 sec      |
| Z88DK/SDCC_CLASSIC|   8558	| 4,510,806	| 1.1277 sec      |
| Z88DK/SDCC_NEW    |   8315	| 3,665,494	| __0.9163 sec__      |

Notes:

* The HITECH-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Z88DK/SCCZ80 tries to generate small code by turning primitive compiler operations into subroutine calls. The additional call/ret overhead of these subroutine calls is significant in the small loop code and this is what hurts its performance in comparison to other compilers.

# Whetstone 1.2

Whetstone is a synthetic floating point benchmark. The benchmark package is available for download.

Floating point performance depends strongly on the number of mantissa bits in the float type.


|                     |Float Size|Mantissa| Bytes| Z80 Cycles | Wall Clock @4MHz | KWIPS |
|---------------------|----------|--------|------|------------|------------------|-------|
| Hitech-C CPM v3.09  |	32	 | 24	  | 7605  | 639,413,871   | 159.8535 sec | 6.2557 |
| Hitech-C Z80 v7.50  |	32	 | 24	  | 6872  | fail          |              |        |
| IAR Z80 V4.06A      |	32	 | 24	  | 6524  | 732,360,277   | 183.0901 sec | 5.4618 |
| SDCC	              | 32	 | 24	  | 14379 | 2,184,812,093 | 546.2030 sec | 1.8308 |
| Z88DK/SCCZ80_CLASSIC| 48	 | 40	  | 5744  | 1,280,818,856 | 320.2047 sec | 3.1230 |
| Z88DK/SCCZ80_NEW    |	48	 | 40	  | __5388__  | 973,210,939   | 243.3027 sec | 4.1101 |
| Z88DK/SDCC_CLASSIC  | 32(48)	 | 24(40) | 7045  | 921,228,352   | 230.3071 sec | 4.3420 |
| Z88DK/SDCC_NEW      | 32(48)	 | 24(40) | 6234  | 916,707,945   | 229.1770 sec | 4.3634 |
| Z88DK/SDCC/MATH32   | 32 	 | 24     | 9554  | 794,722,631   | 198.6806 sec | __5.0332__ |

Notes:

* Hitech-C CPM v3.09 produces two results with excessive error.
* Hitech-C CPM v3.09 binary size is over-estimated as it will contain some stdio structures for cp/m.
* Hitech-C Z80 v7.50 produces incorrect results on all optimization levels.
* SDCC's performance is hurt by a floating point package implemented in C.
* Z88DK/SCCZ80_CLASSIC uses the `genmath` float library while the other Z88DK compiles use `math48`.
* Z88DK/SDCC uses a 48-bit float internally but this is converted to 32-bit at the compiler-library interface since sdcc only understands a 32-bit float type.
* Z88DK/SDCC/MATH32 uses the `math32` 32-bit IEEE-754 floating point package.
