---
title: "Effective C++: 55 Specific Ways to Improve Your Programs and Designs"
authors: Scott Meyers
year: 2005
date: 2023-07-08 11:56
tags: [literature, cpp, oop, functional-programming, metaprogramming]
---

# View C++ as a federation of languages

[C++](../202302190651.md) should be treated as a combination or glues of different
sublanguages that have different conventions. The author divided them into
roughly four categories: **[C](202302190647.md)**,
**[Object-Oriented](../202202041514.md) C++**, **Template C++**, and
**[STL](../202202241717.md)**. Depending on which sublanguages that we are dealing
with, different rules may apply.

- [x] Item 20: Prefer pass-by-reference-to-`const` to pass-by-value

# Prefer `const`s, `enum`s, and `inline`s to `#define`s

Use `const` and `enum` hack to replace simple constant that is otherwise defined
by `#define` and [Inline Function](../202205171916.md) to replace
function-macro. The reason behind using `const` over `#define` is to ensure that
there's only at most one copy at a time for a constant, easier debugging
experience since `const` variables are inserted into the symbol table by the
[Compiler](../202302152015.md), and able to limit the constant to a class scope
via `static const`. If the value of the constant should be known ahead of
compilation, the enum hack (shown below) could be used such as in the case of
determining an array size. Advantages of using enum is that it makes referencing
to the `enum` elements illegal and avoid possible memory allocation just as in
`#define`. It also forms the fundamental techniques of template metaprogramming.

```cpp
enum { NumOfElement = 10 };
int elements[NumOfElement];
```

Comparing to function-macro, inline function obeys the scope and access rules
and doesn't need to worry about possible side effects without sacrificing the
benefits gained from function-macro.

- [ ] Item 48: Be aware of template metaprogramming
- [x] Item 30: Understand the ins and outs of inlining

# Prefer pass-by-reference-to-`const` to pass-by-value

Pass-by-value is an expensive operation especially for big objects that contain
some information as the compiler will call a chain of copy constructors from
each type. Using pass-by-reference-to-`const`, not only avoid such cost and
enforcing immutability to the object within the function scope, it also avoids
slicing. Slicing happen when the derived class was reduced to base class when
passed as a parameter to a function. Only the base class's copy constructor will
be called, and the functionality defined for the derived class are **sliced
off**, hence the name. Other than that, the author advised to use pass-by-value
only on built-in types and STL [Iterator](../202202241727.md) and function
objects.

- [ ] Item 7: Declare destructors virtual in polymorphic base classes

# Use `const` whenever possible

The author strongly suggested the use of `const` keyword ranging on variables,
function parameters, return types, and member functions as a whole. Especially
in the case of function return type for user-defined type, the return value
should be a constant whenever the passing parameters are constants to avoid
fault like `(a * b) = c`. Obviously, such syntax doesn't make sense for built-in
types, thus it is necessary to make the user-defined types appear like built-in
types and act like one. It is also a good practice to overload non-`const`
function to `const` version in order to handle constant variable.

Instead of relying on the compiler's `const`ness guarantees, we should program
with the mindset of conceptual `const`ness with the assumption that the `const`
member function might modify some object's bits in ways that the clients can't
detect. This could be done with the `mutable` specifier which allows necessary
data to be modified even within the scope of `const` member function. To avoid
code duplication, we could utilise the `const_cast` (to remove `const`
specifier) and `static_cast` (to specifically call the `const` member function
in order to avoid recursive call) to call the `const` member function from a
non-`const` counterpart. The following code shows such implementation:

```cpp
class TextBlock {
public:
    // const member function
    const char& operator[](std::size_t position) const { return text[position]; }

    // non-const member function
    char& operator[](std::size_t position)
    {
        return std::const_cast(std::static_cast<const TextBlock&>(*this)[position]);
    }
}
```

- [ ] Item 24: Declare non-member functions when type conversions should apply
  to all parameters
- [ ] Item 18: Make interfaces easy to use correctly and hard to use incorrectly
- [x] Item 30: Understand the ins and outs of inlining
- [ ] Item 28: Avoid returning "handles" to object internals
- [ ] Item 27: Minimize casting

# Understand the ins and outs of inlining

Inline function could be declared implicitly by declaring the function inside
the scope of class declaration or explicitly with the `inline` keyword. Although
[Inline Function](../202205171916.md) allows context specific compiler
optimisation which could improve potential performance, inline function with
large body may result in increase number of object code which hinder the
performance. The compiler may even refuse to inline a function if it is too
complicated (loop or [Recursive Function](../202202151731.md)) or is a virtual
function (needed to be determined during runtime). For this reason, do not
inline class constructor and destructor as they are generated by the compiler
and may be too complicated for the compiler to inline. Therefore, the author
advises only to use inline function whenever the function called is small and
frequent. Library designer should avoid inline function in order to prevent
binary recompilation on the client side.

- [ ] Item 46: Define non-member functions inside templates when type
  conversions are desired
- [ ] Item 44: Factor parameter-independent code out of templates
- [ ] Item 53: Pay attention to compiler warnings

# Use traits classes for information inside templates

[Iterator](../202202241727.md) could be classified into *input iterator* (move
forward only and read only), *output iterator* (move forward only and write
only), *forward iterator* (move forward only and allow read and write),
*bidirectional iterator* (able to move in two direction and allow read and
write), and *random access iterator* (arbitrary access via iterator arithmetic).

We could design and implement traits classes to store information inside
templates by following certain steps. First, we have to identify the types'
information and its appropriate identifiable name, then provide a template and a
set of specifications that contain the information for the types. We can use the
traits classes to create a set of overload worker functions or function
templates that differ in trait, in which we can create master function or
function template that calls workers, passing information provided by traits
classes.

- [ ] Item 54: Familiarize yourself with the standard library, including TR1
