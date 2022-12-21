---
title: Scalable Reflection in C++
authors: Andrew Sutton, Faisal Vali, Daveed Vandevoorde
year: 2018
date: 2022-02-24 17:10
tags: [cpp, metaprogramming, literature, type-check]
---

This paper introduces the `reflexpr` operator which return the type of the
operand which is restricted to one of the following: (p. 3)
- a `type-id` which includes types with templates
- a valid `namespace-name`
- scope-qualifier token `::`
- an expression

Such information, or they call as reflection values will be stored in `info` (in
the namespace `std::meta`) as a scalar type. These values could be categorised
further into four groups: (p. 5-6)
- *Declared-entity reflections* that only designate declared-entity
- *Alias reflections* are similar to declared-entity but for aliases
- *Expression reflections* that will designate the properties of the expression
  and designate the declared-entity depends on the expression
- *Invalid reflections* which express error situations

`reflexpr` always evaluate the top most level. (p. 7)

`reflexpr` can be compared to each other using `==` and `!=` operators. For
declared-entity and alias reflections, if they are identical, using `==` will
return `true` and `!=` return `false`. However, if either both do not designate
declared entities or they designate expression properties of an expression that
is not an expression-id, their equality is unspecified. (p. 8)

The expression on `void g(int)` function is quite confusing. Is it also should
have an *expression-id* as well? (p. 8)

This paper introduces a new syntax as follows: (p. 11-12)
- `typename(reflection)` for simple type
- `namespace(reflection)` for namespace
- `template(reflection)` for template
- `unreflexpr(reflection)` will be the actual expression represented by the
  constant expression designated by `reflexpr`. If `reflexpr` designates
  non-member-function, parameters or variable, data member or an enumerator, it
  will be or similar to an id-expression that is referring to the designated
  entity.
- `(. reflection .)` will return an identifier referring to the alias or entity.
- `(< reflection >)` is only for template argument. It will try `typename`,
  `template` and `unreflexpr` in sequence to fit the well-form.

The syntax `(. reflection .)` is ugly and bad. (p. 12)

Reflections could be ranged. (p. 13-14)

There are quite a number of metafunctions to check against the expression. (p.
22-45)
