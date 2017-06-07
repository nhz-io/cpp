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

### Typings
> Haskell types are *not* first-class. Association of a value with its type is called *typing*

Examples:
* `5 :: Integer`
* `'a' :: Char`
* `inc :: Integer->Integer`
* `[1, 2, 3] :: [Integer]`
* `('b', 4) :: (Char, Integer)

The `::` is read: "has type"

### Polymorphic Types
> Types that are universally quantified in some way over all types are *polymorphic*

Example:
* `(forall a)[a]` is the family of types consisting of, for every type `a`, the type of `lists of a`
* Identifiers such as above (a, `[a]`) are called *type variables*
* `(forall a)` part can be omitted 

Valid Examples of `(forall a)[a]`:
* Lists of integers (`[1, 2, 3]`)
* Lists of characters (`['a', 'b', 'c']`)
* Lists of lists (`[[a], [b], [c]]`)

Invalid Examples of `(forall a)[a]`:
* Invalid types combination (`[2, 'b']`)

#### Haskell's type system properties
https://en.wikipedia.org/wiki/Hindley%E2%80%93Milner_type_system

* Every well-typed expression is guaranteed to have a unique principal type
* Principal type can be inferred automatically

#### Principal type
> An expression's or function's *principal type* is the least general type that, intuitively,
  "contains all instances of the expression"
 
Examples:
* `[a]->a`
* `[b]->a`
* `a->a`
* `[Integer]->Integer` - too specific
