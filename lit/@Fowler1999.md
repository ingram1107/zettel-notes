---
title: "Refactoring: Improving The Design of Existing Code"
authors: Martin Fowler
year: 1999
date: 2022-09-29 09:56
tags: [literature, refactoring, oop]
---

# Preface

[Refactoring](../202206032059.md)# is *the process of changing a software system
in such a way that it does not alter the external behaviour of the code yet
improves its internal structure*. (p. xvi)

Refactoring helps reduce bugs and improve the design. With cumulative of small
changes on the project, it will have drastic improvement on the design which
prevent software decay.

# Chapter 1

A system that is hard to change (finding where the change is needed) is a poorly
designed system. This will ultimately introduce bugs to the system. The modules
or functions used in the system should be reusable especially if there will be
frequent changes on the system.

The first thing to do when refactoring, is to set up
[tests](../202206091002.md)# for the soon-to-be refactored code. The tests
should be self-checking, that is to say "OK" when the tests passed and print out
a list of failures when it is not.

Decompose long method, as that often mean a convoluted method or function which
has too many responsibilities attached to it. Smaller methods or functions are
easier to work with. Inspect what are the variables and parameters that are
local to the scope of soon-to-be refactored code, and then move them into
chunk(s). Rename the variables and parameters that fits its usage for more
clarity.

> *Any fool can write code that a computer can understand. Good programmers
> write code that humans can understand.* (p. 15) 

If the class method doesn't use any information from the class itself but
instead uses information from other class(es), the method could be on the wrong
object. Move the method to the class where it uses the most information of.

Now, remember to find all references to the old method across code base. You can
either remove the old method or just redirect it to the newly refactored method.

If there are temporary variables holding a value from an expression, remove
them, and use the query or function directly. The author argued that temporaries
cause a lot of parameters to be passed around when they don't have to be, with
additional benefit that queries could be reused by other classes or method.
However, this refactoring practice will pay a price in terms of performance as
the value will be calculated over and over again. However, the optimisation
should be done in the method, the class or the function (use cache?).

Do refactor little by little, so that it is less prone to go wrong.

As the author successfully extended the program with the feature of outputting
the HTML format of the movie rental, there are striking similarities between the
original `statement()` and new `htmlStatement()` method for the class. The
solution is to *form template method*. First, create a new class with both the
methods as its children classes. The original class will retain the method, but
will call the procedure from the children classes. Then extract the varying
codes and form a method accordingly to each class. *Pull up the main method* to
the parent class.

It is advised by the author to not do a conditional statement such as `switch`
and `if` based on an attribute of another object. If there is a need for a
conditional statement, it should instead be done on its own data not others. We
can refactor the method by moving it to the object instead of the current one.

If there's a need to change the object's state or behaviour during the lifetime,
refactor the class to adhere the [State Pattern](../202210211637.md)# will be a
good idea. This is especially useful on the classes that involves `switch` or
other conditional statement in order to provide different services based on its
current state. It eliminates the need for conditional statement and type code,
and instead relies on polymorphism to do the job.

# Chapter 2

> Refactoring is a change made to the internal structure of software to make it
> easier to understand and cheaper to modify without changing its observable
> behaviour.

It is similar to [Optimisation](../202203011139.md), however with a different
purpose. Optimisation is to speed up the program but might end up with less
comprehensibility of the codes whereas [Refactoring](../202206032059.md) wants
to make the software easier to understand and extendible.

As a software developer, we have two hats: adding function and refactoring. When
we are adding function, we don't change the existing code. We're merely adding
new capabilities and tests to the software. When we are refactoring, no new
functionalities or tests (unless you miss a case) should be added to the code
base. Instead, we are just changing the internal structure of the code. Changing
test is allowed only if an interface change force it to. It is quite common for
a software developer to change their hats often. We just need to be aware what
hat we are on right now.

