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

- [x] Item 24: Declare non-member functions when type conversions should apply
  to all parameters
- [x] Item 18: Make interfaces easy to use correctly and hard to use incorrectly
- [x] Item 30: Understand the ins and outs of inlining
- [x] Item 28: Avoid returning "handles" to object internals
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

Functions within class are also inlined by default. To avoid that, call a help
function defined outside the scope of the class.

- [x] Item 46: Define non-member functions inside templates when type
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
instead of including the derived part of the class. This is what we called
partial destroy, which can cause leak memory and corrupted #data-structure.

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
- [x] Item 34: Differentiate between inheritance of interface and inheritance of
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

- [x] Item 22: Declare data members `private`
- [x] Item 4: Make sure that objects are initialized before they're used
- [x] Item 13: Use objects to manage resources
- [x] Item 27: Minimize casting
- [x] Item 26: Postpone variable definitions as long as possible

# Use objects to manage resources

Premature return or loop exit and exception could result in resource leak
especially when we choose to manually manage the resources. The author suggests
against the manual resource management as it is not reliable due to its reliance
on the developer to understand the full context and avoid subtle mistakes.
Instead, we should use destructor for automatic resource management. We could
acquire resource and immediately turn it over to resource-managing objects,
either with smart pointers or custom one. This practice is called
[Resource Acquisition Is Initialisation (RAII)](../202202012306.md)

- [x] Item 8: Prevent exceptions from leaving destructors
- [x] Item 14: Think carefully about copying behavior in resource-managing
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

- [x] Item 34: Differentiate between inheritance of interface and inheritance of
  implementation


# Differentiate between inheritance of interface and inheritance of implementation

There are three ways to specify the type of inheritance should the subclass be
inherited, i.e., with pure virtual function, simple virtual function, or
non-virtual function. With pure virtual function, it indicates that the function
should be inherited by interface only. To allow default implementation, using
simple virtual function is advised with implementation specified in the function
body. A better way to do this is by defining two separate functions: a pure
virtual function that provide the interface, and a protected non-virtual
function that provide implementation, thus the subclasses could choose over
whether to implement their own algorithm or to use the ready one. On the other
hand, non-virtual function indicates that such functionality is mandatory and is
not intended to be overridden.

A base class, that is class intended to be inherited, should have at least one
virtual function (destructor). Avoid declaring all function as virtual
unnecessarily.

- [x] Item 32: Make sure public inheritance models "is-a"
- [x] Item 36: Never redefine an inherited non-virtual function

# Prevent exceptions from leaving destructors

Within a [C++ Container](../202202241719.md), if two items inside it encountered
exception inside their destructors, it will result in undefined behaviour.
Therefore, it is advised that exception must come from non-destructor function
by providing regular function to perform resource management if the client needs
to react to the exception accordingly (usually alongside with a dedicated
resource-managing entity). Then, define a destructor from the resource handler
to either terminate or swallow the exceptions coming from its scope as the
default behaviour.

# Think carefully about copying behavior in resource-managing classes

There are several considerations on copying behaviour in resource-managing
classes, which are prohibiting copy, reference counting (smart pointer with
custom deleter), deep copy, or ownership transfer.

- [x] Item 6: Explicitly disallow the use of compiler-generated functions you
  do not want
- [x] Item 5: Know what functions C++ silently writes and calls

# Postpone variable definitions as long as possible

Avoid unused variables and be especially cautious in the case of #exception. A
better practice, argued by Meyer, is to postpone the variable until it is
necessary for it to be initialised with an argument. For loop, unless the
assignment has less expensive constructor-destructor pair and we are dealing
with performance issues, the variable should be defined inside the loop body.

# Make sure that objects are initialized before they're used

For the C part of the C++, i.e., the built-in types, there is no initialisation
guarantee from the language prior to the assignment. Note that initialisation
happen earlier than assignment, thus it is more efficient to initialise an
object instead of via assignment especially in the case of class. The order of
initialisation for class hierarchy is that the initialisation of the base class
happen before its derived children.

