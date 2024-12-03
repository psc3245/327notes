**The Singleton Pattern**
The singleton pattern ensures a class has only one instance, and provides a global access point to it. 

```
using namespace std;

class Singleton {
	private:
		static Singleton *instance;
		Singleton() {
			// singleton init data goes here
		}
	public:
		Singleton(const Singleton &obj) = delete;
		static Singleton *getInstance() {
			if (instance == NULL) {
				instance = new Singleton();
			}
			return instance;
		}
};
```

### Memory Management Techniques
For data that ends up in memory (heap), an individual allocated piece of data will be called a node, cell, or object; and it will be clear from context if we mean object in the OO sense.

Define the **root pointers** or **root references** to be all the nodes referenced by a pointer on the stack, in a machine register, or in global memory.

The **pointer reachability relation** tells us if an object on the heat is reachable from a root pointer or a chain or pointers, starting at a root pointer.

An object is live if there is such a chain to an object on the heap.

**Pointer Reachability:**
For ay node M (or root reference) and some other node N:
	M is related to N (M $\rightarrow$ N) if M holds a reference (pointer) to N. In other words, M is related to N if M "can get to" N.

The set of **live** nodes in a heap is the **transitive referential closure** of the set of root nodes under the $\rightarrow$ relation.

LIVE = { N $\in$ Nodes | ( $\exists r \in ROOT$ | $r \rightarrow N$ ) or ( $\exists M \in LIVE$ | $M \rightarrow N$ ) }

Note that the set of Live objects (LIVE) is a conservative view of the set of objects that can be accessed by the program.
- By conservative, we mean that it over estimates the number of live objects.
	- That is, some objects are in LIVE but are not live (not really accessible)

What situation cause some object to not be used (accessible) but still in LIVE?

1. 
```
function() {
	*m
	...
	*m (last use)
	... // objet not accessed but still in live
	return; // *m out of scope
}
```
2. An uninitialized slot on the stack frame declared as a pointer.
3. An obsolete reference to an object left in a register by a compiler. 

A nodes "liveness" may be determined directly or indirectly.
- Direct methods 
	- Require that a record be associated with each node in the head of all references to that node from other nodes or root references.
		- reference counting
		- lists (distributed computing)
- Indirect methods
	- Regenerate the set of live nodes whenever a request by the user program fails

![[Screenshot 2024-12-02 at 9.26.33 PM.png]]

A cell is assumed to be a contiguous array of bytes / words divided into fields.
	A field may contain a pointer or a non pointer.

An object/cell/node that contains no pointer field is called an atom.

The data in the heap reachable from the root references forms a directed graph defined by the $\rightarrow$ relation from earlier

**Reference Counting**
- Introduced by Collins in 1960
- The method keeps track of if cells are in use by counting the references to each cell
- Each cell has an additional field called the reference count (RC)
- For simplicity, we assume all cells are stored in a "free pool"
	- The free pool is implemented as linked-list with the next field pointing to the next chunk of free memory.

![[Screenshot 2024-12-02 at 9.26.01 PM.png]]

### Allocation Algorithms

#### Reference Counting
Count the number of pointers to an object, it can be garbage collected if there are no pointers to it
![[Screenshot 2024-12-02 at 9.36.25 PM.png | 400]]
```
struct Cell {
	char memory[CELL_SIZE];
	union {
		Cell *next;
		int RC;
	} admin;
};

Cell *allocate() { // assumes free_list is not NULL
	Cell *nelcell = free_list;
	free_list = free_list -> next;
	return newcell;
}

void *new() {
	if (free_list == NULL) // throw error out of memory

	cell *newcell = allocate();
	newCell -> admin.RC = 1;
	return (void*) newcell;
}

void free (Cell *n) {
	n -> admin.next = free_list;
	free_list = n;
}

void delete(void *T) {
	((Cell *) T) -> admin.RC -= 1;
	if (( (Cell*) T) -> admin.RC == 0) {
		for each pointer U in T {
			delete(U);
		}
	}
	free( (Cell*) T);
}

void update(void **R, void *s) {
	delete(*R);
	((Cell*) S) -> admin.RC +=1;
}
```



![[Screenshot 2024-12-02 at 9.37.04 PM.png]]

**Strong Pointer vs Weak Pointer**
Strong pointer is normal pointer
Weak pointer does not increment Reference Count 
- Prev pointer in linked list is good example


### Mark and Sweep Algorithm (scan)
McCathy 1960
Cells are not reclaimed immediately, but rather "remain used in memory until collected."

So if there is a request for new memory and there is no memory available for immediate allocation, then processing is suspended while garbage collector is called to "sweep" all the currently unused cells from the head back into the pool of free cells.

This method relies on the traversal of all the live objects on the heap to identify what should **not** be returned to the free pool.

