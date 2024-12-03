### C Language Summary
- Types:
	- char - 8 bits
	- short (short int) - 16 bits
	- int - 32 bits
	- long (long int) - 64 bits
	- float - 32 bits
	- double - 64 bits
	- void
- Integer types (including char) can be preceded  by "unsigned" and "signed" bits
	- default is signed
- No boolean and no string
	- boolean uses integer (0 == false, anything else == true)

User Defined Types
- `struct`
- Used to collect data together
- NO METHODS IN STRUCTS 
- Everything is public
```
struct date {
	short year;
	short month;
	short day;
};
```
- the name of a struct is the "tag"
- REMEMBER THE SEMICOLON AFTER THE CURLY BRACES
- This does not define the variable, just lets you make one using the tag
- You can use the tag name to declare a struct variable
```
struct date startDate;
```
- Structures can be nested
- You can also define aliases for types using "typedef"
	- This allows for us to have more descriptive names for types
		- Can shorten type name
```
typedef int length_in_inches;
typedef int length_in_cm;
typedef struct date date_t;
```
`typedef` allows you to replace given type with given name 
`length_in_inches height;`
This is valid and would give us variable height of type int

#### Pointers as a Type (Summary)
In C we have pointer variables and types.
The variable (or type) is the **address in memory where something is stored**
To create/declare a pointer, you simply add a \* to the end of the type
`char*` is the pointer to a char
`double**` is a pointer to a pointer to a double
32 bits in memory
`double** bar;` is a pointer to a pointer to a double, and name all of it bar

`printf("%d\n", *p);` -> will print out whatever is at the address p contains
`int* p; == int *p;`

#### Arrays
- you can declared fixed sized arrays as
	- `int A[10];` - declares A as an array of 10 values
- You can declare and array of pointers
	- `int *A[20];` - declares A as an array of 20 pointers

#### Bitwise Operators
- `~` one's compliment
- `&` bitwise and
- `|` bitwise or
- `^` bitwise xor
- `<<` left shift
- `>>` right shift

`unsigned char x = 0x96;`
- `0x` before an integer means hex

#### Conditional Operators:
- `a ? b : c;`
	- if a is true then b else c
- Ex. Write the max function with a ternary operator
	- `x > y ? x : y;`

#### Local and Global Variables
- Local variables - declared inside functions (with scope) and can only be used in side that function
	- pretty much like java
- Local static variables - a variables that is declared "static" in a function or block it is still scoped to the function but it maintains its value between function calls
- Global variables - declared outside of any function (usually at the beginning of the file) and can be used in all functions
	- use `extern` keyword to use in other files

#### Pointers and Function Parameters
Clarification:
- `int *a;` = variable of type address of int
- `int temp = *a;` = set temp to the value at address a
- `temp = &a;` = set temp to address of a
	- & = address of
Below is pass by reference!
```
void swap(int *a, int *b) {
	int temp = *a;
	*a = *b;
	*b = temp;
}

int main() {
	int v1 = 5;
	int v2 = 20;
	printf("%d %d\n", v1, v2);
	swap(&v1, &v2);
}
```

#### Prototypes and Header Files
Recall that in source files we have to declare functions prior to their use in the file.
To rectify this, we use prototypes.

```
#include <stdio.h>
int maxint(int, int);

int main() {
	int a = 2;
	int b = 3;
	int x = maxint(a, b);
	printf("%d \n", c);
}

int maxint(int x, int y) {
	return (x >= y) ? x : y;
}
```
Without the prototype of `int maxint(int, int);`, this would cause an error because `maxint()` has not yet been defined.

Because we may need prototypes for many functions and these functions may be called from function in different files, it makes sense to extract the prototypes out and make them part of a **header** file.

Ex. 
file1.c has code for:
```
int func1a(int int);
void func1b(int);
```
and calls:
`func2a, func2b, func2c`

file2.c has code for:
```
void func2a(int);
int func2b(int);
int func2c(char);
```
and calls:
`func1a`

So in file1.h
```
int func1a(int, int);
void func1b(int);
...
other stuff
```

Then in file1.c, use:
```
#include <stdio.h>
#include "file1.h"
#include "file2.h"
```
- `#include <name>` - > not a header file made by you, has to be found somewhere
- `#include "name"` - > header file on my machine

How do we avoid including the same file more than once?

```
#ifndef FILE1_H
#define FILE1_H
```
This says if `FILE1_H` is not defined, define it according to this file. Otherwise, do not define it as it is already defined for files to use.
`#pragma once` is the same thing - can use them together(?)

#### Arrays and Pointers

```
#include <stdio.h>
int main() {
	char v[20];
	v[0] = 'a';
	v[1] = 'b';
	v[2] = 'c';
	printf("%s \n", v);
	return 0;
}
```
No pointer needed for arrays - if you use an array variable, it assumes its a pointer;
`%s` is to print arrays in printf

The code above won't stop printing the array until it finds a "zero" character.
```	
v[3] = 0;
v[3] = '\0';
```
Both of these would work for a zero character. Not the character '0', but the value of zero because chars are integers.

```
int main() {
	char t[4];
	char *p;
	char **q;
	t[0] = 'x';
	t[1] = 'y';
	t[2] = 'z';
	t[3] = '\0';
	p = &t[0];
	printf("%s \n", p);
	printf("%s \n", t);
	*p++ = t[0];
	*p++ = '\0';
	printf("%s\n", p);
	q = &p;
	printf("%s\n", *q++);
}
```
What is the output of this code?
```
xyz
xyz

```