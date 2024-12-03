
Suppose we want to copy a c:string to another? What is the shortest code to do this?
```
void strcopy(char *s, char *d) {
	while (*s != 0) {
		*d++ = *s++;
	}
	*d = '\0';
}
```
This code is the same as:
```
void strcpy(char s[], char d[]) {
	int i = 0;
	while (d[i] = s[i]) i++;
}
```

#### C Structures:
Suppose we wan to represent a point on a screen with x, y coordinates. These two numbers are related, and thus is makes sense to relate them when stored. 
One way to do this in C is to use Structures. They are similar to classes in Java, except these cannot have methods, inheritance, or privacy.

Two ways to declare
```
struct point 
{
	int x;
	int y;
};

typedef struct {
	int x;
	int y;
} point_t;
```
To use:
```
struct point p1; // or
point_t p2; // or
struct {
	int x;
	int y;
} p3;
```

Unlike Java classes, struct types are not references.
So
	`p1 = p2;` - copies all the data (fields) stored in p2 into p1. Any subsequent changes to p2 will NOT change p1.
You can initialize the values in a structure when they are declared:
`struct point maxpt = {320, 200};`

You can access values in a struct with a `.`
`int x = p1.x;`

Suppose we want to add two points together.
```
struct point addPoint(struct point p1, struct point p2) {
	struct point temp;
	temp.x = p1.x + p2.x;
	temp.y = p1.y + p2.y;
	return temp;
}
```

Pointers and Structures
`struct point *ppoint;`
`ppoint` is a variable that is the address of the beginning of a structure type.

`*ppoint` dereferences to a point struct type

Because pointers and structures are often used together, there is an alternate syntax:
`ppoint -> x` $\equiv$ `(*ppoint).x`

Two rules about structures:
1. The only legal operations on structures are:
	1. Copying (assignment, pass by value)
	2. Taking an address of
	3. Accessing members with `.` or `->`
2. Arrays of structures work just as you think
```
struct histbin {
	char *label;
	int count;
};
struct histbin histogram[100];
```

There is a function in C that gives you the size of types:
```
sizeof(int) // 4 bytes
sizeof(struct histbin) // 8 bytes
sizeof(struct *histbin) // 4 bytes
```

### Intro to Heap Memory
Stack memory - local var, ret addr, parameters
Heap memory - persistent while program is running

We will use `malloc`, function from `stdlib.h`, to allocate memory from the heap.
The signature (prototype) -> `void * malloc (int size)`

This function tries to allocate **size** bytes from the heap. If the function succeeds, it will return a pointer (address) of the first memory address allocated. 
If it is not successful, it return 0 (null).

Allocate an int onto the heap:
```
int *p = malloc(sizeof(int))
.
.
.
*p = 5;
p = malloc(sizeof(int));
*p = 7;
```

Functions can cause memory locals
```
void function() {
	int *p;
	p = malloc(sizeof(int));
	.
	.
	.
	return;
}
```

Structures and 
```
main() {
	int *p;
	struct point p1;
	p = maloc(sizeof(int));
	p1 = malloc(sizeof(struct point));
	p1 -> x = 0;
	p1 -> y = 0;
}
```

#### Macros:
Recall \#define foo cat
Everywhere foo is in the file, it is replaced by cat

`# define max(A, B) (A > B) ? A : B`

Suppose we want to create a stack with a linked-list using heap memory.
Suppose we want to store int data on the stack.

What should we put in the header file `stack.h` ?
```
#pragma once
struct node
{
	int data;
	struct node *next;
}
```


```
main() {
	struct node *top = NULL;
	push(&top, 1);
	.
	.
	.
	push(&top, 2);
}
```

```
void push(Struct node **s, int d) {
	struct node *toAdd = malloc(sizeof(struct node));
	toAdd -> data = d;
	toAdd -> next = *s;
	*s = toAdd;
}
```