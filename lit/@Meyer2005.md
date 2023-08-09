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

- [x] Item 48: Be aware of template metaprogramming
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

- [x] Item 7: Declare destructors virtual in polymorphic base classes

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
- [x] Item 18: Make interfaces easy to use correctly and hard to use incorrectly
- [x] Item 30: Understand the ins and outs of inlining
- [ ] Item 28: Avoid returning "handles" to object internals
- [x] Item 27: Minimize casting

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

# Be aware of template metaprogramming

Template metaprogram is a program that executes inside a compiler that will
output source code instantiated from the defined templates then further compile
the source code.

The advantages of template metaprogramming are able to make something possible
comparing to conventional code and turn runtime works into compile time items
thus making error compile-time detectable. [Recursive](../202202151731.md)
template instantiation is possible. It typically results in a smaller
executable, shorter runtime, lesser memory requirement, but longer compilation.

**Note**: Trait class is part of the template metaprogramming.

Template metaprogramming is widely adopted by scientific and engineering
community since it is able to ensure dimensional unit correctness such as
fractional dimensional exponents, and reduce them during compilation. It could
optimises matrix operations via expression templates that eliminate temporaries
and merge loops. Moreover, it can generate custom [Design Pattern](../202211221249.md)
implementation, which introduce policy-based design (generative programming).

- [ ] Item 55: Familiarize yourself with Boost

# Declare destructors virtual in polymorphic base classes

Undefined behaviour might be resulted from the lack of virtual destructor if the
class was not intended to be a base class or used polymorphically. In the case
when the derived class is deleted through the pointer to the supposing base
class with no virtual destructor, only the base class's part will be destroyed
instead including the derived part of the class. This is what we called partial
destroy, which can cause leak memory and corrupted #data-structure.

If the class is intended to be use polymorphically, indicated by its virtual
function(s), its destructor must be declared virtual in order to avoid the above
described situation. Another case that virtual destructor deem useful is when we
want to create an abstract class but lacking any function to begin with. In such
case, we can declare a pure virtual destructor for it.

Though virtual destructor could protect user from resource leak, it comes with a
great cost in size as there is a need of `vptr` and `vtbl` (array of function
pointers) which may increase the object size ranging from 50% to 100%. This
advised from the author is to only declare virtual destructor if the class is
intended to be inherited and used polymorphically, as in there is a need of
manipulation of derived class types through base class interface.

- [x] Item 13: Use objects to manage resources
- [x] Item 18: Make interfaces easy to use correctly and hard to use incorrectly
- [ ] Item 34: Differentiate between inheritance of interface and inheritance of
  implementation

# Make interfaces easy to use correctly and hard to use incorrectly

When designing an interface, we should consider possible clients' mistakes on
using such interface. There are several ways to restrict the client in using the
interfacing by type system (`struct` or `class`) to enforce parameter type
checking, predefine set of values returned from static function (safer than
`enum`), `const` quantifier, and using STL smart pointer as the return type for
[Factory Method](../202302232101.md). Another reason of using static function
instead of `enum` is because of the problematic reliable initialisation of the
non-local static objects. In general, custom types should behave just like the
built-in types, and the interface should be consistent across different data
structure to prevent confusion and misuse. If there is a need of a custom
deleter for a particular object, pass in a custom deleter (a function that
handle the object deletion) to the smart pointer constructor for the type is the
best way to go.

- [ ] Item 22: Declare data members `private`
- [ ] Item 4: Make sure that objects are initialized before they're used
- [x] Item 13: Use objects to manage resources
- [x] Item 27: Minimize casting
- [ ] Item 26: Postpone variable definitions as long as possible

# Use objects to manage resources

Premature return or loop exit and exception could result in resource leak
especially when we choose to manually manage the resources. The author suggests
against the manual resource management as it is not reliable due to its reliance
on the developer to understand the full context and avoid subtle mistakes.
Instead, we should use destructor for automatic resource management. We could
acquire resource and immediately turn it over to resource-managing objects,
either with smart pointers or custom one. This practice is called
[Resource Acquisition Is Initialisation (RAII)](../202202012306.md)

- [ ] Item 8: Prevent exceptions from leaving destructors
- [ ] Item 14: Think carefully about copying behavior in resource-managing
  classes
- [x] Item 15: Provide access to raw resources in resource-managing classes

# Provide access to raw resources in resource-managing classes

The access to raw resources in resource-managing classes such as built-in smart
pointers or custom types could be done either explicitly or implicitly. Meyers
favours explicit access (`get()`) as it offers a safer solution. However, if the
client deems such method is too inconvenient, we could implement implicit
conversion (`->`, `*`, `()`) by sacrificing the resource safety (possible
dangling pointer). Depending on the clients need and interface consistency, we
should choose between these two access methods wisely.

# Minimize casting

Prefer new style cast (`const_cast()`, `dynamic_cast()`, `reinterpret_cast()`,
`static_cast()`) over old C cast since the compiler could diagnose the usage
errors and easy to identify them. That being said, the author still argues
against the use of casting and prefer to minimise casting in the code base since
it often lead to increase runtime cost as in `dynamic_cast()` especially with
deep hierarchy. Casting within the class hierarchy almost guarantees confusion
as it performs on a copy of base class part of the current object instead
directly on the current object itself which results in no modification to the
current object's state. Unless it is necessary, which in that case we should
hide it inside a function, casting is certainly the wrong approach to the
problem. There are generally two solution to refactor casting: by type-safe
[C++ Container](../202202241719.md) or moving virtual function up to the
hierarchy for multi-type usage so that every class within the hierarchy can
perform the same operation.

- [ ] Item 34: Differentiate between inheritance of interface and inheritance of
  implementation
