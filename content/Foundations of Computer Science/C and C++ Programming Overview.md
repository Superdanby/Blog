---
title: "C/C++ Programming Overview"
date: 2018-12-10T20:00:00+08:00
categories: [Foundations of Computer Science]
tags: [C, C++]
toc: true
math: false
---

## Basic types

- data register
- `int`, `float`, `char`, `long long`, `double`, `short`
- C++ exclusive: `bool`

```c
int i = 0;
int a = b;
```

## Array

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
char carray[23];
// char carray2[4] = a;
long long po [8];
po[0] = 1;
po[7] = po[0];
// po[8];
{{< / highlight >}}

## Pointers and Reference

- address register
- *, &

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int a = 10, b = 1;
int* aptr = NULL;
// double * dptr = &a;
aptr = &a; // aptr -> a, a = 10, b = 1
*aptr = b; // aptr -> a, a = 1, b = 1
a = 2; // aptr -> a, a = 2, b = 1
b = 7; // aptr -> a, a = 2, b = 7
&a = b; // aptr -> old a, old a = 2, b(a) = 7;
printf("%d %d", a, *aptr); // 7 2

int * potato = NULL;
if(potato == NULL)
    printf("hi");

potato = (int *)malloc(sizeof(int) * 127);

if(potato == NULL)
    printf("Memory allocation failed.\n");

potato = NULL;
if(potato != NULL)
    free(potato);

potato[4] = 4; // *(potato + 4) = 4;
{{< / highlight >}}

## Operators

1. `=`, assignment, ``double d = 3.1415926;``
2. `+ - * / %`, basic arithmetic, ``e = d / 12;``
3. `< > ==`, comparisons, ``a > b``
4. `&& ||`, logic operations, ``a && b || c``
5. `& | ^ >> <<`, bit operations, ``a & 1`` === ``a % 2 == 1``
6. `()`, priority, ``a || b && c`` vs ``(a || b) && c``
7. `++`, `--`, increment & decrement by 1, ``++a vs a++``
8. [Precedence](https://en.cppreference.com/w/cpp/language/operator_precedence)

## Conditional control

- 0 means `False`, non-0 means `True`

1. `if`, `else`

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
if(1)
{
    printf("Potato");
}
else
{
    printf("P04A40");
}

int a = 0;
if(a++)
{
    printf("Smashed potato.");
}
{{< / highlight >}}

2. `switch` `case`

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int a = 32, b = 0;

switch (a) {
    case 32:
        printf("32 potatos");
    case 0:
        printf("Void");
        break;
    default:
        printf("Frog");
}
{{< / highlight >}}

## Loops

1. `for`

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int i = 1;
for(i -= 1; i <= INT_MAX; i++)
{
    printf("%d", i);
}

float a[10];
float * b = (float *)malloc(sizeof(float) * 10);
float* a_end = a + 10;

for(float* a_ptr = a; a < a_end; a++)
    printf("%f ", *a_ptr);

// deferences between array and pointer
// C++ only
for(auto x : a)
    printf("%f ", x);
/*
for(auto x : b)
    printf("%f ", x);
*/
{{< / highlight >}}

2. `while`

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
while(True)
{}
{{< / highlight >}}
{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int x = 10;
while(x--)
{}
{{< / highlight >}}

## Functions

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <stdio.h>
#include <stdlib.h>

int potato(int frog)
{
    int potato2;
    frog = 11;
    return 0;
}

int main()
{
    int frog = 1;
    int potato2;
    potato(frog);
    printf("%d\n", frog);
    return 0;
}
{{< / highlight >}}

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <stdio.h>
#include <stdlib.h>

int potato()
{
    // int frog = 11;
    // return frog;
    return 11;
}

void potato2(int * frog1, int * frog2)
{
    *frog1 = 11;
    *frog2 = 12;
    return;
}

int main()
{
    int frog = 1;
    frog = potato();
    printf("%d\n", frog);

    frog = 1;
    int frogg = 2;
    potato2(&frog, &frogg);
    printf("%d %d\n", frog, frogg);
    return 0;
}
{{< / highlight >}}

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <stdio.h>
#include <stdlib.h>

int potato(int ** frog)
{
    // int a[1];
    // int * aptr = (int *)malloc(sizeof(int));
    *frog = (int *)malloc(sizeof(int));
    (*frog)[0] = -1; // *(frog + 0) = -1;
    return 0;
}

int main()
{
    int * frog = NULL;
    potato(&frog);
    printf("%d\n", frog[0]);
    return 0;
}
{{< / highlight >}}

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <iostream>

using namespace std;

int potato(int &frog) // int &frog(potato function) = frog(main function)
{
    frog = 11;
    return 0;
}

int main()
{
    int frog = 1;
    potato(frog);
    cout << frog << "\n";
}
{{< / highlight >}}

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <iostream>

using namespace std;


void swap(int &potato, int &frog)
{
    int s = potato;
    potato = frog;
    frog = s;
    return;
}

int main()
{
    int potato, frog;
    cin >> potato >> frog;
    swap(potato, frog);
    cout << potato << frog << "\n";
}
{{< / highlight >}}

1. [Recursion](https://www.google.com.tw/search?q=Recursion&spell=1&sa=X&ved=0ahUKEwjDzPnI6P3eAhVG6bwKHTPkBxcQBQgoKAA&biw=1536&bih=785)

## Scope

- `global`
- `local`
- `{}`

## User-defined Types

- `typedef`
- `struct`

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
typedef long long ll;

typedef struct potato{
int frog;
potato* next;
} potato;
{{< / highlight >}}

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
struct potato {
int frog;
potato* next;
};

potato potato1, potato2;
potato1.frog = 1;
potato1.next = &potato2;
potato2.frog = 2;
potato2.next = &potato1;

potato * potato_ptr;
potato_ptr -> frog = potato1.frog;
potato_ptr -> next = potato1.next;

for(int i = 0; i < 10; i++)
{
    cout << potato_ptr -> frog << " ";
    potato_ptr -> frog = (potato1 -> next) -> frog;
    potato_ptr -> next = (potato1 -> next) -> next;
}

potato * potato3, * potato4;
potato3 -> frog = 3;
potato3 -> next = potato4;
potato4 -> frog = 4;
potato4 -> next = potato3;

potato_ptr = potato3;

for(int i = 0; i < 10; i++)
{
    cout << potato_ptr -> frog << " ";
    potato_ptr = potato_ptr -> next;
}
{{< / highlight >}}

## Preprocessors

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
#include <stdlib.h>
#include "user-defined.h"

#define potato frog

int x = potato; // int x = frog;

#define multiply(potato2, frog2) (potato2 * frog2)

int y, z;

x = multiply(y, z); // x = (y * z);

#undef potato

x = potato; // x = potato

#if 1 == 2
    printf("OMG!\n");
#elif 2 == 3
    printf("LOL\n");
#else
    printf("LGTM!\n");
#endif

#ifndef potato
    #define potato
    #include "potato.h"
#endif
{{< / highlight >}}

## Qualifiers

- `const`: forbids assignement
{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int a = 10;
const int b = a;
int * ptr1 = a;
const int * ptr2 = a;
int const * ptr3 = a;
int * const ptr4 = a;
const int * const ptr5 = a;
{{< / highlight >}}

- `static`
{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
foo()
{
    static int i = 0;
    ++i;
}
bar()
{
    int i = 0;
    ++i;
}
{{< / highlight >}}

- [`volatile`](https://en.wikipedia.org/wiki/Volatile_(computer_programming)#Optimization_comparison_in_C)
