# ALGORITHM (algorithm.h)

 | Header     | [{z88dk}/include/algorithm.h](https://raw.githubusercontent.com/z88dk/z88dk/master/include/algorithm.h)        | 
 | ----------------------------------------------------------------------------------------------------------------------------- 
 | Source     | [{z88dk}/libsrc/algorithm](https://github.com/z88dk/z88dk/tree/master/libsrc/algorithm/)                       | 
 | Include    | #include `<algorithm.h>`                                                                                          |
 | Linking    | -lalgorithm                                                                                                     |
 | Compile    | from {z88dk}/libsrc : make algorithm.lib ; make install                                                         |
 | Comments   | none                                                                                                            |

A collection of algorithms of general interest across z80 platforms.

# A* Search Algorithm

The [A* search algorithm](http://en.wikipedia.org/wiki/A%2A_search_algorithm) is a pathfinding algorithm that searches for the lowest cost path between the start node and the end node.  It is a best-first search algorithm, meaning it pursues paths of lowest cost first.  Although designed with pathfinding problems in mind, the A* algorithm can be used as a search-based solver for other types of problems cast as a pathfinding exercise.

This implementation of A* uses the [heap](library/adt#heap_static) from the Abstract Data Types library as a priority queue.  This means your program must also be linked with *-ladt* during compilation.  Each path is described by a single *struct astar_path*, a 7-byte block which contains a node, a pointer to a prefix path and a cost.  These blocks are dynamically allocated by the library functions by calling the user-supplied functions u_malloc() and u_free() as usual.  See the [library dynamic memory allocation](library/memory_allocation#memory_allocation_implicitly_performed_by_z88dk_libraries) discussion for details.

The nodes of the problem are represented by 16-bit unsigned integers.  To use A* you must first specify the start and end nodes by initializing two global variables:

	:::c
	uint astar_startNode;   // start node
	uint astar_destNode;    // end node


This implementation keeps track of all paths being investigated in a priority queue, which is implemented by the heap data type from the [Abstract Data Types](library/adt) library.  The heap data type requires an array to be supplied in which to maintain the queue of size equal to the maximum number of pending paths that can be investigated.  It also requires an unsigned integer variable to hold the number of paths currently in the queue and another to indicate the maximum size of the queue.  All of these variables must be declared globally in the program.  *astar_queue* must be initialized to point at the array and *astar_queueSize* must be initialized to hold the size of the array in words.

	:::c
	void **astar_queue;     // address of an array of void* that will hold the queue
	uint astar_queueSize;   // maximum number of pending paths in the queue
	uint astar_queueN;      // current number of pending paths in the queue


Before the problem begins, all nodes should be marked as "open".  A* will request that nodes be "closed" as they are visited by the search algorithm.  This prevents the algorithm from going in circles while searching for the solution path.

A* will call functions supplied by the program that provide information on the connectivity of nodes and the cost of moving between pairs of nodes.  The functions should have the following prototypes:

	:::c
	void [[__FASTCALL__]] astar_TestAndClose(uint node);
	
	//    Mark the node as 'closed' and return carry flag set
	//    if the node was already closed previously.  C functions
	//    should make use of the special z88dk keywords return_c()
	//    and return_nc() to exit with known carry flag state.
	
	void astar_Successor(uint *node, uint *cost);
	
	//    This function enumerates all the neighbours of node, one
	//    after the other on successive calls.  With node not -1, a
	//    new start node is indicated.  If node is -1, the next
	//    neighbour of the last valid node supplied should be returned.
	//    This function indicates to the algorithm the connectivity
	//    of the map.
	//
	//    If node is not -1, write into *node* its first neighbour
	//    and into *cost* the cost of moving from that node to the
	//    neighbour.
	//
	//    If node is -1, write the next neighbour into *node* and the
	//    cost of moving from the last valid node to this neighbour
	//    into *cost*.
	//
	//    Return with carry flag set when there are no more neighbours
	//    and reset if the neighbour returned is valid.  C functions
	//    should use the z88dk keywords return_c() and return_c()
	//    to exit with known carry flag state.
	
	uint [[__FASTCALL__]] astar_DestCost(uint node)
	
	//    (optional) Estimate the cost from the node indicated to
	//    the destination node.  These nodes may be widely separated
	//    so a cartesian distance metric is probably simplest.
	//
	//    This function is only called by astar_EstimateBestPath().
	//    If astar_EstimateBestPath() is not used, this function need
	//    not be defined.


Do not use the function names listed above.  Instead the following function pointers should be globally declared and initialized to point at the relevant functions:

	:::c
	void *astar_TestAndClose;   // void (*astar_TestAndClose)(uint node)
	void *astar_Successor;      // void (*astar_Successor)(uint *node, uint *cost)
	void *astar_DestCost;       // uint (*astar_DestCost)(uint node)


With the above initialization completed, a call to *astar_Search()* is made to determine the path of least cost between the start and end nodes.  There are three possible outcomes:

 1.  No path is found.  No cleanup is necessary, all memory will already be deallocated.
 2.  A solution is found.  The path returned is the lowest cost path.  This path and the priority queue will still be populated with other paths; a call to *astar_CleanUp()* should be made with the solution path as parameter to deallocate all memory.  Enumerating the nodes of the solution path should be done with a call to *astar_WalkPath()*.
 3.  An out of memory error occurs.  This can happen if allocation of more 7-byte blocks is not possible or if the queue fills up.  There are two things that can be done.  More memory can be added so that further 7-byte blocks can be allocated, in which case the search can be resumed with a call to *astar_SearchResume()*.  Alternatively, *astar_EstimateBestPath()* can be called to compute the lowest cost to the destination node of all the paths currently being considered.  This latter function only produces an estimate as the last node in all paths may be very far from the destination.  Memory must still be deallocated with *astar_CleanUp()* when finished.

## A* Search Algorithm API

**1. astar_Search()** *Uses IX*

Find the path of lowest cost between the start node and the end node.  There are three possible outcomes:


*  No path can be found.  0 is returned and the carry flag is reset.

*  The lowest cost path is found and returned.  The carry flag is set.  The path returned and pending paths in the queue must still be deallocated with a call to *astar_CleanUp()*.

*  An out of memory error occurs.  The current lowest cost path is returned and the carry flag is reset.  The path returned and pending paths in the queue must still be deallocated with a call to *astar_CleanUp()*.

	:::c
	struct astar_path *astar_Search(void);


**2. astar_SearchResume()** *Uses IX*

Resume a search previously initiated by a call to *astar_Search()* or *astar_SearchResume()*.

	:::c
	struct astar_path *astar_SearchResume(struct astar_path *p);
	// p = path to begin investigation with; returned by a previous call to astar_Search() or astar_SearchResume()
	//     if 0, further paths to investigate are retrieved from the priority queue.


**3. astar_EstimateBestPath()**

not implemented yet

	:::c
	struct astar_path *astar_EstimateBestPath(struct astar_path *p);
	// p =


**4. astar_PathLength()**

Return the number of nodes in the path.

	:::c
	uint astar_PathLength(struct astar_path *p);
	// p = a path as returned by astar_Search()


**5. astar_WalkPath()**

Write at most *n* nodes from the path *p* into the supplied array *node_arr*.  Return a pointer inside *node_arr* that points at the first node of the path written.  The carry flag is set if not all nodes of the path could be written into the array.

	:::c
	uint *astar_WalkPath(struct astar_path *p, uint *node_arr, uint n);
	//        p = path whose nodes are to be enumerated
	// node_arr = an array of unsigned integers into which the nodes of the path will be written
	//        n = maximum size of the array


**6. astar_DeletePath()**

Deallocate memory associated with the path.

	:::c
	void astar_DeletePath(struct astar_path *p);
	// p = path to delete


**7. astar_CleanUp**

Delete the path *p* and all pending paths in the priority queue.  A call to this function completely deallocates all memory associated with A*.

	:::c
	void astar_CleanUp(struct astar_path *p);
	// p = path to delete, 0 if none


## Code Example

