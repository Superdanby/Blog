---
title: "C++ Notes"
date: 2019-01-05T00:00:00+08:00
categories: [C++]
tags: [C, C++]
toc: true
math: false
---

## Important Concepts

-	[Rvalue Reference(move & perfect forward)](http://thbecker.net/articles/rvalue_references/section_01.html)
-	[glvalue prvalue xvalue](https://en.cppreference.com/w/cpp/language/value_category)
	-	[`decltype` behavior](https://en.cppreference.com/w/cpp/language/decltype)
-	[`constexpr` vs `const`](https://stackoverflow.com/questions/13346879/const-vs-constexpr-on-variables)
`constexpr` variables will be evaluated on compile time while `const` variables may be evaluated
	-	checking with [`noexcept`](https://en.cppreference.com/w/cpp/language/constexpr#Notes)
-	[list initialization vs non-list initialization](https://stackoverflow.com/questions/18222926/why-is-list-initialization-using-curly-braces-better-than-the-alternatives)
-	Read array declarations from inside to outside
```cpp
int \*ptrs[10];            //  ptrs is an array of ten pointers to int
int &refs[10] = /* ? \*/;  //  error: no arrays of references
int (\*Parray)[10] = &arr; //  Parray points to an array of ten ints
int (&arrRef)[10] = arr;  //  arrRef refers to an array of ten ints
int \*(&arry)[10] = ptrs; // arry is a reference to an array of ten pointers
```
-	`size_t` is a machine-specific unsigned type that is
guaranteed to be large enough to hold the size of any object in memory.
-	[lambda expression vs inline function](https://stackoverflow.com/questions/13722426/why-can-lambdas-be-better-optimized-by-the-compiler-than-plain-functions)
	- [The `inline` specifier will not guarantee the function call to be inlined.](https://stackoverflow.com/questions/15930755/are-lambdas-inlined-like-functions-in-c#answer-15931934)