The relative order of initialisation of non-local static objects defined in
different translation units is undefined by the language. The state objects
referred to global object, object defined at namespace scope, static object
within class, static object within functions, and file static object. Only local
static object is the static objects defined in the function scope. Translation
unit refers to the source code that give rise to a single object file. It
happens when a non-local static object in one translation unit is initialised
using other non-local static object in different translation unit. The solution
is to transform non-local static object into static function that return the
reference of that object, thus reforming it into a local static object. The
function could be further optimised using `inline` specifier. However, static
object still suffers problem in multithreading environment due to [Race Condition](../202112061109.md).
Meyer proposes that we could bypass such concern by manually invoke all
reference-return functions during single-threaded start-up portion of the
program.

# Know what functions C++ silently writes and calls

A smart compiler will generate the codes for you as needed. For example, upon
declaration, default constructor and destructor will be created by the compiler
in order to manage the class's resources. Instantiation will trigger generation
of default copy constructor while reassignment will cause generation of copy
assignment operator. The compiler will not generate constructor for the user if
they have already defined their own constructor, regardless of the number of the
parameters.

**Note**: Unless declared virtual on the based class, the derived class's
default destructor will be non-virtual.

The author advised the reader to define their copy assignment if the class
declared has referenced data member(s) in it. Because there will be a lot of
problems dealing with referenced data member in which cases such declaration
would be illegal, the compiler will throw out the compilation error.

# Item 6: Explicitly disallow the use of compiler-generated functions you do not want

We can explicitly disable code generation by declaring functions like copy
constructor to be private, accompany by simple declaration, i.e., we don't
implement the copy constructor, so that no function can legally call it. If a
client tries to call it, it will throw a link error. To convert such error into
compile time, we can implement it via class inheritance with the disable
function declared as private.

**Note**: This seems to be replaced by the syntax sugar `delete`.

- [x] Item 32: Make sure public inheritance models "is-a"
- [x] Item 39: Use private inheritance judiciously
- [x] Item 40: Use multiple inheritance judiciously

# Item 32: Make sure public inheritance models "is-a"

Public inheritance indicates "is-a" relationship between the superclass and
subclass, meaning subtype can be the supertype, but not the other way around.
However, there is a discrepancy between the real world categorisation and
[OOP](../202202041514.md) as there may have some exceptions. Implementing
runtime error to indicate an object can't do something doesn't semantically mean
that it can't do something, but it simply does it with the error output.

- [x] Item 39: Use private inheritance judiciously
- [x] Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition

# Item 38: Model "has-a" or "is-implemented-in-terms-of" through composition

Composition has two meaning that applies on two domains which are application
domains and implementation domains. In terms of implementation domains,
composition actually means that something is implemented in terms of something
else.

# Item 39: Use private inheritance judiciously

Private inheritance doesn't convert the derived class into the base class. It
should be viewed as is-implemented-in-terms-of relationships in the hierarchy.
The author suggests only to use private inheritance when there are only
protected members and/or virtual functions. However, even in that case, we could
use private nested class with public inheritance to avoid redefinition of the
function from the derived children and minimise the compilation dependencies.
The only edge case in which private inheritance seems useful is to prevent
padding over empty class, as in no non-static data, virtual functions, and
virtual base class, and it only works under single inheritance.

- [ ] Item 35: Consider alternatives to virtual functions
- [ ] Item 50: Understand when it makes sense to replace `new` and `delete`

# Item 40: Use multiple inheritance judiciously

In multiple inheritance, overloaded functions are not examined based on its
accessibility (public, private, or protected), but whether it is the best match.
Therefore, it is unwise to inherit multiple classes that have the same function.
We can disable this replication by implementing virtual base class. However,
this comes with performance and space trades off. The author advises against the
use of virtual base class unless it is necessary. If there is a case for that,
don't put any data in it. A great use case of multiple inheritance is for
defining interface via public inheritance, and implementation via private
inheritance.