Fowler stated four benefits of refactoring: it improves the *design of
software*, it makes software *easier to comprehend*, it helps in *finding bugs*,
and it *speeds up development*.

Without refactoring, the code will lose its structural cohesion as people change
the codes for various intention. The harder it is to see the design in code, the
harder it is to preserve it, and the more rapidly the structure of the code
decays. Refactoring helps code retain its shape (p. 55). Poorly designed code
usually has [duplications](../202206171004.md)# where different codes are doing
the same thing. Refactoring can remove such duplications. A good design has only
the code that says everything once and once only.

Software development is always about human, not the computer. The code should be
readable so the next guy who picks up the codebase won't waste too much time on
understanding the code before adding more codes. Refactoring makes the code
speak its purpose with more clarity. A clearer codebase is also easier to spot
bugs. Overall, it speeds up the whole software development process. Rapid
software development is about consistent speed not a burst.

The author opposed the idea to schedule refactoring. Quoted from him, "You don't
decide to refactor, you refactor because you want to do something else, and
refactoring helps you do that other thing." (p. 58) There are hints that a
refactoring is necessary. If there is a duplication of functionality, follow Don
Roberts' Rule of Three: "Three strikes and you refactor", where strike means the
functionality or code that shares similarities. Refactoring should be done too
when you want to *add function*, *fix a bug* (because it is hard to spot a bug),
and/or doing a *code review* (with a small review groups consist of one reviewer
and the original author, essentially pair programming). If adding function is
*not easy*, that indicates a need to refactor to improve the design. In larger
groups, code review could be done with the UML diagrams and CRC cards.

If the manager questions about the legitimacy of refactoring, depending on their
expertise and/or focus, there are different ways to deal with them. Let's say
the manager has a technical background, we could introduce refactoring to him.
If the manager is quality-oriented, persuade them by saying refactoring is
essentially a review on the code which could reduce buys and thus speed up the
development. However, if the manager is focus on schedule, then simply don't
tell, refactoring silently as it generally speeds things up.

