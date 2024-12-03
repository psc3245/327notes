
### Arrays on the Heap
```
int *p = new int[10]; // can be variable size
delete []p;
```

File Input Streams:
```
#include <iostream>
#include <fstream>
int main() {
	ofstream filetowrite;
	filetowrite.open("path"); // path is path to filename
	filetowrite << "Hello file \n";
	filetowrite.close();
	return 0;
}
```

## Nested Classes (inner classes)
- Similar to Java inner classes
- The semantics are similar to Java static inner classes
- Unlike Java, private member variables in the nested (inner) class are **not** visible to enclosing class

### Inheritance in C++
Java example first:
```
public class A {
	public A() {
		System.out.println("In A constructor");
		init();
	}
	public void init() {
		System.out.println("In A init");
	}
	
}

public class B extends A {
	public B() {
		System.out.println("In B constructor");
	}
	public void init() {
		super.init();
		System.out.println("In B init");
	}
}

public class main() {
	public static void main(String[] args) {
		B b = new B();
	}
}
```
Prints:
```
In A constructor
In A init
In B init
In B constructor
```

**There is no polymorphism in C++ constructors**

```
class A {
	public:
		A() {
			cout << "In A constructor \n";
			init();
		}
		virtual void init() {
			cout << "In A init \n";
		}
};

class B: public A {
	public:
		B() {
			cout << "In B constructor \n"
		}
		void init() {
			A::init();
			cout << "In B init \n";
		}
};

int main() {
	B *b = new B();
}
```
Prints:
```
In A constructor
In A init
In B constructor
```

Another Example:
```
class A {
	public:
		A():dataOne(2) {}
		virtual void whoami() {
			cout << "Class A \n";
		}
	protected:
		int dataOne
};

class B: public A {
	public:
		B(): dataTwo(9) {}
		virtual void whoami() {
			cout << "Class B \n";
		}
		private:
			int dataTwo();
}
```

#### Slicing:
```
A objectA;
B objectB;
obectA = objectB;
objectA.whoami(); // outputs: class A
```

```
A *objectA = new A();
B *objectB = new B();
objectA = objectB;
objectA-> whoami();
*objectA = *objectB; // slicing happens here
```

```
B objectB;
A &refA = objectB;
refA.whoai(); // outputs class B
```

#### Slicing can be non obvious:
```
class Ick {
	public:
		A foo(A*) {
			return an A type
		};
}
```

#### Calling constructors in super class:
**Java:**
```
public class Box {
	private int val;
	public Box(int v) {
		val = v;
	}
}

public class BigBox extends Box {
	private double dval;
	public BigBox(double d, int v) {
		super(v);
		dval = d;
	}
}
```
**C++**
```
class Box {
	private:
		int val;
	public:
		Box(int v):val(v){}
};

class BigBox: public Box {
	private:
		double dval;
	public:
		BigBox(double d, int v):
			Box(v), dval(d) {}
			// must be initializer, initializer must be first
}
```

#### Combining Constructors
**Java**
```
public class A {
	public A(int i) {
		...
	}
	public A(int i, int j) {
		this(i);
		...
	}
}
```
**C++**
```
class A {
	public:
		A(int i){
			...
		}
		A(int i, int j) {
			A(i);
			...
		}
}
```

#### Visibility Modifiers for inheritance:
- Public
- Private
- Protected
- Java has package - **not** in c++

```
class Parent {
	public:
		virtual void test() {
			cout << "Int parent \n";
		}
};

class Child: public parent {
	private:
		void test() {
			cout << "In child \n";
		}
};

int main() {
	child *c = new Child();*
	c -> test(); // syntax error

	Parent *p = new Child();
	p -> test(); // outputs: In child
}
```

### Types of Inheritance
`class A: x B`
i.e. A is derived and B is parent

|                 | A Public                   | A Protected                | A Private                |
| --------------- | -------------------------- | -------------------------- | ------------------------ |
| **B Public**    | public in derived class    | protected in derived class | private in derived class |
| **B Protected** | protected in derived class | protected in derived class | private in derived class |
| **B Private**   | n/a                        | n/a                        | n/a                      |
**HAS - A** (composition) - a car HAS - A engine, implemented as a member variable of the class
**IS - A** (inheritance) - a rectangle IS - A polygon
**AS - A** - List AS - A Queue


#### Polymorphism in Java
```
abstract class Animal {
	abstract public void speak();
}

public class Bird extends Animal {
	public void speak() {
		System.out.println("Hello - squak");
	}
}

public class Mammal extends Animal {
	public void speak() {
		System.out.println("Can't Speak");
	}
	public void bark() {
		System.out.println("Can't bark");
	}
}

public class Cat extends Mammal {
	public void speak() {
		System.out.println("Howdy");
	}
	public void purr() {
		System.out.println("purr");
	}
}

public class Dog extends Mammal {
	public void speak() {
		System.out.println("woooof");
	}
	public void bark() {
		System.out.println("WOOOOF");
	}
}

Animal a = new Dog(); // legal
a.bark(); // Illegal syntax error
a.speak(); // outputs woof
Mammal m = (Dog) a;
m.bark(); // outputs WOOOOOF
```

#### Polymorphism in C++
**You Cannot make a non-virtual function/method virtual in a subclass.**

AI: "In C++, a virtual method (also called a virtual function) is a member function in a base class that you can redefine in a derived class."

```
class Animal {
	public:
		virtual void speak() = 0;
};

class Bird : public Animal {
	public:
		virtual void speak() { // virtual keyword is redundant
			cout << "Hello - squak \n";
		}
}

class Mammal : public Animal {
	public:
		virtual void speak() {
			cout << "No Speak \n";
		}
		virtual void bark() {
			cout << "No Bark \n";
		}
}

class Dog: public Mammal {
	public:
		virtual void speak() {
			cout << "Dog - speak \n";
		}
		void bark() {
			cout << "Dog bark";
		}
}

Dog *d = new Dog();
d -> bark(); // outpus Dog bark
Mammal *m = d;
m -> bark(); // outputs Dog bark
d -> speak() // outputs Dog speak
m -> peak() // outpus Dog speak
Animal *a = d;
a -> speak(); // outpus Dog speak
a -> bark(); // compile error
```