---
title: Sturcture and Interpretation of Computer Programs
authors: ['Harold Abelson', 'Gerald Jay Sussman', 'Julie Sussman']
year: 1996
date: 2023-02-17 18:40
tags: [literature, programming-language]
---

# Chapter 1: Building Abstractions with Procedures

Programming languages should have the following three mechanisms to combine
simple ideas into more complex ideas:
- primitive expressions which represents the simplest entities
- means of combination by which compound elements are built from simpler one
- means of abstraction by which compound elements can be named and manipulated
  with

Data are stuff that we want to manipulate with. Procedures are descriptions of
the rules for manipulating the data.

In LISP (Scheme), the combination consists of operator which is at the left most
and operands which are the argument to the procedure specified by the operator.
