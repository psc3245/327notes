
#### Unions
- A union is useful if:
	1. You want to access data in two different ways
	2. You want to store different data depending (or not depending) on type in the same space
Syntax:
```
union datadef {
	int a;
	floate v;
	char *s;
	char ch;
};
```
Overlap all variables in the same space
Allocate space to fit the largest type using `sizeof`

Suppose we declare the variable:
`union datadef d;`