---
title: Assuring Good Style for Object-Oriented Programs
authors: ["Karl J. Lieberherr", "Ian Holland"]
year: 1989
date: 2022-07-01 12:06
tags: [oop, literature]
---

The Law of Demeter states that each method can send messages to only a limited
set of objects: to *argument objects*, to *itself*, to objects either *created
in the method* or in *global variables*, and to the *immediate subpart of self*
(elements or object instantiated within the class). These are the
*preferred-supplier objects* of the method. 

**Note**: The self construct in Smalltalk and Flavors is called "this" in #cpp
and "Current" in Eiffel.

It is to organise and reduce dependencies (a class calls a function defined in
other class) between classes. Such practice promote maintainability and
comprehensibility of the codebase.
