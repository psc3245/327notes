# Practice for test:
- Just make all data structures in C
	- including their functions

// Node struct
```
#pragma once
struct node
{
	int data;
	struct node *next;
}
```
// completed push function
```
void push(Struct node **s, int d) {
	struct node *toAdd = malloc(sizeof(struct node));
	toAdd -> data = d;
	toAdd -> next = *s;
	*s = toAdd;
}
```

```
int pop(Struct node **s) {
	int toreturn;
	
	if (*s == null) { // if (!*s)
		fprintf(stderr, "Stack underflow \n");
		return 0; // not good but we're doing it anyway
	}
	
	toreturn = (*s) -> data;
	
	struct node *temp = *s; // store *s so it can be freed
	
	*s = (*s) -> next;
	
	free(temp);
	
	return toreturn;
}
```

```
main() {
	struct node *top = NULL;
	push(&top, 1);
	.
	.
	.
	q = pop(&top);
}
```

# Doubly Linked List
Circularly linked list with a dummy node

##### dll.h
```
#ifndef DLL_H
#define DLL_H

typedef struct node_t NODE;
typedef struct node_t *NODEPTR;

struct node_t {
	void *data;
	NODEPTR next;
	NODEPTR prev;
};

typedef struct list_t LIST;
typedef struct list_t *LISTPTR;
struct list_t {
	NODEPTR head;
	int numItems;
};

LISTPTR createList();

int add(LISTPRT lst, void *item); // add to the end

void *get(LISTPTR lst, int index);

void *remove(LISTPRT lst, int index);

int empty(LISTPTR lst);

void clear (LISTPTR lst); //. remove all elements

void deleteList(LISTPTR lst);

#endif
```

##### dll.c
```
int add(LISTPTR lst, void *item) {
	// Allocate memory for a node
	NODEPTR p = malloc(sizeof(NODE));
	// Check if the memory actually got allocated
	if (p == NULL) return 0;

	// Now do linked list shit
	// Set the data to the given item
	p -> data = item;

	// Change p's prev to the last node (since its added at the end)
	p -> prev = lst -> head -> prev;
	// Change new node's prev to head (for DLL circular)
	p -> next = lst -> head;

	// Set the list's head's prev's next to p
	// What this means is find the old last node and make its next point to p
	lst -> head -> prev -> next = p;
	// Set list's head's prev (the pointer to last node) to p
	lst -> head -> prev = p;
	
	lst -> size ++;
	return 1;
}
```

```
void deleteList(LISTPTR lst) {
	NODEPTR p = lst -> head;
	// Add one because of the dummy node
	for (int i = 0; i < lst -> numItems + 1; i++) {
		// store the pointer to next
		NODEPTR pn = p -> next;
		// Free p's memory
		free(p);
		// Set p to the next we stored
		p = pn;
	}
	free(lst);
	// Now the list is gone
}
```

```
void *get(LISTPTR lst, int index) {
	if (index >= lst -> numItems || index < 0) return NULL;
	NODEPTR p;
	for (int i = 0, p = lst -> head -> next; 
		i < index; i++, p = p-> next);
	return p->data;
	
}
```

```
void * remove(LISTPTR lst, int index) {
	
}
```

### Standard I/O Again
`fprintf` and `fscanf` are like `printf` and `scanf` except the first parameter is a `FILE*` (file pointer).
```
fprintf(fileptr, format string, ...)
fscanf(fileptr, format string, ...)
```
File pointers can be `stdin`, `stdout` and `stderr`

We can use these to read and write files.

```
FILE *outfile;
outfile = fopen("<filename>", "w");
```
`fopen` returns a pointer to a `FILE` structure that describes the file in OS terms
Returns NULL if the file cannot be opened.
```
if (!outfile) {
	fprintf(stderr, "Could not open file \n");
	exit(-1);
}

fprintf(outfile, "Square table \n\n");
for (int i = 0; i < 10; i++) {
	fprintf(outfile, "%d, %d, \n", i, i*i);
}
fclose(outfile);
```
This will print to standard error if the file can't be opened, otherwise it will print to out file since we opened

Note that we can write code like this:
```
FILE *f;
...
f = stdout;
```
I/O to standard out is buffered - it will not print everything as it comes, it will wait to dump a bunch at once 
(stderr is not buffered)
You can flush the buffer with `fflush(FILE *);`

