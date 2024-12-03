### UML - Unified Modeling Language
Thousands of pages on how to specify design

Jim teaches a beginning class in programming and has an idea. Jim noticed that students learn better when students interact and ask questions. Jim decides to write a simulation to test this theory.

Jim makes SimU Student, a program that simulates students in the classroom. 

The simulation works really well. Jim has a research idea:
	Suppose we simulate having a robot in the classroom that acts like a student that asks questions.

Class Student: `talk(), write(), raisehand()`
Subclasses of student:
- TalkativeStudent: `talk(), write(), raisehand()`
- SleepyStudent: `talk(), write(), raisehand()`
- RobotStudent: `talk(), write(), eatpizza()`

To draw a class diagram for this, make a box for the classes and add their class name and methods.
The **parent** class should be above the **children** classes, and the **children** classes should have an arrow pointing to the **parent** class.

![[Screenshot 2024-12-02 at 12.01.43 PM.png]]

The simulation seems to work really well, and so Jim take the simulation to a conference to show it off. When Jim runs the simulation program at the conference, the robot suddenly has a slice of pizza floating in front of it's face.
What happened?
The night before, a well-meaning grad student added eat pizza to student class and only updated TalkativeStudent and SleepyStudent.

How can this be fixed easily?
Add eatPizza method to RobotStudent that does nothing.

The program (simulation) was a great success and lots of requests for changes come in.

It is hard to remember to override new methods in all the subclasses, even if it does nothing.

Using inheritance doesn't work well since student behaviors keep changing across subclasses. 
Adding a behavior is complicated and reuse is not easy using interfaces.

#### Design Principle:
**Identify the aspects of your applications that vary and separate them from what stays the same.**

This can be applied here by separating the student classes away from inheritance. One generic student class, then an **Interface** called TalkBehavior, with subclasses Talkative, NoTalk, and Quiet.

Student class contains an instance of interface TalkBehavior, then uses it to do method `PerformTalk()`.

New Class Diagram:
![[Screenshot 2024-12-02 at 12.11.47 PM.png]]

Java Example of this:
```
public class Student {
	protected TalkBehavior tb;
	public Student() { 
		tb = new NoTalk();
	}
	public void doTalk() {
		tb.talk();
	}
}

public class TalkativeStudent extends Student {
	public TalkativeStudent() {
		tb = new Talkative();
	}
}
```

### Design Principles:
- **Favor Composition over Inheritance**
- **Program to an Interface, not an Implementation**

The observer pattern defines a one-to-many dependency between objects so that when one object changes state, all of its dependents are notified and updated automatically.

![[Screenshot 2024-12-02 at 12.20.29 PM.png]]

Observable.h
```
#pragma once
#include <vector>
#include "Observer.h"
#include "ObservableData.h"

using namespace std;

class Obeservable {
	protected:
		vector<Observer*> observers;
	public:
		void registerObserver(Observer *o);
		void removeObserver(Observer *o);
		virtual void notifyObservers(ObservableData*) = 0;
}:
```

Observable.cpp
```
#include "Observable.h"
#include <algorithm>

using namespace std;

void Observable::registerObserver(Observer *o) {
	for (Observer *p : observers) {
		if (p == o) return;
	}
	observers.push-back(o);
}

void Observable::removeObserver(Observer *o) {
	for(int i = 0; i < observers.size(); i++) {
		if (observers[i] == o) {
			observers.erase(obserers.begin() + i);
			return;
		}	
	}
};
```

ConcreteObservable.h
```
#pragma once
#include "Observeable.h"

class ConcreteObservable::public observable {
	virtual void notifyObservers(void *);
};
```

ConcreteObservable.cpp
```
#include "ConcreteObserable.h"
#include <vector>

ConcreteObservable::notifyObservers(int *d) {
	for (int i = 0; i < observers.size(); i++) {
		observers[i] -> update( new int(d) );
	}
}
```

ObserverPattern.h
```
#pragma once
#include <vector>
#include <algorithm>

using namespace std;

template <class T>
class ObservableData {
	public:
		virtual T getData() = 0;
};

template <class T>
class Observer {
	public:
		virtual void update(ObservableData<T> *) = 0
};

template <class T>
class Observable {
	protected:
		vector<Observer<T>*> observers;
	pubic:
		void registerObserver( Observer<T> *o) {
			// same as other but...
			if (find (observers.begin(), observers.end(), o) 
				!= observers.end() ) {
				observers.push-back(o);	
			}
		}
		void removeObserver(Observer<T> *o) {
			// similar
		}
		virtual void notifyObservers(ObservableData<T> * ) = 0;
};

class TestObservable:public Observable<int> {
	void notifyObservers(ObservableData<int> *d) {
		for (Observer<int> *o : observers) {
			Data *d1 = new Data(d.getdata()); // add constructor to data
			o -> update(d1);
		}
	}
}
```

```
#include "ObserverPattern.h"

class Data: public ObservableData<int> {
	public:
		int theData;
		int getData() override { // "override" keyword is optional
			return theData;
		}
}
```


#### Operator Overloading

```
class Box {
	private:
		int value:
	public:
		Box(int v):value(v) {}
		bool operator < (const Box &rhs) {
			return value < rhs.value;
		}
};

```