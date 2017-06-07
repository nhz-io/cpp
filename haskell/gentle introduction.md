# A Gentle Introduction to Haskell, Version 98 Summary

https://www.haskell.org/tutorial

## CH01 Introduction

https://www.haskell.org/tutorial/intro.html

> **typeful programming** is a part of the haskell programming experience and cannot be avoided.

## CH02 Values, Types and Other Goodies

https://www.haskell.org/tutorial/goodies.html

### Expressions
> All computations are done via the evaluation of *expressions* (syntactic terms)  
  to yield *values* (abstract entities that we regard as answers)

### Values
> Every value has an associated *type* (Intuitively, we can think of types as sets of values)

Examples:
* `5`
* `'a'`
* `\x -> x + 1`
* `[1, 2, 3]`
* `('b', 4)`

### Types
> Type expressions are syntactic terms that denote type values (or just *types*)

Examples:
* `Integer`
* `Char`
* `Integer->Integer`
* `[Integer]`
* `(Char, Integer)`
