## Pointers ##

### What's a pointer anyway? ###

A pointer is a data type whose value is an address in memory.  The value "points" to another value stored in that memory location.

Here is an example of a declaration of a pointer c which points to a char.
```c
    char *c;
```
The * in the declaration lets us know that we are working with a pointer.
The data type before the * indicates the data type or structure that the pointer points to.

We also use a * when accessing the data that the pointer points to.
```c
    char b;
    b = *c;
```
Here, b would take on the value on what ever was pointed to by the char pointer c.

To access the address of a variable, we use an &.
```c
    int x = 5;
    int *y;
    y = &5;
```    
This tells y to point to the address of the int x.

### I still don't get it ###

Legendary Professor Alfred Aho once compared data types to people and pointers to where they live. Hopefully the analogy will make pointers easier to understand for you as well!

Imagine that I have a person named Joe who is represented by an int. Let's say his value is 13.

```c
    int person = 13;
```

He lives somewhere (in this case in your computer) and you can ask him for his address in the computer (just like you could ask a person his home address) and store it somewhere. *Note we use & to ask a variable for its address.

```c
    int *persons_address = &person;
```

It's been a while and I want to vist Joe again and see how he's doing. Good thing I have his address.

```c
    printf("%d", *persons_address);
```

*Note that here we use * to travel to a pointers address and get the contents. It's kind of like traveling to Joe's house and asking for him.
    
And that's pointers in a nutshell.

### Pointer Example ###

Walk through the following program, keeping track of what pointers point to what, and what's happening at each step.
```c
#include <stdio.h>

int main(int argc, char **argv)
{
    int i,j;
    int *x;
    int *y;
    char a,b;
    char *k;

    i = 7;
    j = 97;
    x = i;
    y = &j;
    a = 'a';
    *k = a;
    b = *k;
    a = 'c';
    a = (char)j;

    if (a == b)
    {
        printf("a and b are equal!\n");
    }

    printf("i equals %d\n", i);
    printf("j equals %d\n", j);
    printf("the value stored in y equals %d\n", *y);
    printf("a equals %c\n", a);
    printf("b equals %c\n", b);
    printf("the value stored in k equals %c\n", *k);
    printf("the value of x equals %d\n", x);
    printf("the value stored in x equals %d\n", *x);

    return 0;
}
```
