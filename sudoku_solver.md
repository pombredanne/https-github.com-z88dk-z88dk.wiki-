# SUDOKU.C

{{libnew:examples:sudoku1.png?312|sudoku_solver_1}}

These example programs tackle the solution of [Sudoku](https://en.wikipedia.org/wiki/Sudoku) puzzles.

The first approach is a brute force trial and error that traverses the puzzle in top-down, left-right order trying every legal number placement in turn and backtracking when a solution cannot be found for the current puzzle state.  The number of possibilities to investigate can grow very large and therefore this approach can lead to very long running times.

The second approach applies logic to the problem and this considerably reduces running time.

## Solver #1:  Brute Force Depth First Search with Backtracking

This solution was written by [syb0rg@stackexchange.com](http://codereview.stackexchange.com/questions/37430/sudoku-solver-in-c).

The one good thing about this solution is that it is very short but other than that it is not practical.  The number of possibilities to investigate can be very large and can lead to very long running times, perhaps into months for rare cases.

This program uses a two-dimensional array to hold the puzzle which means it can only be compiled using sdcc without modification.  The variable types are all unsigned char; sdcc can produce good quality code when dealing with unsigned chars and that is evident in this program.

The compile line at the top of the source is for the zx spectrum target but it can be changed for any target that supports stdin & stdout.

The line "fflush(stdin);" clears the input buffer so that any pending keypresses are dropped.  This is not guaranteed behaviour in all C implementations but this is defined behaviour in z88dk.

	:::c
	// http://codereview.stackexchange.com/questions/37430/sudoku-solver-in-c
	
	// zcc +zx -vn -startup=1 -clib=sdcc_ix -SO3 --max-allocs-per-node200000 --reserve-regs-iy sudoku.c -o sudoku
	// appmake +zx --binfile sudoku_CODE.bin --org 32768 --blockname sudoku
	
	#include `<stdio.h>`
	#include `<stdint.h>`
	
	int isAvailable(uint8_t puzzle[][9], uint8_t row, uint8_t col, uint8_t num)
	{
	    uint8_t rowStart;
	    uint8_t colStart;
	    uint8_t i;
	
	    rowStart = (row/3) * 3;
	    colStart = (col/3) * 3;
	
	    for(i=0; i<9; ++i)
	    {
	        if (puzzle[row][i] == num) return 0;
	        if (puzzle[i][col] == num) return 0;
	        if (puzzle[rowStart + (i%3)][colStart + (i/3)] == num) return 0;
	    }
	    return 1;
	}
	
	int solveSudoku(uint8_t puzzle[][9], uint8_t row, uint8_t col)
	{
	    uint8_t i;
	    
	    if(row<9 && col<9)
	    {
	        if(puzzle[row][col])
	        {
	            if((col+1)<9) return solveSudoku(puzzle, row, col+1);
	            else if((row+1)<9) return solveSudoku(puzzle, row+1, 0);
	            else return 1;
	        }
	        else
	        {
	            for(i=0; i<9; ++i)
	            {
	                if(isAvailable(puzzle, row, col, i+1))
	                {
	                    puzzle[row][col] = i+1;
	                    if(solveSudoku(puzzle, row, col)) return 1;
	                    puzzle[row][col] = 0;
	                }
	            }
	        }
	        return 0;
	    }
	    else return 1;
	}
	
	int main()
	{
	    static uint8_t puzzle[9][9];
	    static uint8_t i, j, c;
	
	    printf("\x0cSUDOKU SOLVER\nby syb0rg @ stackexchange\n");
	
	    while (1)
	    {
	       printf("\n\nEnter Puzzle:\n\n");
	       fflush(stdin);
	       
	       for (i=0; i<9; ++i)
	       {
	           for (j=0; j<9; ++j)
	           {
	               while ((c = getchar()) == '\n') ;
	               puzzle[i][j] = ((c>='1') && (c<='9')) ? c-'0' : 0;
	           }
	       }
	
	       if(solveSudoku(puzzle, 0, 0))
	       {
	           printf("\n+-----+-----+-----+\n");
	           for(i=0; i<9; ++i)
	           {
	               for(j=0; j<9; ++j) printf("|%u", puzzle[i][j]);
	               printf("|\n");
	               if ((i+1)%3 == 0) printf("+-----+-----+-----+\n");
	           }
	       }
	       else printf("\n\nNO SOLUTION\n\n");
	    }
	
	    return 0;
	}


## Solver #2: Logic Applied to the Solution

For this solver puzzle state is expanded to track two items:


*  Each cell has "pencil marks" associated with it that keeps track of which numbers 1..9 can be placed in the cell.

*  Cells are kept in one of ten linked lists according to how many numbers are currently valid for the cells.  So, for example, if a particular cell can only allow two numbers in it according to pencil marks, that cell would be held in list[2].  If any cell ends up in list[0], indicating it cannot take any number, the puzzle is unsolvable.

With this information available, the program follows a few simple strategies:


*  First, it traverses list[1] which holds cells which can only be one number.  These cells are played in turn and the pencil marks for all affected cells are updated, maybe moving them to new lists.

*  Second, the program examines list[2] which holds cells that can only be one of two numbers.  It looks for pairs which are any two cells that lie in the same row, column or block.  If a pair shares the same pencil marks, ie they can only contain the same two numbers, then all other cells in the same row, column or block cannot be those two numbers.  So all other cells' pencil marks are updated to eliminate those two numbers.

*  With the first two strategies exhausted, the program performs trial and error placement.  It tries to play legal numbers for cells with the least amount of possible numbers first by playing cells, in order, from list[2], then list[3], and so on up to list[9].  In this way it minimizes the number of trial and error investigations it has to perform.

These strategies are simple and after implementing them I did expect to have to add more strategies to get good solving times but that was not the case.  The program can solve most sudoku puzzles very quickly, including the [world's hardest sudoku](http://www.telegraph.co.uk/news/science/science-news/9359579/Worlds-hardest-sudoku-can-you-crack-it.html) in under 25 seconds on a 3.5MHz z80 so it was left as is.

Compilation turned up a bug in sccz80.  sccz80 cannot compile the const array initializer in the source so at the moment it cannot be used to generate a binary.  This problem is being looked into and hopefully not too long from now this paragraph can be deleted.

The source is shown below.  If compiled with "-DVERBOSE" on the compile line, the program will print the steps it takes to solve a puzzle.

The source uses some special features of the library including p_list_t (a doubly linked list type modelled on C++'s list`<void*>`), an obstack to save puzzle state during trial-and-error attempts, and a demonstration of creating an independent section to hold data.  More discussion follows the listing.

**File: "sudoku.c"**

	:::c
	// zcc +zx -vn -startup=1 -clib=new sudoku.c block.asm -o sudoku
	// note: sccz80 compilation currently does not work because of a bug with array initialization
	//
	// zcc +zx -vn -startup=1 -clib=sdcc_ix -SO3 --max-allocs-per-node200000 --reserve-regs-iy sudoku.c block.asm -o sudoku
	//
	// appmake +zx --binfile sudoku_CODE.bin --org 45000 --blockname sudoku
	
	#include `<stdio.h>`
	#include `<stdlib.h>`
	#include `<string.h>`
	#include `<adt/p_list.h>`
	#include `<obstack.h>`
	#include `<stdint.h>`
	
	// define VERBOSE to print list of steps taken
	
	#pragma output CRT_ORG_CODE = 45000
	#pragma output REGISTER_SP = 0
	#pragma output CLIB_EXIT_STACK_SIZE = 0
	#pragma output CLIB_MALLOC_HEAP_SIZE = 0
	#pragma output CLIB_STDIO_HEAP_SIZE = 0
	#pragma output CLIB_FOPEN_MAX = 0
	#pragma output CLIB_OPEN_MAX = 0
	
	struct sudoku_cell
	{
	   void *next;                   // doubly linked list links
	   void *prev;
	   int16_t state;                // bits 15 = flag (do not change), 8:0 set indicates number possible
	};
	
	struct sudoku
	{
	   struct sudoku_cell grid[81];  // 9x9 board
	   p_list_t remain[10];          // remain[N] holds list of cells with N possible numbers
	};
	
	struct sudoku puzzle;
	
	uint16_t changes;
	
	const uint8_t count[512] = { 
	   0, 1, 1, 2, 1, 2, 2, 3, 1, 2, 2, 3, 2, 3, 3, 4, 1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, /*   0.. 31 */ 
	   1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, 2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, /*  32.. 63 */ 
	   1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, 2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, /*  64.. 95 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /*  96..127 */ 
	   1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, 2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, /* 128..159 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /* 160..191 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /* 192..224 */ 
	   3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, 4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8, /* 225..255 */ 
	   1, 2, 2, 3, 2, 3, 3, 4, 2, 3, 3, 4, 3, 4, 4, 5, 2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, /* 256..287 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /* 288..319 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /* 320..351 */ 
	   3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, 4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8, /* 352..383 */ 
	   2, 3, 3, 4, 3, 4, 4, 5, 3, 4, 4, 5, 4, 5, 5, 6, 3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, /* 384..415 */ 
	   3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, 4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8, /* 416..447 */ 
	   3, 4, 4, 5, 4, 5, 5, 6, 4, 5, 5, 6, 5, 6, 6, 7, 4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8, /* 448..479 */ 
	   4, 5, 5, 6, 5, 6, 6, 7, 5, 6, 6, 7, 6, 7, 7, 8, 5, 6, 6, 7, 6, 7, 7, 8, 6, 7, 7, 8, 7, 8, 8, 9, /* 480..511 */ 
	};
	
	extern char stack_space[30*sizeof(struct sudoku)+6];
	
	struct obstack *stack;
	
	void mark(struct sudoku_cell *sc, uint16_t pstate)
	{   
	   if ((sc->state `< 0) || ((sc->`state & pstate) == 0))
	      return;
	
	   p_list_remove(&puzzle.remain[count[sc->state]], sc);
	   sc->state &= ~pstate;
	   p_list_push_front(&puzzle.remain[count[sc->state]], sc);
	
	   ++changes;
	   
	   return;
	}
	
	uint16_t place(struct sudoku_cell *sc, uint16_t pstate, uint8_t peers)
	{
	   uint8_t i;
	   divu_t coord;
	   struct sudoku_cell *end, *v;
	   
	   // COMPUTE ROW, COL
	   
	   _divu_(&coord, (uint16_t)(sc - puzzle.grid), 9);
	
	   // coord.quot = row
	   // coord.rem  = column
	   
	   // VISIT ROW
	   
	   if (peers & 0x4)
	   {
	      v = &puzzle.grid[coord.quot * 9];
	      end = v + 9;
	   
	      while (v < end)
	      {
	         mark(v,pstate);
	         ++v;
	      }
	   }
	   
	   // VISIT COLUMN
	
	   if (peers & 0x2)
	   {
	      v = &puzzle.grid[coord.rem];
	      end = v + 81;
	
	      while (v < end)
	      {
	         mark(v,pstate);
	         v += 9;
	      }
	   }
	   
	   // VISIT BLOCK
	
	   if (peers & 0x1)
	   {
	      v = &puzzle.grid[(coord.quot/3)*27 + (coord.rem/3)*3];
	      end = v + 21;
	
	      i = 0;
	      while (v < end)
	      {
	         mark(v,pstate);
	      
	         if (++i == 3)
	         {
	            i = 0;
	            v += 7;
	         }
	         else ++v;
	      }
	   }
	
	   return p_list_empty(&puzzle.remain[0]);
	}
	
	uint8_t buddy(struct sudoku_cell *sc1, struct sudoku_cell *sc2)
	{
	   divu_t coord1, coord2;
	   uint8_t peers;
	   
	   _divu_(&coord1, (uint16_t)(sc1 - puzzle.grid), 9);
	   _divu_(&coord2, (uint16_t)(sc2 - puzzle.grid), 9);
	
	   // coord.quot = row
	   // coord.rem  = column
	
	   peers  = (coord1.quot/3 == coord2.quot/3) && (coord1.rem/3 == coord2.rem/3);
	   peers += (coord1.rem == coord2.rem) * 2;
	   peers += (coord1.quot == coord2.quot) * 4;
	   
	   return peers;
	}
	
	uint16_t solve_sudoku(void)
	{
	#ifdef VERBOSE
	   divu_t coord;
	#endif
	   uint16_t j;
	   uint8_t i;
	   struct sudoku_cell *sc2, *sc1;
	
	process_solo:
	
	   // PLAY CELLS WITH ONE REMAINING CHOICE
	
	   while (sc1 = p_list_pop_front(&puzzle.remain[1]))
	   {
	#ifdef VERBOSE
	      _divu_(&coord, (uint16_t)(sc1 - puzzle.grid), 9);
	      printf("Placing %u at (%u,%u)\n", ffs(sc1->state), coord.quot+1, coord.rem+1);
	#endif
	
	      sc1->state |= 0x8000;
	      if (!place(sc1, sc1->state & 0x01ff, 0x7)) return 0;
	   }
	
	   // INVESTIGATE TUPLES - PAIRS
	
	#ifdef VERBOSE
	   printf("Investigating Pairs...\n");
	#endif
	
	   do
	   {
	      changes = 0;
	      
	      for (sc2 = p_list_front(&puzzle.remain[2]); sc2; sc2 = p_list_next(sc2))
	      {
	         for (sc1 = p_list_next(sc2); sc1; sc1 = p_list_next(sc1))
	         {
	            if ((sc1->state == sc2->state) && (i = buddy(sc1,sc2)))
	            {
	#ifdef VERBOSE
	               _divu_(&coord, (uint16_t)(sc1 - puzzle.grid), 9);
	               printf("Pairs at (%u,%u) and ", coord.quot+1, coord.rem+1);
	               
	               _divu_(&coord, (uint16_t)(sc2 - puzzle.grid), 9);
	               printf("(%u,%u)\n", coord.quot+1, coord.rem+1);
	#endif
	
	               sc1->state |= 0x8000;
	               sc2->state |= 0x8000;
	            
	               if (!place(sc1, sc1->state & 0x01ff, i)) return 0;
	            
	               sc1->state &= 0x01ff;
	               sc2->state &= 0x01ff;
	           
	               if (!p_list_empty(&puzzle.remain[1])) goto process_solo;
	            }
	         }
	      }
	      
	   } while (changes);
	
	   // TRIAL AND ERROR
	   
	   for (i=2; i<10; ++i)
	      if (sc1 = p_list_front(&puzzle.remain[i])) break;
	
	   if (sc1 == 0) return 1;
	   
	   j=1;
	   do
	   {
	      if (sc1->state & j)
	      {
	         if ((sc2 = obstack_copy(stack, &puzzle, sizeof(struct sudoku))) == 0)
	         {
	#ifdef VERBOSE
	            printf("Out of Memory!\n");
	#endif
	            return 0;
	         }
	
	#ifdef VERBOSE
	         printf("Trial and Error... %09B\n  -> ", sc1->state);
	#endif
	
	         p_list_remove(&puzzle.remain[count[sc1->state]], sc1);
	         sc1->state = j;
	         p_list_push_front(&puzzle.remain[1], sc1);
	
	         if (solve_sudoku()) return 1;
	         
	         memcpy(&puzzle, sc2, sizeof(struct sudoku));
	         obstack_free(stack, sc2);
	
	#ifdef VERBOSE
	         printf("Backtrack... %09B\n", sc1->state);
	#endif
	      }
	
	   } while ((j*=2) & 0x1ff);
	   
	   return 0;
	}
	
	main()
	{
	   uint8_t i, j;
	   int16_t c;
	   struct sudoku_cell *sc;
	
	   #asm
	   di
	   #endasm
	
	   stack = (struct obstack *)(stack_space);
	   obstack_init(stack, sizeof(stack_space));
	
	   printf("\x0cSUDOKU SOLVER\nby aralbrec @ z88dk.org\n");
	
	   while (1)
	   {
	      memset(&puzzle, 0, sizeof(struct sudoku));
	      obstack_free(stack, 0);
	
	      printf("\n\nEnter Puzzle:\n\n");
	      fflush(stdin);
	      
	      sc = &puzzle.grid[0];
	      for (i=0; i<9; ++i)
	      {
	         for (j=0; j<9; ++j)
	         {
	            while ((c = getchar()) == '\n') ;
	            if (c == EOF) return 0;
	      
	            if ((c>='1') && (c<='9'))
	            {
	               sc->state = 1 << (c-'1');
	               p_list_push_front(&puzzle.remain[1], sc);
	            }
	            else
	            {
	               sc->state = 0x1ff;
	               p_list_push_front(&puzzle.remain[9], sc);
	            }
	            
	            ++sc;
	         }
	      }
	
	      if (solve_sudoku())
	      {
	         printf("\n+-----+-----+-----+\n");
	         
	         sc = &puzzle.grid[0];
	         for(i=0; i<9; ++i)
	         {
	            for(j=0; j<9; ++j)
	            {
	               printf("|%u", ffs(sc->state));
	               ++sc;
	            }
	               
	            printf("|\n");
	            
	            if ((i+1)%3 == 0) printf("+-----+-----+-----+\n");
	         }
	      }
	      else printf("\n\nNO SOLUTION\n\n");
	   }
	}


**File: "block.asm"**

	
	SECTION OBSTACK
	org 23296
	
	PUBLIC _stack_space
	
	_stack_space:  defs 15786      ; 30*sizeof(struct sudoku)+6


### p_list_t: Doubly Linked List Container

The new C library supplies several container types modeled on C++'s STL containers.  [p_list_t](p_list_t) is a [doubly linked list](https://en.wikipedia.org/wiki/Doubly_linked_list) whose links are intrusive, ie space for them is reserved in the list items themselves.  The API for using p_list_t is similar to [C++'s list`<void*>`](http://www.cplusplus.com/reference/list/list/) but with several iterator- and list-specific- operations not implemented.  Consult the [p_list_t](p_list_t) documentation or the [p_list.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/_DEVELOPMENT/sdcc/adt/p_list.h?view=markup) header file for a list of available functions.

In the sudoku solver, individual sudoku cells can be added to lists so their definition:

	:::c
	struct sudoku_cell
	{
	   void *next;                   // doubly linked list links
	   void *prev;
	   int16_t state;                // bits 15 = flag (do not change), 8:0 set indicates number possible
	};


contains space for a "next" and "prev" pointer required by p_list_t to maintain lists.  When adding or retrieving items from a list, it is the address of the "next" pointer that is communicated so having these links at the top of the struct means the address of the cell struct itself is communicated with the p_list_t functions.

In the source code you can see various standard list operations being performed by p_list_t.  Function names are scoped with a leading "p_list_" in their names.  For example, "p_list_pop_front()" will remove the first item from the list.  This sort of naming convention is applied generally within the library.

"p_list_t"s are initialized to empty by zeroing them.  This fact is taken advantage of in main() where the sudoku puzzle is initialized by zeroing the entire "struct sudoku" structure.

### Trial and Error: Backtracking

When the program enters the trial and error stage, it speculatively places numbers in an attempt to solve the puzzle.  Of course, placing these speculative numbers may lead to a contradiction so the program will have to undo what it has done (backtrack) and try placing a different number in the cell causing the contradiction.

This backtracking is accomplished by saving the puzzle state before a speculative placement is made and then restoring the puzzle state if a contradiction is discovered and before trying a different number for the offending placement.

To make this easy, the entire puzzle state is held in a single global variable -- "struct sudoku puzzle;".  A "struct sudoku" is defined like this:

	:::c
	struct sudoku
	{
	   struct sudoku_cell grid[81];  // 9x9 board
	   p_list_t remain[10];          // remain[N] holds list of cells with N possible numbers
	};


Every sudoku cell is represented by a "struct sudoku_cell" which holds information about list membership and the cell's pencil marks (the set of numbers that can legally be played).  The ten lists holding the cells are also included, with "remain[n]" enumerating all cells with "n" remaining legal number choices.

So saving state is a simple matter of copying the data of this structure someplace and then restoring state is a matter of copying into this data structure from a stored copy.

The question is now where to store state as trial and error steps are taken.  One way would be to get memory with malloc().  malloc() is not a bad choice for this program as it only ever allocates "struct sudoku"s so that all memory requests are the same size.  Because malloc() can accommodate any size memory requests usually its memory pool becomes [fragmented](https://en.wikipedia.org/wiki/Fragmentation_%28computing%29#External_fragmentation) over time when variably-sized requests are made and this is especially a problem when the available memory is small, as in the z80's case which only has at most 64k of memory available (and typically much less for the heap!).  In embedded applications, a top concern is to constantly reset malloc's heap so that fragmentation is kept under control and a long-running embedded application will not fail due to memory allocation issues.

Because of this fragmentation issue, embedded applications sometimes make use of other memory allocation algorithms.  Two such approaches are the [obstack](http://www.gnu.org/software/libc/manual/html_node/Obstacks.html#Obstacks) and a [block memory allocator](https://en.wikipedia.org/wiki/SLOB).  Both these memory allocation methods are implemented in the C library and this program makes use of an obstack.

The idea behind the obstack is very simple.  An obstack is created by reserving an area of memory for it.  A fence pointer is maintained that points at the next available memory address in the obstack.  When an allocation is made, the fence pointer address is returned as the address of the allocation and the fence is advanced by the size of the allocation.  In short, objects are allocated one after the other in address order.  Obstacks are not as general as malloc's heap when it coming to freeing memory.  When an object is freed, all objects following the freed object are also freed.  Freeing is simply moving the fence pointer to the address of the freed object so that all memory following becomes available again.  The obstack is very quick in comparison to malloc's heap and this freeing object restriction also happens to be the way most embedded applications reset their memory periodically so it's a very good fit for embedded applications.

In this program, memory is set aside for one obstack.  The array set aside is an external variable so that no actual memory is set aside by the C compiler; the C compiler is only being informed of its existence and size.  This is explained in the next topic.

	:::c
	extern char stack_space[30*sizeof(struct sudoku)+6];


You can think of this as a block of memory of the indicated size.

An obstack pointer is declared and initialized to point into that block:

	:::c
	struct obstack *stack;
	...
	main()
	{
	   ...
	   stack = (struct obstack *)(stack_space);
	   obstack_init(stack, sizeof(stack_space));
	   ...
	}


obstack_init() prepares the block to hold an initially empty obstack.  Allocations will be allowed up to the maximum size of the block and memory can be freed down to the minimum address inside the block.

The code to copy sudoku state into the obstack is simple:

	:::c
	         if ((sc2 = obstack_copy(stack, &puzzle, sizeof(struct sudoku))) == 0)
	         {
	#ifdef VERBOSE
	            printf("Out of Memory!\n");
	#endif
	            return 0;
	         }


obstack_copy() will attempt to allocate "sizoef(struct sudoku)" bytes and copy the puzzle into it.  Like malloc(), the obstack functions return 0 if there isn't sufficient memory available.  The return value is otherwise the address of the allocated memory.

If backtracking is necessary, the puzzle state is restored from the copy in the obstack and then that allocation is freed:

	:::c
	         memcpy(&puzzle, sc2, sizeof(struct sudoku));
	         obstack_free(stack, sc2);


Trial and Error + Backtracking always frees its allocations in the reverse order they are made so this fits fine with how obstack free works.

### Creating an Independent SECTION to Hold Data

The area of memory used by the obstack is declared extern as described in the last section.  It's not necessary to do that -- the area could be declared without using extern so that the compiler reserved space in the BSS section.  But at the time of writing, it was not known how much memory was going to be needed for the backtracking when solving sudoku puzzles so the code was written preparing for very tight memory conditions.  On the zx target, the idea was going to be to use a large chunk of memory normally reserved by the basic operating system.  This could not be part of the binary as using the basic o/s to load such a binary would immediately cause the program to crash before it started.  Instead, the allocation of the memory chunk was taken out of the compiler's hands and out of the generated binary.

The first step in doing that was to inform the compiler that a memory block is created externally:

	:::c
	extern char stack_space[30*sizeof(struct sudoku)+6];


and then using assembler, that block is reserved in memory at a specific address in a separate asm file:

**File: "block.asm"**

	
	SECTION OBSTACK
	org 23296
	
	PUBLIC _stack_space
	
	_stack_space:  defs 15786      ; 30*sizeof(struct sudoku)+6


On the zx target this address (23296) immediately follows the display file and the reserved chunk of memory (15786 bytes) clobbers the basic o/s's running program and system variables.  The basic o/s's interrupt service routine also writes data into this area so one of the first steps taken in main() is to disable interrupts.

The memory reservation occupies addresses 23296 through 39081.  The sudoku code is ORGd at address 45000 to get out of that space by using the "CRT_ORG_CODE" pragma in the source.

When the program is compiled using one of the suggested compiler invocations:

	
	zcc +zx -vn -startup=1 -clib=sdcc_ix -SO3 --max-allocs-per-node200000 --reserve-regs-iy sudoku.c block.asm -o sudoku


both "sudoku.c" and "block.asm" are listed.  Because the "_stack_space" block of memory is placed in an unknown (to the crt) section named "OBSTACK" with its own org address, the compiler will output two files:  one is the executable to be loaded at address 45000 and named "sudoku_CODE.bin" and the other will be "sudoku_OBSTACK.bin" representing the unknown section at address 23296.  We can throw away "sudoku_OBSTACK.bin" and simply load and execute "sudoku_CODE.bin" from address 45000.  It's fine to throw away this file because it does not need to be loaded into memory -- the program intializes that area of memory in the obstack_init() call.  Note that these sorts of external sections are unknown to the crt so the crt will not initialize them prior to calling main().

The space allocated for the obstack allows trial-and-error to save state 30 times consecutively.  In actuality, the program never seems to need more than around 15 consecutive state saves so the size reserved could be further reduced.

### Other Things to Try

#### Condense the Code into One Source File

It turned out to be unnecessary to create an obstack externally in a separate source file.  The source could be cleaned up and made more portable by simply reserving the space required in a plain character array rather than an external array.  The inlined asm used to disable interrupts could also be eliminated for the zx target as that external array would no longer be placed on top of the zx's basic o/s variables.  For the zx target, the program start address would have to be moved downward; the default 32768 would provide sufficient space for the program + obstack.

#### Compile with -DVERBOSE

With VERBOSE defined the sudoku solver will print out all steps taken to solve a puzzle.

#### Implement Further Solution Strategies

Real Sudoku players frown on trial-and-error as evidence of lack of strategy.  Reliance on trial-and-error could be reduced by implementing more solution strategies.  These additional solution strategies would also reduce the time needed to solve puzzles.

Two additional strategies being considered were:


*  **Investigate Triples.**  Like the investigation of pairs, three cells with the same three pencil marks in the same row, column or block must take on those three values so all other cells in the same row, column or block can have those cells stricken from their pencil marks.

*  **Hidden Singles.**  The program already does Naked Singles, which is simply playing cells that can only take on one number.  Hidden Singles are cells that are the only ones able to take on a specific number within a row, column or block.





 