# Item 22: Declare data members `private`

Declaring all data members as `private` and use function to access them with
fine-grained control policy. Though not every data member needs getter or
setter, it could be encapsulated into function which allow much implementation
flexibility. Since only member function only affect the data member(s), the
class invariant could be maintained from the client violation. Another case
against `public` and `protected` data members is that they make things
unchangeable as they will break code if changed (for `protected`, it changes all
derived classes).

- [x] Item 23: Prefer non-member non-friend functions to member functions

# Item 36: Never redefine an inherited non-virtual function

The author advises a one guideline in designing a class, that is the same object
must have the same behaviour regardless of the result of casting. In fact,
non-virtual function should not be redefined in principle as it is statically
bounded to its corresponding class and entirely depends on the type of the
pointer's type declaration. Inherited non-virtual functions should be treated as
invariants so that the objects, albeit the derived ones, will have the same
behaviour. If that is not the case for such, one should use virtual function for
it as it is dynamically bound, and not depends on the pointer's type
declaration. This makes us rethink the [OOP](../202202041514.md) relationship of
the object: if one object really needs to reimplement one of the non-virtual
member function of another object, then it must not be in an "is-a" relationship
with such object.

- [ ] Item 33: Avoid hiding inherited names
- [ ] Item 37: Never redefine a function's inherited default parameter value

# Item 23: Prefer non-member non-friend functions to member function

Member function that does interact with other member functions that manipulate
data member(s) yield less encapsulation. Friend function has the same issue with
member function as it can gain access to the private part of the class.
Therefore, the author suggests non-member non-friend function as a replacement
in order to increase the packaging flexibility for class functionality which
yields fewer compilation dependencies and increase class extensibility. By such,
we should decrease the number of function accessing the private part of the
class. We can conclude that encapsulation is proportional to the class
flexibility and extensibility.

Another advice by Meyer is to split convenient functions into different headers
with the same namespace so that the client can choose which to compile to
without adding unnecessary dependencies.

# Item 24: Declare non-member functions when type conversions should apply to all parameters

If there is a need of type conversions on all parameters to a function, such
function must be a non-member function. For examples, if a rational number,
constructed as a class, must behave communicative in multiplication just like
the primitive's counterpart, then a non-member operator function that includes
type conversions parameters is the way to go.

- [x] Item 46: Define non-member functions inside templates when type
  conversions are desired

# Item 28: Avoid returning "handles" to object internals

Handles are ways to get at other objects. This includes references, pointers,
and iterators. Returning such handles from a member function, even within a
`const` scope, runs the risk of exposing data member that was supposed to be
protected or private. Quote "The data member is as encapsulated as the most
accessible function that return function to it." from the authors. This means
that the function caller can modify the value of the returned handle from
`const` member function if it is stored outside the scope of the function. Even
with specified `const` return modifier, it also runs the risk of possible
dangling pointer at the end of the original handle's lifetime. The pointer
points to the returned handle will dangle once the returned handle is destroyed.

# Item 46: Define non-member functions inside templates when type conversions are desired

Templatised function doesn't do implicit type conversion during template
argument deduction. For that to happen, we have to declare non-member function
as `friend` and define it inside the template. `friend` modifier allows
non-member function to be defined inside a template, which in turns allow proper
function instantiation.

To minimise inlining, the function may call a helper function outside the
template.

# Item 44: Factor parameter-independent code out of templates

Templates may lead to code bloat due to unnecessary duplication resulted from
template parameters. We have to do **commonality and variability analysis** on
the parameters, including non-type and type parameters. This can be done by
factoring out the common parts (in this case the template parameters). For
common non-type parameter(s), we factor it out to form a derived class and hide
the templatised version from the public to ensure encapsulation. The main
functionality will be defined in the base class where the derived class will
redirect to it. For type parameter(s), we can share implementations for
instatiation types with identical binary representation like `int` and `long`.

Such approach comes with the tradeoffs of less optimisation opportunity for the
compilers and increase overall size of each object.
