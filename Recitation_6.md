# Recitation 6 #

## Malloc ##
Sometimes you want a bit more than a local variable, but less than a static variable.  
Sometimes, you want a dynamic AND persistant array.  Who you gonna call? -> Heap allocation.
How you gonna call it? -> with malloc().
A variable allocated on the heap will stay there until free() is called.  Wow!
Example:
```c
#include <stdio.h> // look!  a new library for malloc and free!
#include <stdlib.h>

int *allocateIntArray()
{
  int *p = (int *)malloc(3 * sizeof(int));
  return p;
}

void callNumber(int *p, int n)
{
  printf("Calling ");
  int i;
  for (i = 0; i < 3; i++)
  {
    printf("%d", *(p + i));
  }
  printf("\n");
}

void freeIntArray(int *p)
{
  free(p);
}

int main(int argc, char **argv)
{
  int *emergency = allocateIntArray();
  *(emergency) = 9;
  *(emergency + 1) = 1;
  *(emergency + 2) = 1;
  callNumber(emergency, 3);
  freeIntArray(emergency);
  return 0;
}
```
### Memory Leaks ###
When you run valgrind on your programs, you will have memory errors if you allocated memory on the heap that wasn't freed.
Don't forget to free()!  Memory errors will lose you points on labs!

## Structs ##
### What's a struct? ###
A struct is a collection of variables.  Some may say "it's like a class in C".
Actually, there were structs well before there were classes in C++ and Java.  A class is an expansion of a struct.
One of the key differences between a struct and a class is that the variables in a struct are inherently public while the variables in a class are inherently private.
But you don't have to worry about classes for the time being, cause we're still in C.

### Struct declaration ###
A struct can be declared in a few ways:
```c
// way 1
struct Dog {
	char *name;
};
struct Dog d1;

// way 2
struct {
	char *name;
} d1;

// way 3
typdef struct {
	char * name;
} Dog;
Dog d1;
```
### Contact Example ###
Here, we create a struct for a contact that might appear in your phone.
```c
#include <stdio.h>
#include <stdlib.h> // for malloc

struct Contact {
	char *first;
	char *last;
	int number;
};

/* Met a cute girl at Mel's, gotta add her */
struct Contact *createContact(char *_first, char *_last, int _number)
{
	struct Contact *new = (struct Contact *)malloc(sizeof(struct Contact));
	if (new == NULL)
	{
		printf("malloc failed");
		return NULL;
	}
	new -> first = _first;
	new -> last = _last;
	new -> number = _number;
	return new;
}

/* And now she's making out with my friend, forget her */
void deleteContact(struct Contact *c)
{
	free(c);
}

void printContact(struct Contact *c)
{
	printf("%s %s's number is %d\n", c -> first, c -> last, c -> number);
}

int main(int argc, char **argv)
{
	struct Contact *c1 = createContact("Don", "Yu", 1234567890);
	printContact(c1);
	deleteContact(c1);
	return 0;
}
```
### Dots and Arrows ###
You use arrows to access a struct's variables when you are obtaining the variables from a pointer to a struct.
You use dots to access a struct's variables when you are obtaining the varaibles from the struct itself.

## Functions on Functions: Function Pointers ##
Sometimes, it may make sense for a function to take another function as a parameter.
This is seen in a lot of sorting, where a sorting function, like quicksort or mergesort, takes in a comparison function.
C has a built-in function called qsort in stdlib.h.  It's declaration looks like this:
```c
 void qsort(void *baseAddress, size_t numElem, size_t sizeElem,
  int (*compareFn)(const void *, const void *));
```
Let's update our contact example to use qsort and a compare function:
```c
#include <stdio.h>
#include <stdlib.h> // for malloc and qsort()

struct Contact {
	char *first;
	char *last;
	int number;
};

/* Met a cute girl at Mel's, gotta add her */
struct Contact *createContact(char *_first, char *_last, int _number)
{
	struct Contact *new = (struct Contact *)malloc(sizeof(struct Contact));
	if (new == NULL)
	{
		printf("malloc failed");
		return NULL;
	}
	new -> first = _first;
	new -> last = _last;
	new -> number = _number;
	return new;
}

/* And now she's making out with my friend, forget her */
void deleteContact(struct Contact *c)
{
	free(c);
}

/* prints a contact info */
void printContact(struct Contact *c)
{
	printf("%s %s's number is %d\n", c -> first, c -> last, c -> number);
}

// void qsort(void *baseAddress, size_t numElem, size_t sizeElem,
//  int (*compareFn)(const void *, const void *));

int compareContacts(const void *c1, const void *c2)
{
	int val;
	if ((val = strncmp(((struct Contact *)c1) -> last, ((struct Contact *)c2) -> last, 20)) == 0)
		return strncmp(((struct Contact *)c1) -> first, ((struct Contact *)c2) -> first, 20);
	return val;
}

int main(int argc, char **argv)
{
	struct Contact myContacts[3];	//our array of contacts that will be sorted

	// allocate memory on heap
	struct Contact *c1 = createContact("Don", "Yu", 1234567890);
	struct Contact *c2 = createContact("Etan", "Zapinsky", 1867877234);
	struct Contact *c3 = createContact("Louis", "Croce", 1555555555);

	// assign contacts
	myContacts[0] = *c1;
	myContacts[1] = *c2;
	myContacts[2] = *c3;

	// call qsort
	qsort(myContacts, 3, sizeof(struct Contact), compareContacts);

	int i;
	for (i = 0; i < 3; i++)
	{
		printContact(&myContacts[i]);
	}

	// free space allocated on heap
	deleteContact(c1);
	deleteContact(c2);
	deleteContact(c3);
	return 0;
}
```
