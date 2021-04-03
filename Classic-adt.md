# ABSTRACT DATA TYPES LIBRARY (adt.h, adt.lib)

 | Header     | [{z88dk}/include/adt.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/adt.h)                |          
 | -----------|-------------------------------------------------------------------------------------------------------------          
 | Source     | [{z88dk}/libsrc/adt](https://github.com/z88dk/z88dk/tree/master/libsrc/adt)                                 |         
 | Include    | #include `<adt.h>`                                                                                                |     
 | Linking    | -ladt                                                                                                           |     
 | Compile    | from {z88dk}/libsrc : make adt.lib ; make install                                                                    |
 | Comments   | **1.** Some of the data structures in this library need to allocate memory implicitly in the course of          |     
 | | performing their functions.  All z88dk libraries that need to perform this implicit memory allocation           |                
 | | do so through user-supplied functions as documented on the [memory allocation](library/memory allocation) page.  |               
 | | **2.** Some of the data structures in this library make use of the IX and IY register pairs which can           |                
 | | be a problem for some z80 targets.                                                                              |                

The [abstract data types](http://en.wikipedia.org/wiki/Abstract_data_type) library is a collection of common data structures frequently used in programs.  The intent is to provide a simple, comfortable, and debugged interface to commonly used software structures.

Data structures included in this library:


*  AVL Tree (not implemented yet, dynamic, a balanced binary tree)

*  Byte Circular Buffer (not implemented yet, static, a byte-indexed array)

*  Word Circular Buffer (not implemented yet, static, a word-indexed array)

*  [ Doubly Linked List](adt#doubly_linked_list_dynamic ) (dynamic)

*  Singly Linked List (not implemented yet, static, a single pointer buried in user structs)

*  Static Hash Table (not implemented yet, static, an array)

*  [ Heap](adt#heap_static ) (static)

*  Map (not implemented yet, dynamic, using AVL tree)

*  [ Stack](adt#stack_dynamic ) (dynamic)

*  [ Queue](adt#queue_dynamic ) (dynamic)

# Doubly Linked List (dynamic)

A [linked list](http://en.wikipedia.org/wiki/Linked_list) is an ordered list of items.  This linked list implementation is a doubly-linked list that dynamically allocates nodes to hold each item.  The fact that it is doubly-linked means each item in the list has a pointer to the item following it and to the item preceeding it which allows O(1) addition and removal of items from anywhere in the list.  Because this type dynamically allocates nodes to store items, your program must supply u_malloc() and u_free() functions that the library can call to perform these memory allocations implicitly.  This is explained on the [memory allocation](library/memory allocation) page.

This linked list type maintains a single internal pointer called the 'current pointer' that can point at one of the following: an item in the list, before the start of the list or after the end of the list.  List operations can utilize this 'current pointer' or can reference the start and end of the list directly.  This will be clear on review of the API functions below.

## List API

**1. adt_ListCreate()** ; *O(1)*

Creates an empty list and returns a pointer to the list structure.  u_malloc() is called to get a 9-byte block of memory to create the list handle.  If the allocation is unsuccessful, 0 will be returned and the list will not be created.

	:::c
	struct adt_List *adt_ListCreate(void); 


**2. adt_ListDelete()** ; *O(N)* ; *Uses IX*

Deletes the list.  The list handle will be deallocated and will no longer be valid.  u_free() is called to free any allocated memory associated with the list.

	:::c
	void adt_ListDelete(struct adt_List *list, void *delete);
	//   list = pointer to list handle
	// delete = void (*delete)(void *item)
	//            For each item in the list, this function is called with the item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty list.
	//          Set delete = 0 to do no user cleanup on items.


**3. adt_ListCreateS()** ; *O(1)*

Initializes an existing struct_adt_List to create an empty list.  This static create option is offered as an alternative to the dynamic version in order to simplify the implicit memory allocation function u_malloc() so that it only has to worry about allocating list nodes.

	:::c
	void adt_ListCreateS(struct adt_List *list);
	// list = pointer to an exiting struct_adt_List


**4. adt_ListDeleteS()** ; *O(N)* ; *Uses IX*

Deletes the list.  The list handle will not be deallocated but will no longer be valid.  u_free() is called to free any allocated memory associated with the list.  This function should be called to delete lists created with adt_ListCreateS().

	:::c
	void adt_ListDeleteS(struct adt_List *list, void *delete);
	//   list = pointer to list handle
	// delete = void (*delete)(void *item)
	//            For each item in the list, this function is called with the item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty list.
	//          Set delete = 0 to do no user cleanup on items.


**5. adt_ListCount()** ; *O(1)*

Returns number of items in list.

	:::c
	unsigned int adt_ListCount(struct adt_List *list); 
	// list = pointer to list handle


**6. adt_ListPushBack() *alias adt_ListAppend()*** ; *O(1)*

Adds item to the end of the list.  u_malloc is called to allocate a 6-byte node to hold the item.  If allocation fails the item is not added to the list and 0 is returned to report failure.  If successful the list's current pointer will be updated to point at the new item.

	:::c
	int adt_ListPushBack(struct adt_List *list, void *item);
	int adt_ListAppend(struct adt_List *list, void *item); 
	// list = pointer to list handle
	// item = item to add to list


**7. adt_ListPushFront() *alias adt_ListPrepend()*** ; *O(1)*

Adds item to the beginning of the list.  u_malloc is called to allocate a 6-byte node to hold the item.  If allocation fails the item is not added to the list and 0 is returned to report failure.  If successful the list's current pointer will be updated to point at the new item.

	:::c
	int adt_ListPushFront(struct adt_List *list, void *item);
	int adt_ListPrepend(struct adt_List *list, void *item);
	// list = pointer to list handle
	// item = item to add to list


**8. adt_ListAdd()** ; *O(1)*

Adds item after the the item pointed at by the list's current pointer.  u_malloc is called to allocate a 6-byte node to hold the item.  If allocation fails the item is not added to the list and 0 is returned to report failure.  If successful the list's current pointer will be updated to point at the new item.

	:::c
	int adt_ListAdd(struct adt_List *list, void *item); 
	// list = pointer to list handle
	// item = item to add to list


**9. adt_ListInsert()** ; *O(1)*

Adds item before the item pointed at by the list's current pointer.  u_malloc is called to allocate a 6-byte node to hold the item.  If allocation fails the item is not added to the list and 0 is returned to report failure.  If successful the list's current pointer will be updated to point at the new item.

	:::c
	int adt_ListInsert(struct adt_List *list, void *item); 
	// list = pointer to list handle
	// item = item to add to list


**10. adt_ListFirst()** ; *O(1)*

Makes the list's current pointer point at the first item in the list.  Returns the first item or 0 if the list is empty.

	:::c
	void *adt_ListFirst(struct adt_List *list); 
	// list = pointer to list handle


**11. adt_ListLast()** ; *O(1)*

Makes the list's current pointer point at the last item in the list.  Returns the last item or 0 if the list is empty.

	:::c
	void *adt_ListLast(struct adt_List *list); 
	// list = pointer to list handle


**12. adt_ListNext()** ; *O(1)*

Moves the list's current pointer to the next item in the list.  Returns this next item or 0 if the current pointer has moved past the end of the list.

	:::c
	void *adt_ListNext(struct adt_List *list); 
	// list = pointer to list handle


**13. adt_ListPrev()** ; *O(1)*

Moves the list's current pointer to the previous item in the list.  Returns this previous item or 0 if the current pointer has moved past the beginning of the list.

	:::c
	void *adt_ListPrev(struct adt_List *list); 
	// list = pointer to list handle


**14. adt_ListCurr()** ; *O(1)*

Returns the item pointed at by the list's current pointer.  Returns 0 if the list is empty or if the current pointer points either past the end of list or before the start of the list.

	:::c
	void *adt_ListCurr(struct adt_List *list); 
	// list = pointer too list handle


**15. adt_ListRemove()** ; *O(1)*

Removes the item pointed at by the list's current pointer.  The just-removed item is returned and the current pointer is advanced to the next item in the list.  If the current pointer wasn't pointing at anything (the list is empty or it points either past the end of the list or before the start of the list) 0 is returned.  If an item is removed u_free is called to release the memory used as the item's container.

	:::c
	void *adt_ListRemove(struct adt_List *list); 
	// list = pointer to list handle


**16. adt_ListPopBack() *alias adt_ListTrim()*** ; *O(1)*

Removes the item at the end of the list and returns it.  If the list is empty, 0 is returned and the carry flag is reset.  The list's current pointer points at the new last item in the list.  If an item is removed u_free is called to release the memory used as the item's container.

	:::c
	void *adt_ListPopBack(struct adt_List *list);
	void *adt_ListTrim(struct adt_List *list);
	// list = pointer to list handle


**17. adt_ListPopFront()** ; *O(1)*

Removes the item at the front of the list and returns it.  If the list is empty, 0 is returned and the carry flag is reset.  The list's current pointer points at the new first item in the list.  If an item is removed u_free is called to release the memory used as the item's container.

	:::c
	void *adt_ListPopFront(struct adt_List *list);
	// list = pointer to list handle


**18. adt_ListConcat()** ; *O(1)*

Concatenates list2 to the end of list1.  List2 is destroyed in the process and the memory for the list2 handle is freed through u_free.

	:::c
	void adt_ListConcat(struct adt_List *list1, struct adt_List *list2); 
	// list1 = pointer to list handle
	// list2 = pointer to list handle


**19. adt_ListSetCurrBefore()** ; *O(1)*

Sets the list's current pointer to point before the start of the list.

	:::c
	void adt_ListSetCurrBefore(struct adt_List *list);
	// list = pointer to list handle


**20. adt_ListSetCurrAfter()** ; *O(1)*

Sets the list's current pointer to point after the end of the list.

	:::c
	void adt_ListSetCurrAfter(struct adt_List *list);
	// list = pointer to list handle


**21. adt_ListSetCurr()** ; *O(1)*

Set the list's current pointer to point at a specific node in the list.  This node must be a part of the list and may be obtained from a previous position of the current pointer directly from the struct adt_List.  If the specific node is 0, the list's current pointer is unchanged.

	:::c
	void adt_ListSetCurr(struct adt_List *list, struct adt_ListNode *n);
	// list = pointer to list handle
	//    n = pointer to struct adt_ListNode ; if 0 the list's current pointer is unchanged


**22. adt_ListSearch()** ; *O(N)* ; *Uses IX, IY*

Searches the list FROM THE LIST'S CURRENT POINTER (this is easy to forget; to search the entire list call adt_ListFirst() or adt_ListSetCurrBefore() first).  The search is a user-controlled iteration over the list's items.  For each item visited, the user-supplied function "char (*match)(void *item1, void *item2)" is called with "item2" equal to the list item and "item1" equal to the parameter passed into this function.  If the user-supplied function match() returns with non-zero result, the list search function terminates with the list's current pointer pointing at "item2".  The value returned is this "item2" or 0 if the user-supplied function never matches.

	:::c
	void *adt_ListSearch(struct adt_List *list, void *match, void *item1); 
	//  list = pointer to list handle
	// item1 = item to be matched in list
	// match = char (*match)(void *item1, void *item2)
	//         Return non-zero to indicate a match between item1 and item2.
	//         This will terminate the list search and return item2 as result.


## Code Example 1

Declarations of the functions u_malloc() and u_free() which perform memory allocation on behalf of the ADT library are not shown.

This code snippet shows a subroutine that takes as input a list and an index N.  The subroutine returns the Nth item in the list.  adt_ListSearch() is used to perform an iteration over all items in the list, only stopping when the helper function determines that N-1 items have already been passed over.  N=0 is used to select the first item in the list.

	:::c
	int selectnth_helper(uint *n, void *item)
	{
	   return !((*n)--);  // if *n==0 return nonzero to stop search
	}
	
	void *SelectNthInList(struct adt_List *ls, int n)
	{
	      adt_ListSetCurrBefore(ls);
	      return (adt_ListSearch(ls, selectnth_helper, &n));
	}
	
	// example usage
	
	item = SelectNthInList(ls, 4);


## Code Example 2

Declarations of the functions u_malloc() and u_free() which perform memory allocation on behalf of the ADT library are not shown.

This subroutine example takes an unsorted list of items and sorts the list according to a user-provided criteria function.  The sorting algorithm used is a simple selection sort and the criteria function takes as parameter an item in the list and returns a single unsigned integer corresponding to the item's absolute ordinal position.  The subroutine destroys the list it takes as parameter and returns a new sorted list containing the same items.

	:::c
	struct adt_List *SortList(struct adt_List *ls, void *criteria)
	{
	   struct adt_List *newlist;
	   struct adt_ListNode *position;
	   uint min, val;
	   
	   newlist = adt_ListCreate();
	   
	   while (adt_ListCount(ls)) {
	      
	      min = (criteria)(adt_ListFirst(ls));
	      position = ls->current;
	      
	      while (adt_ListNext(ls)) {
	      
	         val = (criteria)(adt_ListCurr(ls));
	         if (val < min) {
	            min = val;
	            position = ls->current;
	         }
	         
	      }         
	      
	      adt_ListSetCurr(ls, position);
	      adt_ListAppend(newlist, adt_ListRemove(ls));
	      
	   }
	   
	   adt_ListDelete(ls, 0);
	   return newlist;
	}
	
	// example usage
	
	struct {
	   uint cost;
	   char *name;
	} myItems;
	
	struct adt_List *ls;                    // list of myItems
	
	uint myCriteria(struct myItems *i)
	{
	   return i->cost;
	}
	
	ls = SortList(ls, myCriteria);          // sorts list in increasing cost order


## Code Example 3

Declarations of the functions u_malloc() and u_free() which perform memory allocation on behalf of the ADT library are not shown.

This subroutine example shuffles a list into random order.  As with the previous example, the list passed in is destroyed and a new one is returned.  The subroutine makes use of SelectNthInList() from code example 1.  The function RND(N) returns a random number in the range [0,N-1].

	:::c
	uint RND(uint range) {
	   return (uint)(((long)(range) * rand()) / (long)(RAND_MAX));
	}
	
	struct adt_List *ShuffleList(struct adt_List *ls)
	{
	   struct adt_List *newlist;
	   uint n;
	   
	   newlist = adt_ListCreate();
	   
	   while (n = adt_ListCount(ls)) {
	   
	      SelectNthInList(ls, RND(n));                     // pick a list item at random
	      adt_ListAppend(newlist, adt_ListRemove(ls));     // remove it from original list and add it to the new one
	      
	   }
	   
	   adt_ListDelete(ls, 0);
	   return newlist;
	}
	
	// example usage
	
	ls = ShuffleList(ls);


# Heap (static)

A [ heap](http://en.wikipedia.org/wiki/Heap_%28data_structure%29 ) maintains a collection of items in a special order that allows insertion of a new item and extraction of either the largest or the smallest item quickly.  Because of this property a heap is also sometimes called a priority queue, a queue which allows random items to be added to it but items are removed from the queue in either increasing or decreasing order.

This heap implementation maintains the heap as an array of void*.  Each entry in the array is two bytes in size and can be treated as an integer or as a pointer to an object.  The user must supply an array of void* large enough to hold the maximum number of items that will be placed in the heap and must supply an integer variable to keep track of the number of items in the heap.  The heap functions will modify the integer variable directly as items are added and removed.  The user must also supply a compare function that allows comparison of items in the heap.  The sense of the compare function determines if the heap is a min-heap (items are extracted from the heap in increasing order) or a max-heap (items are extracted from the heap in decreasing order).

The heap algorithm is conducive to using array indices starting at 1 rather than the usual C array which uses 0 as the initial index.  This means the C array you provide for the heap must be one item larger than usual, with the heap using array indices beginning at 1 and leaving the 0th index unused.  Eg, if the heap must hold a maximum of 20 items, a static C array declared for the heap should contain space for 21 items with the heap using indices 1 through 20 and leaving index 0 alone.  See the code example below for clarification.

## Heap API

**1. adt_Heapify()** ; *O(N*log(N))* ; *Uses IX, IY, AF'*

Takes an array of unsorted items as input and sorts it into a heap.

	:::c
	void adt_Heapify(void **array, uint n, void *compare);
	//   array = address of an array of void*
	//       n = number of items in the array
	// compare = char (*compare)(void **item1, void **item2)
	//             return negative if item1>item2 for max heap (extract largest item first)
	//             return negative if item1<item2 for min heap (extract smallest item first)
	//           NOTE: item1 and item2 are addresses within the array not the void* values stored in the array


**2. adt_HeapAdd()** ; *O(log(N))* ; *Uses IX, IY, AF'*

Adds the item to the heap and increases N by one.

	:::c
	void adt_HeapAdd(void *item, void **array, uint *n, void *compare);
	//    item = item to add to heap
	//   array = address of an array of void* that is a valid heap
	//       n = address of an integer holding the number of items in the array
	// compare = char (*compare)(void **item1, void **item2)
	//             return negative if item1>item2 for max heap (extract largest item first)
	//             return negative if item1<item2 for min heap (extract smallest item first)
	//           NOTE: item1 and item2 are addresses within the array not the void* values stored in the array


**3. adt_HeapExtract()** ; *O(log(N))* ; *Uses IX, IY, AF'*

Removes and returns the largest item (max heap) or smallest item (min heap) from the array and decreases N by one.  If the heap was
empty to start with, zero is returned.  Note that array[1] will always contain the next item extracted (ie either the maximum or minimum
item in the heap).

This function also writes the extracted item into the end of the heap array where a vacant slot is created by the removal of one
item from the heap.  A side effect of this operation is that if the heap is emptied by extracting all items one after the other,
the array left behind will be sorted either in descending order for a min heap or in ascending order for a max heap.  This is an
implementation of [ heapsort](http://en.wikipedia.org/wiki/Heapsort ) whose application is demonstrated in the code example at
the end of this section.  Heapsort has an important space advantage over [ quicksort](library/stdlib#void_l_qsort_void_base_size_t_n_char_cmp_const_void_keyval_const_void_datum ), especially on small 8-bit
processors, since it requires no extra memory or stack space to perform the sort whereas, in worst case, stack usage by quicksort
is proportional to the number of items being sorted.

	:::c
	void *adt_HeapExtract(void **array, uint *n, void *compare);
	//   array = address of an array of void* that is a valid heap
	//       n = address of an integer holding the number of items in the array
	// compare = char (*compare)(void **item1, void **item2)
	//             return negative if item1>item2 for max heap (extract largest item first)
	//             return negative if item1<item2 for min heap (extract smallest item first)
	//           NOTE: item1 and item2 are addresses within the array not the void* values stored in the array


## Code Example

	:::c
	#include `<stdio.h>`
	#include `<stdlib.h>`
	#include `<adt.h>`
	
	// Example command line compile with ZX Spectrum as target:
	// zcc +zx -vn heap.c -o heap -ladt -lndos -create-app
	
	unsigned int n;                                     // number of items in heap
	void *heap[101];                                    // max heap size is 100 items
	
	int compare(int *a, int *b)                         // params passed in are addresses within the heap array
	{                                                   // the void* stored in the heap array are interpretted as integers
	   if (*a < *b)
	      return -1;                                    // this will be a min heap -- smallest items are extracted first
	   return 1;
	}
	
	main()
	{
	   int i;
	
	   printf("First we will fill the array with random numbers....\n\n");
	
	   for (i=1; i!=101; i++)
	      printf("%d, ", heap[i] = rand()&0xfff);       // fill up the heap with random integers from 0-4095
	   n = 100;                                         // there are 100 items in the heap
	
	   printf("\n\nNext we reorder the array into a heap...\n\n");
	
	   adt_Heapify(heap, n, compare);                   // change random array into a heap
	   for (i=1; i!=101; i++)
	      printf("%d, ", heap[i]);
	
	   printf("\n\nAnd now we extract items one at a time in order...\n\n");
	
	   while (n!=0)
	      printf("%d, ", adt_HeapExtract(heap, &n, compare));
	
	   printf("\n\nA side effect of the extraction is that the array is now sorted!  This is an example of heapsort in action...\n\n");
	
	   for (i=1; i!=101; i++)
	      printf("%d, ", heap[i]);
	
	   printf("\n\nTHE END\n\n");
	}


# Stack (dynamic)

A [ stack](http://en.wikipedia.org/wiki/Stack_%28data_structure%29 ) is a container that holds a collection of items accessed in LIFO order.  That is, items are stored in Last-In-First-Out order: the last item pushed onto the stack is also the first item popped off it.  A common analogy of stack operation is a stack of plates in a cafeteria.  You can only put plates on top of the stack and you can only remove plates from the top.  Each plate you take off the top was also the last one added to the stack.

This stack implementation is a linked list of items with nodes holding each item dynamically allocated.  The library gets memory for nodes implicitly using user supplied functions u_malloc() and u_free() as described on the [memory allocation](library/memory allocation) page.  To use stack you must supply these memory allocation functions.

## Stack API

**1. adt_StackCreate()** ; *O(1)*

Creates an empty stack and returns a pointer to the stack structure.  u_malloc() is called to get a 4-byte block of memory to create the stack handle.  If the allocation is unsuccessful, 0 will be returned and the stack will not be created.

	:::c
	struct adt_Stack *adt_StackCreate(void);


**2. adt_StackDelete()** ; *O(N)* ; *Uses IX*

Deletes the stack, invalidating the stack handle. u_free() is called to deallocate memory used to store each user item in the stack.

	:::c
	void adt_StackDelete(struct adt_Stack *s, void *delete);
	//      s = address of stack handle
	// delete = void (*delete)(void *item)
	//            For each item in the stack, this function is called with an item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty stack.
	//          Set delete = 0 to do no user cleanup on items.


**3. adt_StackCreateS()** ; *O(1)*

Initializes an existing struct_adt_Stack to create an empty stack.  This static create option is offered as an alternative to the dynamic version in order to simplify the implicit memory allocation function u_malloc() so that it only has to worry about allocating stack nodes.

	:::c
	void adt_StackCreateS(struct adt_Stack *s);
	// s = pointer to an existing struct_adt_Stack


**4. adt_StackDeleteS()** ; *O(N)* ; *Uses IX*

Deletes the stack.  Memory for the struct_adt_Stack is not deallocated but it is no longer valid. u_free() is called to deallocate memory used to store each user item in the stack.  This function should be called to delete stacks created with adt_StackCreateS().

	:::c
	void adt_StackDeleteS(struct adt_Stack *s, void *delete);
	//      s = address of stack handle
	// delete = void (*delete)(void *item)
	//            For each item in the stack, this function is called with an item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty stack.
	//          Set delete = 0 to do no user cleanup on items.


**5. adt_StackPush()** ; *O(1)*

Pushes the item onto the stack.  u_malloc is called to allocate a 4-byte node to hold the item.  If the 
allocation is unsuccessful, 0 is returned with the carry flag reset and the item will not be placed on the stack.

	:::c
	int adt_StackPush(struct adt_Stack *s, void *item);
	//    s = address of stack handle
	// item = item to be pushed onto stack


**6. adt_StackPop()** ; *O(1)*

Pops the top item on the stack and returns it.  If the stack was empty, zero is returned with the carry flag reset.  If an item
was popped, u_free() is called to free the stack node used to hold the user item.

	:::c
	void *adt_StackPop(struct adt_Stack *s);
	// s = address of stack handle


**7. adt_StackPeek()** ; *O(1)*

Returns the top item on the stack without popping it.  If the stack was empty, zero is returned.

	:::c
	void *adt_StackPeek(struct adt_Stack *s);
	// s = address of stack handle


**8. adt_StackCount()** ; *O(1)*

Returns the number of items in the stack.

	:::c
	unsigned int adt_StackCount(struct adt_Stack *s);
	// s = address of stack handle


## Code Example

This example uses the standard malloc() and free() functions for memory allocations.  It is usually a better idea to use the block memory allocator, especially for programs that allocate and free fixed-size memory blocks over and over again.  Visit the [memory allocation](library/memory allocation) page for a discussion on this topic.

	:::c
	#include `<stdio.h>`
	#include `<malloc.h>`
	#include `<adt.h>`
	
	// Example command line compile with ZX Spectrum as target:
	// zcc +zx -vn stack.c -o stack -ladt -lndos -lmalloc -create-app
	
	// variables
	
	char str[] = "the quick brown fox jumped over the lazy dogs";
	struct adt_Stack *s;
	
	// user-supplied malloc and free functions for use by libraries
	// that perform implicit memory allocation
	
	void *u_malloc(unsigned int size)
	{
	   return malloc(size);               // using the usual malloc
	}
	
	void u_free(void *addr)
	{
	   free(addr);                        // using the usual free
	}
	
	// z88dk's malloc implementation requires some set-up
	
	long heap;         // malloc's heap pointer
	char marray[500];  // we'll reserve these 500 bytes compiled as part of the binary for malloc
	
	main()
	{
	   char *p, c;
	   
	   // initialize malloc's heap
	
	   mallinit();         // sets malloc's heap to empty
	   sbrk(marray, 500);  // make marray available to the heap
	
	   printf("I've got me a string:\n%s\n\n", str);
	   
	   // push chars of string onto stack
	   
	   printf("And I'm going to push each letter onto the stack one at a time:\n\n");
	   
	   s = adt_StackCreate();                    // create empty stack
	   for (p = str; c = *p; p++) {
	      printf("%c, ", c);
	      adt_StackPush(s, (void *)(c));        // push letter onto stack
	   }
	   
	   // popped letters off stack will be in reverse order
	   
	   printf("\n\nAnd now I pop them off in reverse order:\n\n");
	   
	   while (c = (char)(adt_StackPop(s)))      // adt_StackPop will return 0 if it is empty
	      printf("%c", c);                      // this is good enough as no 0 chars were put on stack
	   
	   printf("\n\nTHE END\n");
	}


# Queue (dynamic)

A [ queue](http://en.wikipedia.org/wiki/Queue ) is a container that holds a collection of items accessed in FIFO order.  That is, items are stored in First-In-First-Out order: the first item placed in the queue is the first item retrieved from it.

This queue implementation is a linked list of items with nodes holding each item dynamically allocated.  The library gets memory for nodes implicitly using user supplied functions u_malloc() and u_free() as described on the [memory allocation](library/memory allocation) page.  To use queue you must supply these memory allocation functions.

## Queue API

**1. adt_QueueCreate()** ; *O(1)*

Creates an empty queue and returns a pointer to the queue structure.  u_malloc() is called to get a 6-byte block of memory to create the queue handle.  If the allocation is unsuccessful, 0 will be returned and the queue will not be created.

	:::c
	struct adt_Queue *adt_QueueCreate(void);


**2. adt_QueueDelete()** ; *O(N)* ; *Uses IX*

Deletes the queue, invalidating the queue handle. u_free() is called to deallocate memory used to store each user item in the queue.

	:::c
	void adt_QueueDelete(struct adt_Queue *q, void *delete);
	//      q = address of queue handle
	// delete = void (*delete)(void *item)
	//            For each item in the queue, this function is called with the item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty queue.
	//          Set delete = 0 to do no user cleanup on items.


**3. adt_QueueCreateS()** ; *O(1)*

Initializes an existing struct_adt_Queue to create an empty queue.  This static create option is offered as an alternative to the dynamic version in order to simplify the implicit memory allocation function u_malloc() so that it only has to worry about allocating queue nodes.

	:::c
	void *adt_QueueCreateS(struct adt_Queue *q);
	// q = pointer to an existing struct_adt_Queue


**4. adt_QueueDeleteS()** ; *O(N)* ; *Uses IX*

Deletes the queue.  The struct_adt_Queue itself is not deallocated but it is invalid.  u_free() is called to deallocate memory used to store each user item in the queue.  This function should be called to delete queues created with adt_QueueCreateS().

	:::c
	void adt_QueueDeleteS(struct adt_Queue *q, void *delete);
	//      q = address of queue handle
	// delete = void (*delete)(void *item)
	//            For each item in the queue, this function is called with the item as parameter.
	//            This is meant as an opportunity for user-cleanup of items that are in a non-empty queue.
	//          Set delete = 0 to do no user cleanup on items.


**5. adt_QueueCount()** ; *O(1)*

Returns the number of items in the queue.

	:::c
	unsigned int adt_QueueCount(struct adt_Queue *q);
	// q = address of queue handle


**6. adt_QueueFront()** ; *O(1)*

Returns the item at the front of the queue without removing it from the queue.  If the queue is empty, 0 is returned and the carry flag is reset.

	:::c
	void *adt_QueueFront(struct adt_Queue *q);
	// q = address of queue handle


**7. adt_QueueBack()** ; *O(1)*

Returns the item at the end of the queue without removing it from the queue.  If the queue is empty, 0 is returned and the carry flag is reset.

	:::c
	void *adt_QueueBack(struct adt_Queue *q);
	// q = address of queue handle


**8. adt_QueuePushFront()** ; *O(1)*

Adds item to the front of the queue.  u_malloc() is called to get a 4-byte block of memory to hold the item.  If the allocation fails, 0 is returned, the carry flag is reset and the item is not added to the queue.

	:::c
	int adt_QueuePushFront(struct adt_Queue *q, void *item);
	//    q = address of queue handle
	// item = item to add to queue


**9. adt_QueuePopFront()** ; *O(1)*

Removes the item at the front of the queue and returns it.  If the queue is empty, 0 is returned and the carry flag is reset.

	:::c
	void *adt_QueuePopFront(struct adt_Queue *q);
	// q = address of queue handle


**10. adt_QueuePushBack()** ; *O(1)*

Adds item to the end of the queue.  u_malloc() is called to get a 4-byte block of memory to hold the item.  If the allocation fails, 0 is returned, the carry flag is reset and the item is not added to the queue.

	:::c
	int adt_QueuePushBack(struct adt_Queue *q, void *item);
	//    q = address of queue handle
	// item = item to add to queue


**11. adt_QueuePopBack()** ; *O(N)*

Removes the item at the end of the queue and returns it.  If the queue is empty, 0 is returned and the carry flag is reset.

	:::c
	void *adt_QueuePopBack(struct adt_Queue *q);
	// q = address of queue handle


## Code Example

This example uses the standard malloc() and free() functions for memory allocations.  It is usually a better idea to use the block memory allocator, especially for programs that allocate and free fixed-size memory blocks over and over again.  Visit the [memory allocation](library/memory allocation) page for a discussion on this topic.

# Questions & Answers

Post common questions and answers here. Report any bugs or ask a question not listed here on the [z88dk mailing list](start#support).