However, there are several good points to not refactor the codes. First, when
the application is highly coupled to the database schema. It is rather hard to
refactor the codes as database is difficult to change and because of data
migration ([Persistence Programming](@Cobbs2022.md)# aims to solve that). A
change to the schema could result in a data migration which could be a
frustratingly long task. A suggestion coming to from the author is to create an
intermediate layer between the object model and database model so that updating
one model only result in a change of intermediate layer rather than the other
model.

Second, if the interface is published, then it is not recommended refactoring
the interface without first notifying the users that the method is being
refactored especially if the method name changed. Retain the old method, and
redirect it to the newly refactored method can solve the problem, but the code
could be harder to maintain. The point here is to not overuse published
interface.

Third, if starting from scratch is an easier approach. The sign for it is when
the codebase doesn't work at all and is full of bugs, and it is impossible to
stabilise it. There is a way to do it though with refactoring, is by turning a
large piece of software into components with strong encapsulation. From here, we
can make a refactor-versus-rebuild decision for one component at a time.

Forth, obviously, you shouldn't do refactoring when a deadline is close. It is
too late to refactor right now.

A perfect upfront design is too idealistic in planning. Many times, people will
miss some details about the project. Refactoring provides an alternative to this
way of software development strategy, argued by Fowler, and it is about refining
the first ever design that come out from our mind. Extreme Programming practices
this. First, they come out with design ideas using CRC cards, UML diagrams or
other design tools and discuss for a first plausible design solution. The
mindset changed: we don't try to find the best solution, instead we find a
reasonable solution which tends to be simple. Flexible solution is great, but
too much flexibility could result in a complex design.

Although refactoring come at a cost of performance penalty due to indirection,
it opens opportunities for fine-tuning since the code is more readable than
ever. With refactoring, components tend to be small and easier to test. This
means that it is easier to find out which part of code needs to be optimised
which is measured by [profiler](../202203011139.md) since they are rather
decoupled.

## Additional

Kent Beck identifies four types of programs that are hard to modify:
- programs that are hard to read
- programs that have duplicated logic
- programs that require additional behaviour that requires you to change running
  code
- programs with complex conditional logic

The goal of the refactoring is to make the program easy to read, have all logic
specified in one and only one place ([DRY Principle](../202206171004.md)#),
doesn't allow changes to alter existing behaviour and allow only simple
conditional logic.

# Chapter 3

There are several indications that the code needs to be refactored.

The obvious one is there is [duplicated](../202206171004.md)# code. If the same
code structure appear in more than one places, extract it into a new method and
then replace the code in place with the method. If it appears in *sibling
subclasses*, then extract it into a new method and pull it up to the parent
class. Be aware if the code is similar but with slightly different steps, form a
template method where the similar bits will be placed, and different bits to be
in new method that will be called in template method. This way, the template
method will be easier to read, and the implementation details will be
encapsulated in other methods. If the methods are doing the same thing, but one
is doing it is a simpler way, adopt the simpler method over the complex one. If
the duplicated code is in unrelated classes, then extract it into a class and
make it a component in both classes. That being said, it is highly dependable on
the situation which class does the code or method should belong to.

If there appears to be a long method in terms of line, the usual solution is to
extract a method. If instead the method has lots of temporary variables, replace
temporary variables with query method. With long lists of parameters, we could
introduce parameter object if they tend to be together or preserve whole object
(pass in the whole object) if the values are from the same object. However, if
there is a need to preserve the dependency structure, don't preserve the whole
object as it will cause a dependency between the required object and the called
object. If you can get the data in one parameter making request of an object
that ready to be called, replace the parameter with method that called inside
the method. If after all these chores, the lists of parameter or temporary
variables is still too many, simply replace method with a new class.

Fowler suggests that if the code needs comment to explain what it is doing,
there is a good chance that it is better to be a method with the name based on
the comment. Complicated conditional expressions could be decomposed using
methods. Loop could be encapsulated into its own method.

Another refactoring opportunity is that a class have too much instance
variables. Extract a new class from variables that go together well, especially
those will common prefixes or suffixes. Depends on the circumstances, it might
make more sense to be a subclass of the current class. If the class is a GUI
class, move data and behaviour to a separate domain class (#review p. 189 on how
to do this).

If we can't jump to a single clear point in the system and make a change, this
indicates a bad code smell. Ask yourself, do you need to change multiple methods
in a class when there is a new database or business logic introduced to the
codebase? If so, it is better to have two objects than one so that one kind of
change is isolated within a single class.

That being said, having more classes is not always a good sign. If there is a
need to make changes all over the place (to different classes), there is a need
to move the necessary methods and/or fields to a single class so that all
changes will happen on only one class. Parallel inheritance hierarchies could be
a source of [duplication](../202206171004.md)# too which we could solve it by
moving the duplicated methods and/or fields up to the hierarchy.

If a method is depending on another class and/or its methods, it is a feature
envy that we should fix. Move the method to the corresponding class as it
clearly belongs to there. If feature envy only happens in part of it, then
extract the jealous bit to a new method and move it to the designated class.
Move the method to the place where it can get more of the data it needed. Put
things together that change together. However, there are some special cases such
as Strategy Pattern, Visitor Pattern and Self Delegation (Kent Beck's Smalltalk
Best Practice Patterns).

Data clump is a group of data that always gather together. It is a sign to
create a new object to manage them as a whole. This will reduce the parameter
lists and simplify the method calling. A small class contains only two or three
fields will not be the end of the world. Primitives are not always the best
choice.

Type codes usually show a bad smell in code. Replace them with either class (if
it doesn't affect the behaviour), subclasses or [State Pattern](../202210211637.md)#
or Strategy Pattern (the latter is useful in replacing conditional codes such as
switch statement). Usually, polymorphism is the cure to the symptom, but it could
be an overkill. If they effect on a single method, replacing them with
explicit methods could do the job well. If one of the conditional cases is null,
then introduce a [Null Object Pattern](../202211162216.md)#.

A class that isn't doing much should be put into grave. Some are born from
speculative generality where we expect a case to handle, but it is not required
in reality. A hint to spot them is when the only one that exercises them is the
test case. If the method has an odd abstract name, we should rename it for more
clarity.

If there is a temporary field which only set in some circumstances, it is better
to create a class for it or introduce a null object to indicate invalidity.

There will be a case where there is a long list of message chain where one
object ask for another object, and then the object ask for yet another object.
The symptom can be spotted when we see a long line of `get()` methods. We can
collapse it by hiding it in the object that needs it the most. However, the
advice from the author is not to go too far. Make sure that the target object is
talking to the object it needed directly. If there is additional behaviour,
replace the delegation to inheritance.

If a class, especially a subclass, knowing too much information that it doesn't
need, there is a need to move some components from it. See if we can change the
relationship between two classes from bidirectional to unidirectional in UML. Or
if they have common interests, put up a new class to place their commonality or
use a delegate class act in between. For subclasses, this could be eliminated by
replacing the inheritance with delegation.

For methods that do the same thing but have different signatures for what they
do, rename them so that they appear identical. Move methods to the class until
the protocols are the same. Sometimes, it will be better to extract a superclass
from the commonality.

There are some cases where the library class doesn't include the behaviour that
you often use, introduce a new method (foreign method) to handle that case or if
there are tons of behaviours to be implemented, extend it with inheritance or
wrapper class.

Data class are great initially, but eventually we will need to add more
behaviours into it to make it act as a real object. Make sure all fields are
encapsulated within the class including the collection where getting it will
only provide a view (unmodifiable). Remove all the setters if the fields are not
meant to be changed. Look for where the getters and setters are called in other
classes, move or extract the behaviour as a method into the data class. Then, we
can hide all the getters and setters of the data class situationally.

It is rather fine to have some subclasses to know a little more information that
they don't need. However, if it is causing confusion and problems, then create a
new sibling class and push down all unused method and/or field to the sibling
where the parent will hold only the common methods. In addition, if the subclass
is reusing the behaviour but doesn't want to support the interface of the
superclass, remove the subclass from the hierarchy and use the superclass as a
delegate.

[Comment](../202202241058.md)# are usually an indicator for refactoring. If
there's a need to explain what a code does, extract it as a new method with
easily understandable name (or rename it if it already exists). If the comments
are used to state rules, use [Assertions](../202207091744.md)# to enforce it.

# Chapter 4

Tests are essential for refactoring, and it doesn't just benefit on refactoring
itself but the overall productivity as it reduces the time for debugging.
[Debugging](../202203131236.md)# is the most time-consuming practice in software
development. Fixing a bug can be quite quick, but finding it is another story.
Having tests is to minimise the chance to miss a bug or the scope where bug
could be located.

Make testing easy so that it requires little efforts to code. Make sure all
tests are fully automatic and that they check their own results. Begin feature
expansion by writing a test for it, which by default should fail. By this, we
can inspect and review what's the requirements for the feature to work and make
the test pass. It also forces us to focus on the interface rather than the
implementation.

In refactoring, we only need to run the tests that exercise the code where we
are working, which is a few, so the process will be fast.

It is advised to make a unit test that exposes the bug tracked by a newly filed
bug report.

However, Fowler didn't like the idea of testing every public method in the
codebase. He treated unit testing as risk driven; a tool to find bugs now or in
the future. If the function is so simple, it is unlikely to a bug will be there.
Writing too many tests usually leads to not writing enough as we can never reach
the 100% threshold.

He advised thinking of the boundary conditions under which things might go
wrong, which includes possible raised #exception and concentrate our tests
there. Try to intentionally break the test by specifying possible special case.

Don't treat testing as some kind of a magic that can catch all bugs in the
codebase. Treat testing as a performance boost to your productivity.
