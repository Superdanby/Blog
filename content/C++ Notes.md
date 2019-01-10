---
title: "C++ Notes"
date: 2019-01-05T00:00:00+08:00
categories: [C++]
tags: [C, C++]
toc: true
math: false
---

# Important Concepts

## Rvalue Reference
-	[Rvalue Reference(move & perfect forward)](http://thbecker.net/articles/rvalue_references/section_01.html)

## Value Category
-	[glvalue prvalue xvalue](https://en.cppreference.com/w/cpp/language/value_category)
	-	[`decltype` behavior](https://en.cppreference.com/w/cpp/language/decltype)

## `constexpr`
-	[`constexpr` vs `const`](https://stackoverflow.com/questions/13346879/const-vs-constexpr-on-variables):
`constexpr` variables will be evaluated in compile time while `const` variables may be evaluated in compile time or runtime.
-	checking with [`noexcept`](https://en.cppreference.com/w/cpp/language/constexpr#Notes)

## List Initialization
-	[list initialization vs non-list initialization](https://stackoverflow.com/questions/18222926/why-is-list-initialization-using-curly-braces-better-than-the-alternatives)

## Array declaration
Read array declarations from inside to outside
{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
int *ptrs[10];            //  ptrs is an array of ten pointers to int
int &refs[10] = /* ? */;  //  error: no arrays of references
int (*Parray)[10] = &arr; //  Parray points to an array of ten ints
int (&arrRef)[10] = arr;  //  arrRef refers to an array of ten ints
int *(&arry)[10] = ptrs; // arry is a reference to an array of ten pointers
{{< / highlight >}}

## `size_t`
`size_t` is a machine-specific unsigned type that is guaranteed to be large enough to hold the size of any object in memory.

## Lambda expression
-	[lambda expression vs inline function](https://stackoverflow.com/questions/13722426/why-can-lambdas-be-better-optimized-by-the-compiler-than-plain-functions)
	-	The `inline` specifier will not guarantee the function call to be inlined.[^1]

[^1]: https://stackoverflow.com/questions/15930755/are-lambdas-inlined-like-functions-in-c#answer-15931934

## `auto` Runtime Performance

Type deduction is done in compile time instead of runtime.[^2]

[^2]: https://stackoverflow.com/questions/19618759/c-11-auto-compile-time-or-runtime

# Interesting Stuff

## Non-const Initialized Array

This is against the ISO standard. But, GCC allows this as an extension by default.

Pros:
-	convenient when initializing variable length arrays
-	Reduces memory fragmentation on heap.

Side effects:
- Users may be able to overuse stack, causing runtime error.

## Stopping Loops Before End

{{< highlight cpp "linenos=table,hl_lines=3 5-7,linenostart=1,noclasses=false" >}}
auto back = std::prev(STL.end());
for(auto it = STL.begin(); it != back; it = std::next(it));
{{< / highlight >}}

## Syntax Alias

-	`Using A = B`: A will be resolved as B at syntax level, similar to `#define`
