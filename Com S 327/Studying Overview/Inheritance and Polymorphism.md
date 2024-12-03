#### Types of Inheritance
- Public Inheritance
	- Derived class inherits ALL public and protected members of the base class
	- Maintains "is a" relationship
- Protected Inheritance
	- Public and protected members of the base class become protected in the derived class
	- Limits access to the inherited members to derived classes and friends
- Private inheritance
	- ALL members of the base class (public and protected) become private in the derived class
	- Often used for "has a" relationships rather than "is a"

#### Polymorphism
- References and pointers
	- Polymorphism works via references or pointers to the base class type
```
class Base {
public:
    virtual void display() { std::cout << "Base"; }
};
class Derived : public Base {
public:
    void display() override { std::cout << "Derived"; }
};

// in main
Base* ptr = new Derived();
ptr->display(); // Outputs "Derived"
```

- Object slicing
	- Occurs when a derived class object is assigned to a base class object
	- the derived part of the object is "sliced" off
		- This leaves only access to the base class' variables and methods
```
Derived d;
Base b = d;  // Slicing: only Base part is copied
b.display(); // Calls Base's version of display
```

#### Virtual Functions
- methods declared in base class with keyword `virtual`
- Allows the method to be overridden in derived class
- If a base class function is not virtual, the derived class version **won't** be called when accessed through a base class pointer

An Interface can be simulated with only pure virtual functions
```
class Interface {
public:
    virtual void function() = 0; // Pure virtual
};
```
A pure virtual function is an abstract function, it is ONLY implemented in derived class and is simply a prototype in the base class

#### Calling the Base Class Method
Use `BaseClassName::method()` in front of method to act as `super.method()`
```
class Derived : public Base {
public:
    void function() {
        Base::function(); // Calls the base class version
    }
};
```

#### Polymorphism
- Enables behavior based on the actual derived type rather than the base type reference/pointer
	- Use `virtual` functions and base class pointers/references to achieve polymorphism