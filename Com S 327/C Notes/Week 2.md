Why Learn C/C++:
1. Jobs - there are still numerous jobs that require C/C++ expertise
	1. Legacy Code
	2. C/C++ is still the best language in some situations
2. Today some applications are still written in C
	1. Linux Kernel
	2. Device Drivers
3. Techniques are still useful
	1. There are concepts in C/C++ that cannot be taught in other languages like Java

C History, Philosophy, Features
- 1969, Ken Thompson wrote the first Unix System
	- In Assembly language on a DEC PDP-7 computer
- Thompson wanted a language for system programming
	- Started with BCPL (existing language)
	- Stripped it down to get rid of junk and called it B
	- Caused problems because it was a typeless language
- Dennis Ritchie joined the project added types and other stuff and called it C.
	- Used C to implement the Unix Kernel

#### C Philosophy
At the time computers were not very powerful, so programmers were very concerned with efficiency (Both CPU and Memory)
Overall C philosophy: No feature should be added to the language if it causes a speed or memory penalty for programmers that do not use that feature.

At the time, C was a very modern programming language. 
- Flow Control (branching and loops)
- User defined procedures (functions)
- User defined data structures (struct union)
- Economy of expression
- Easy to compile

##### Bottom line:
In C, the programmer is always right.
- Very little error checking - its a penalty on runtime

```
int x[10];
int y[50];
x[42] = 101;
```
What happens in C?
1. The program runs fine - no visible errors
2. The program does something strange and you can't figure out why
3. Segmentation fault

Which of the three outcomes may depend on on:
- input to the program
- which compiler you use (including version of compilers)
- which system you run it on
- when (what time) you run it at

Almost anything! That sucks.

#### Comparing C and Java
- Operators and syntax are similar, but beware of subtle differences in both syntax and semantics.
- Both store programs in a text file and code can be divided between files.
- Java converts to "byte codes" and executed on a virtual machine
- C compiles to native machine code
- Java is Object Oriented, C is NOT

Java Features you should forget about in C
- Type safety
- Bounds checking
- Garbage collecting

C programs are like Java programs where everything is in one class and declared static

# C
```
/* a simple comment */
// also a comment
#include <stdio.h>
int main() {
	printf("hello World");
	return 0;
}
```

Header files, or library files, allow the compiler to have access to some functions it may not have access to otherwise

##### printf
You can put a "%\_" in your printf (formatted print) string to insert values without concatenating
- %s = string
- %d = integer (decimal)
- %x = integer (hex)
- %c = char
- %f = floating point

#### Compiling C Source Code
c source (hello.c) $\rightarrow$ compiler (pre processor, adds library) $\rightarrow$ Object files $\rightarrow$ linker $\rightarrow$ executable (a.out)

# & = Address of operator

#### Compilation:
Suppose we have green.c, blue.c and common.h header file for both of them.

First, compile both into object files green.o and blue.o.

`> gcc -c green.c` = compile but don't link, produces green.o

`> gcc green.o blue.o` = link these two files and make an a.out executable

`> gcc green.c blue.c` = same as above, link two files and make a.out

`> ./a.out` = run the executable

#### Splitting your C program into separate files
- No two files can have functions with the same name
- Also true for global variables
- Use a header (.h) file to make functions and variables visible to code in other files
- Unless creating a library, at least one file must contain a main function

#### Make Files
Dependency and Dependency Graphs
		        Project 1
		/                |                \\
	data.o         main.o         io.o
	/       \\      /      |       \\      /    \\
data.c  data.h   main.c    io.h.       io.c

Make files are a simple way to make sure source files are compiled and linked in the right order.
More importantly, they tell you what has to be remade if only a few changes are made in some but not all files.

Makefile
```
project1: data.o main.o io.o
	gcc -g data.o main.o io.o -o project1
data.o: data.c data.h
	gcc -c -g data.c
main.o: data.h main.c io.h
	gcc -c -g main.c
io.o: io.h io.c
	gcc -c -g io.c
clean:
	rm -f main.o io.o data.o project1
```
First, say what a file (or target) should be made of, ex. project1 (file/target) : data.o main.o io.o (made of)
Then put a tab and write the command to tell the computer what to do if any files change - how should we re compile?

`-g` flag in gcc call will add debug information to the file
`-o` tells what the output should be named
`-f` means use file as make file (seen in command below)

To run, type `>make -f <filename>`

`clean` = `rm -f *.o program1` - this will remove all .o files from program 1
(`-f` means force here)
`clean` removes the clutter, (as seen in `./mvnw clean` for spring boot) 
	"clutter" means compiled files or any other that aren't necessary to run the code
