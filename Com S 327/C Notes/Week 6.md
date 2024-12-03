#### Command Line Arguments
```
#include <stdio.h>
int main(int argc, char *argv[]) {
	
}
```
`argc` is the number of parameters (including the program name that was run)
> ./a.out param1 param2 param3

`argc` = 4
`argv` = the parameters in an array
	note that `char* argv[]` $\equiv$ `char **argv`
`argv[0]` = `./a.out`
`argv[1]` = param1
`argv[2]` = param2
`argv[3]` = param3

Example:
```
#include <stdio.h>
int main(int argc, char *argv[]) {
	FILE *readfile;
	if (argc > 1) {
		readfile = fopen(argv[1], "r");
		if (!readfile) {
			fprintf(stderr, "No such file \n");
			exit(-1);
		}
	}
	else {
		readfile = stdin;
	}
	float input1;
	float input2;
	
	int r = fscanf(readfile, "%f, %f", &input1, &input2);
}
```

You can have pointer variables that point to code.
The C library has a quick sort function that performs a quick sort an array of data.

How can we make it so that we can use the same code on integer data, double data, or even struct data?
Just like Java, we make a "generic" comparator.
We need the client to write a compare function the specific type, and somehow, q-sort knows to call their function.
Answer: pointers to functions;

Consider functions that perform a computation:
```
eval.c
	float plus(float a, float b) { return a + b; }
	float minus(float a, float b) { return a - b; }
	float divide(float a, float b) { return a / b; }
	float times(float a, float b) { return a * b; }
```

```
float evaluate(float a, float b, char op) {
	float result;
	switch (op) {
		case '+':
			result = plus(a, b);
			break;
		case '-':
			result = minus(a, b);
			break;
		...
		...
	}
	return result;
}
```
This is bad code. Every time we change eval.c, we have to go in and update the evaluate method.

We can use pointers to functions to simplify this code.
In a main function that uses evaluate, we can declare variables that are pointers to functions that perform our compute functions. 

So what should this look like?

```
main() {
	float (*func) (float, float);
	<return type> (<variable name>) (<function parameters>);
}
```
This declares a variable named "func" to be a pointer to a function that returns a float and takes two float arguments.
```
func = &plus;
```

We can call an updated evaluate function (still needs to be updated).
`float x = evaluated(2.0, 3.0, &add);`
This works - the third parameter becomes a pointer to the function add
`float z = evaulate(2.0, 3.0, *func);`
This works- dereference whatever is in the pointer `func` to use the function

New eval function:
```
float evaluate(float a, float b, float(*opfunc)(float, float) ) {
	return *opfunc(a, b);
}
```
We must specify the return type and parameters of the function within the parameters of the function calling it.

If you had to write a prototype for evaluate:
- `float evaluate(float a, float b, float(*func)(float, float));`
- `float evaluate(float, float, float(*)(float, float));`


Back to quick sort:
	`void qsort(void *base, size_t nmemb, size_t size, 
		`int (*compare) (const *void, const *void) );`

Function to compare integers for q sort:
```
int intemp(int *a, int *b) {
	return *a - *b;
}
```
`if a > b` return a positive number
`if b > a` return a negative number
`if a == b` return 0
This is the archetype that all comparison functions use in c.

