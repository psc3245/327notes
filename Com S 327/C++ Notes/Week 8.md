
Hello World in C++:
```
#include <iostream>
using namespace std;

int main() {
	cout << "Hello world \n";
	return 0;
}
```

Other output examples
```
cout << "Something";
cout << 120
cout << x
cout << "Something" << 120 << x;
```

Input object:
```
// code fragment
int age;
cin >> age;

// now the program
#include <iostream>
using namespace std;
int main() {
	cout << "Enter age and zip code:";
	int age;
	int zip;
	cin >> age >> zip;
	return 0;
}
```

No more c strings!
```
#include <string>
#include <iostream>
using namespace std;
int main() {
	string mystring;
	cout << "What is your name";
	getline(cin, mystring);
}
```

References:
```
int i = 7;
int &j = i;
j++;
i += 3;
// i = 11 now
```
`j` cannot be reassigned after being declared a reference to `i`. In this instance `j` and `i` are the same variable.
`j` is an "alias", or reference to `i`.

Not only can you have variables that are references, you can pass parameters to methods/functions using references.
```
void passTest(int &i) {
	i++;
}
```
A reference can be used as the target of an assignment statement:
```
int v = 0;
int &r = v;
r = 6;
```
So assigning values to references seems obvious until you realize that a function/method can return a reference

```
int value[100];
.
.
int & index(int i) {
	return value[i+2];
}

int main() {
	index(27) = 12;
}
// value[29] = 12;
```

#### Objects:
Java syntax requires a single public class with the same file name. C++ doesn't care.

However, there are conventions, and please follow these in class.
- .h file: declarations, method prototypes, macros, sometimes inlined methods
- .cpp file: implementations of methods

Ignore convention for a second:
	What would a class look like?
```
class Box {
	private:
		int val;
	public:
		int myVeryBadIdea;
		Box(int v) {
			val = v;
		}
		int value() {
			return val;
		}
	protected:
		// none
};
```
Semi colon at the end is important
No access modifier for the class (eg public, private, protected)
May do multiple sections of each access modifier for each variable
	If no modifier, default is public
	But always have a modifier

Now a class using conventions:
Counter.h
```
#pragma once
class Counter {
	private:
		int value;
		int maxValue;
	public:
		void setValue(int);
		counter();
		counter();
		void increment();
		void decrement();
		int getValue() const;
		inline int getMaxValue const { return maxValue; }
};
```
`const` means that this function, `getValue()` in this case, cannot make changes to member variables
`inline` means that every time you see the function name, add the function body in. It adds space to the code and is not good practice for methods longer than 1 line. It is similar to the macros we learned already.

countertest.cpp
```
#include <iostream>
#include "counter.h"
using namespace std;

int main() {
	Counter c1;
	Counter c2(10);
	c1.setValue(50);
	c1.decrement();
	c2.increment();
	cout << c1.getValue() << c2.getValue() endl;
	c2 = c1;
}
```
`Counter c1;` $\equiv$ `Counter c1();` This means use the default constructor to make an object;

the line `c2 = c1;` is an **overloaded assignment copy operator** is a shallow copy, meaning the default primitive types are copied, but they remain different objects. 

Counter.cpp
```
#include <iostream>
#incldue <limits>
#incldue "Counter.h"

Counter::Counter() {
	value = 0;
	maxValue = INT_MAX;
}
Counter::Counter(int max) {
	value = 0;
	maxValue = mv;
}
void Counter::setValue(int val) {
	if ((val >= 0) && (val <= maxValue)) {
		value = val;
	}
	else {
		cerr << "Set value is out of range" << val << endl;
	}
}
void Counter::increment() {
	if (value < maxValue) {
		value++;
	}
	else {
		cerr < "Counter overflow" << endl;
	}
}
```

makefile:
```
countertest: Counter.o Countertest.o
	g++ -g -o countertest counter.o countertest.o
counter.o: counter.cpp counter.h
	g++ -g -c counter.cpp
countertest.o: countertest.cpp counter.h
	g++ -g -c countertest.cpp
```

### Const:
(The real story)
`const int c = 42;`
	c cannot be changed - it is a **constant**
	why? limits scope, compiler type checking, optimization
`const int *c1;`
	a variable pointer to a constant value
`int const *c2;`
	a variable pointer to a constant value
`int * const c3;`
	a constant pointer to a variable integer
`int const * const c4;`
	a constant pointer to a constant int

Rule of thumb for const
	const keyword applies to whatever is on the left of it, unless there is nothing on the left, in which case use whats on the right.


