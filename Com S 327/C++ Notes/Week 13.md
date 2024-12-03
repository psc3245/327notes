Mark and Sweep Algorithm Continued
From Week 12:
### Mark and Sweep Algorithm (scan)
McCathy 1960
Cells are not reclaimed immediately, but rather "remain used in memory until collected."

So if there is a request for new memory and there is no memory available for immediate allocation, then processing is suspended while garbage collector is called to "sweep" all the currently unused cells from the head back into the pool of free cells.

This method relies on the traversal of all the live objects on the heap to identify what should **not** be returned to the free pool.

```
void *new() {
	if (free_pool == NULL) {
		arksweep(); // assume it throws out of memory error
	}
	newcell = allocate();
	return newcell;
}

void marksweep() {
	for (R \in Root Pointers) {
		mark(R);
	}
	sweep();
	if (free_pool == null) {
		throw out of memory error
	}
}

void mark(N) {
	if (N == null) return;
	if (N -> markbit == UNMARKED) {
		N -> markbit = MARKED;
		for each (pointers/reference M in N) {
			mark(M); // i.e. mark everything that N points to
		}
	}
}
```
![[Screenshot 2024-12-02 at 9.48.30 PM.png]]
```
void sweep() {
	N = heap_bottom;
	while (N < heap_top) {
		if (N -> markbit == UNMARKED) {
			free(N);
		}
		else {
			N -> markbit = UNMARKED;
		}
		N += the size of the cell (go to next cell)
	}
}
```
![[Screenshot 2024-12-02 at 9.50.10 PM.png]]

**Problems with the Mark Sweep Algorithm:**
- Worst case running time is O(size of heap)
- Start/stop issue
- Fragment memory
	- causes problem if variable size of memory can be allocated

### The Copying Algorithm (Copying Garbage Collector)
- Divide the heap into two equal spaces
- One contains correct data (toSpace)
- One contains obsolete data (fromSpace)
- Copy from "fromSpace" to "toSpace"

**Advantage**: Data is compacted (holes removed)
**Disadvantage**: Lose half of available memory

```
init() {
	toSpace = heap_bottom;
	space-size = heap_size / 2;
	top_of_space = toSpace + space_size - 1;
	fromSpace = top_of_space + 1;
	free = toSpace;
}
```
![[Screenshot 2024-12-02 at 9.55.12 PM.png]]
We now allow the user to request an arbitrary size of cells.
Each cell knows it's size.
```
void *new(int n) { // n == requested cell size
	if ((free + n) > top_of_space) {
		flip();
	}
	if ((free + n) > top_of_space) {
		throw out of memory error
	}
	temp = free;
	free += n;
	return temp;
}

void flip() {
	temp = fromSpace;
	fromSpace = toSpace;
	toSpace = temp;
	free = toSpace;
	top_op_space = toSpace + space_size - 1;
	for each (R \in root pointers) {
		R = copy(R);
	}
}

var copy(p) { // var replace with what type p is?
	if (p == null) return p;
	if (atom(P)) return p; // true if p does not contain references to 
							other objects or values
	
	if (!forwarded) { // forwarded is likely a flag used to indicate if p
					 has been prcessed, and if so, to move on 
					 and avoid re copying.
		n = sizeof(*p);
		p = free;
		free = free + n;
		temp = p[0];
		p[0] = p;
		p[0] = copy(temp);
		for (int i = 1; i < n; i++) {
			p[i] = copy(p[i]);
		}
	}
	return p[0];
}
```
![[Screenshot 2024-12-02 at 10.06.30 PM.png]]



### Templates

// Template functions
```
template< Class T >
T max (T a, T b) {
	return a > b ? a : b;
}
```

class template specialization
```
#include <iostream>

using namespace std;

template < class T >
class Foo {
	public:
		void f() {
			cout << "foo normal" << endl; // endl == end line
		}
};

template <>
class Foo<char> {
	public:
		void f() {
			cout << "foo special" << endl;
		}
};
```

```
int main() {
	Foo<int> x;
	Foo<char> y;
	x.f();
	y.f();
}
```
Output:
foo normal
foo special