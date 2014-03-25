---
layout: post
title: "learning concept checking the hard way - part 2"
date: 2014-03-24 18:38
comments: true
categories: 
---

In the [previous post](blog/2014/03/21/learning-concept-checking-the-hard-way-1/), I talked about importance/use case of concept
checking. In this post we'll step by step see how concept checking
works.

As an example, we'll write a `has_less` method, which for any
given type will return true or false based on whether `<` is defined for
that type or not.


Let's first look at some of the c++ concepts which we'll use in the
process.

### 1. decltype

`decltype` let's you extract type from any expression. It's available in
C++11

For example, In the example below, decltype can be used to get the type
of variable `myType1` and used to define another variable with the same
type.

```cpp
//dec.cpp file
class MyType{
public:
    MyType(){std::cout << "Mytype: constructor called." << std::endl; }
};

int main(int argc, const char * argv[])
{
    MyType myType1;
    decltype(myType1) myType2;
    return 0;
}

>> ./a.out
Mytype: constructor called.
Mytype: constructor called.

```

**Takeaway**: we can use **decltype** to get type of a variable.

### 2. Template specialization

Templates in c++ are turing complete and you can do lot of cool stuff
with them. Some trivia about templates:

* **Templates are instantiated at compile time and only if they are used
   somewhere in a program.** However they are checked for syntax.

   Consider the example below, where we have a generic template method(`hello(T a)`) that
   calls a method(`iDontExist`) that is not defined anywhere. However the code
   compiles successfully since we are not calling that method.

   Note the specialization of the template for int type. When compiler
   has more than one functions with the same name, it looks at the type
   and select the best match. In case of `hello<int>(2)`, it selects the
   specialized template over generic so, it instantiates only the
   specialized template which is both syntactically and semantically correct.


```cpp

// This is a valid code and will compile just fine
template <typename T>
void hello(typename T::foo a){
    int b = T::iDontExist(12); //iDontExist doesn't exist.
    std::cout << a << std::endl;
}

// Another possible candidate.
template <typename T>
void hello(T a){
    std::cout << a << std::endl;
}

int main(int argc, const char * argv[]){
    hello<int>(2);
    return 0;
}

```


* **Substitution failure is not an error (SFINAE)**

Note in the above program, when `hello<int>` is called compiler first
tries to match it with the `void hello(typename T::foo a)`, however `int` doesn't have a nested type `int::foo`.
This is called a substitution failure, compiler failed to substitute
typename T for int. 

Compiler doesn't throw a compilation error here; it removes this
overload from potential candidate and tries to
find the next possible match. If one or more candidates remain and overload resolution succeeds, 
the invocation is well-formed. Check out more about SFINANE on [wikipedia](http://en.wikipedia.org/wiki/Substitution_failure_is_not_an_error)

* **Given two possible candidates where one is variadic and other is not.
  Compiler always chooses the non-variadic method.** For example, In the
  example below compiler has two candidates for `hello<int>`. Compiler
  chooses the non-variadic one.

```cpp
template <typename T>
// Note the variadic(...) arguments
void hello(...){
    std::cout << "variadic method." << std::endl;
}
// Another possible candidate.
template <typename T>
void hello(T a){
    std::cout << "non-variadic method." << std::endl;
}
int main(int argc, const char * argv[]){
    hello<int>(2);
    return 0;
}
>> ./a.out
non-variadic method.

```
---

Now let's start building our `has_less` method to identify if certain
method exists for a certain type.



