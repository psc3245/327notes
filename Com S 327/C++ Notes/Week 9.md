Example String class from C++
```
class string {
	private:
		char *buffer;
	public:
		char & operator[](unsigned int index)
		{ return buffer[index]; }
}
```

Returning a reference is usually more efficient than returning a class (for example)
	Suppose we have a Pair class that encapsulates two double type values.It also has a constructor that creates a Pair given two values.
	Now consider the function minMax that returns a pair that is the min and max values in an array
```
Pair & minMax(double data[], int n) {
	double minval = data[0];
	double maxval = data[0];
	for (int i = 1; i < n; i++) {
		if (data[i] < minval) minval = data[i];
		if (data[i] > maxval) maxval = data[i];
	}
	Pair p(minval, maxval);
	return p;
}
```
The "&" in the function declaration is causing a problem here - we are returning a reference to something that has just gone out of scope, and is thus being freed from the stack memory.

**Lifetime Errors**: Once a function/method ends, the variables stored on the stack associated with that method call are no longer valid.
Can use heap memory to solve this problem:
```
int main() {
	Box b;
	Box *b1;
	b1 = new Box(7); // keyword new returns a pointer to a malloc'd object
	Box b2 = *b1; // "take what is in b1's pointer and make a shallow copy" 
		// "of it"
	b2 = b;
}
```

**In C++ besides constructors, there are 3 special methods:**
- Destructor: automatically called when an object goes out of scope
- Copy Constructor: copies the contents of one object to a newly constructed object
- Operator = Assigns (copies) contents to an object after creation
It is important to know how and when these are called

triple.h:
```
class Triple {
	private:
		int v1;
		int v2;
		int v3;
	public:
		Triple(int, int, int);
};
```
triple.cpp:
```
Triple::Triple(int p1 = 0, int p2 = 0, int p3 = 0)
{
	v1 = p1;
	v2 = p2;
	v3 = p3;
	
}
// Does the same thing as:
Triple::Triple(int p1 = 0, int p2 = 0, int p3 = 0): v1(p1), v2(p2), v3(p3);
```

**Destructor:**
The destructor is called whenever an object goes out of scope or is deleted from the heap.
It is used to free any resources used by the object, this means: 
- freeing any heap allocated memory used
- closing files used
- network resources used (e.g. sockets)
Note that c++ automatically supplies a default destructor if you do not define one.
The default behavior is to apply (call) the destructor in each (non primitive) data member (member variables) WILL NOT CALL THROUGH POINTER.
- If the variable is a pointer, it will not call the destructor on what it points to.

**Copy Constructor**:
A special constructor is used to construct a new object of the same type.
The copy constructor is called in 3 different circumstances:
1. Box copy = original; or Box copy(original).
2. When an object is passed by value to a method or function.
3. When an object is returned from a method or function.
Note that 2 and 3 are NOT a reference and NOT a pointer

Default behavior is to apply it to each member variable. Primitives are applied by value.

**Operator =**
The copy assignment operator (operator =) is applied to two constructed objects when an assignment is made.
```
copy = original
int main() {
	Box copy = *(new Box(1));
	copy = *(new Box(2));
}
```
Signatures for these functions.
Destructor: `~Box()`
Copy Constructor: `Box(const Box &rhs)`
Operator = : `Box b = Box a;`

Example: intbox
intbox.h
```
#pragma once
class IntBox {
	private: 
		int *storedValue;
	public:
		intBox(int);
		IntBox(const IntBox &);
		~IntBox();
		const IntBox & operator = (Const IntBox &);
		int value();
}
```

IntBox.cpp
```
#include "intbox.h"
IntBox::IntBox(int val = 0) {
	storedValue = new int(intvalue);
}
IntBox::IntBox(constIntbox & rhs) {
	storedValue = new int( *(rhs.storedValue) );
}
IntBox::~IntBox() {
	delete(storedValue);
}
constIntBox &IntBox :: operator=(const IntBox &rhs) {
	if (this != &rhs) {
		*storedValue = *(rhs.storedVale);
	}
	return *this;
}
```

## IN C++ delete() == free(), new() == malloc()

Queue Example / Singly Linked List:

queue.h 
```
#pragma once
# ifndef NULL
# define NULL 0
# endif

struct ListNode {
	public:
		int element;
		ListNode * next;
		ListNode(int theElement, ListNode *n = NULL):
			element(theElement), next(n) {}
};

class UnderflowException {
};

class IntQueue {
	private:
		int size;
		ListNode *front;
		ListNode *back;
		void makeEmpty();
	public:
		IntQueue();
		~IntQueue();
		const IntQueue & operator = ( const &IntQueue&);
		IntQueue( const IntQueue& );
		bool isEmpty();
		int getFront();
		void enqueue(int);
		int dequeue();
		void printQueue();
}
```

queue.cpp
```
#include "queue.h"
IntQueue::IntQueue(): front(NULL), back(NULL), size(0) {}
IntQueue::~IntQueue() {
	makeEmpty();
}
void IntQueue::makeEmpty() {
	while(!isEmpty()) {
		dequeue();
	}
}

const IntQueue & IntQueue::operator=(const IntQueue &rhs) {
	if (this != &rhs) {
		makeEmpty();
		ListNode *rptr = rhs.front;
		while (rptr != NULL) {
			enqueue(rptr -> element);
			rptr = rptr -> next;
		}
	}
	return *this;
}

inline bool IntQueue::isEmpty() { return size == 0; }

int IntQueue::getFront() {
	if (isEmpty()) throw new UnderflowExpection();
	return fornt -> element;
}

void IntQueue::enqueue(int x) {
	if (isEmpty()) {
		back = front = new ListNode(x);
	}
	else {
		back = back -> next = new ListNode();
	}
	size += 1;
}

int IntQueue::dequeue() {
	int frontItem = getFront();
	ListNode *old = front;
	front = front -> next;
	if (size == 1) { back = NULL; }
	delete old;
	size -= 1;
	return frontItem;
}
```

main.cpp
```
#include <iostream>
#include "queue.h"
int main() {
	IntQueue q;
	IntQueue r;
	IntQueue *p = new IntQueue();
	q.enqueue(1);
	q.enqueue(2);
	q.enqueue(3);
	q.enqueue(4);
	r = q;
	r.dequeue();
	IntQueue z(r);
	Intqueue z1 = r;
	*p = r;
}
```