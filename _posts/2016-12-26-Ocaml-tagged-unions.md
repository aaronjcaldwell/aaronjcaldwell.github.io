---
title: Tagged Unions in OCaml
layout: post
author: "Aaron Caldwell"
tags: [ocaml, functional programming]
---
In OCaml, tagged unions (also referred to as *disjoint unions, variant records or algebraic types*) are declared types containing identifiers of other types. Here's the basic syntax:

```ocaml
type typename =
  | Constructor1 of type1
  | Constructor2 of type2
  | Constructor3 of type3
  ...
  | ConstructorN of typeN
```

The first vertical bar is optional. Each type has an identifier called a constuctor which is followed by an optional type. An example might be if you constrain input to a given function to a number but you're flexible on the type. You could create the following type to categorize and eventually handle different numberical input:

```ocaml
type num =
  | Zero
  | Integer of int
  | Real of float
```

It might seem confusing at first to declare "a type made up other types" however this capability gives us some flexibility in a language where every function expects/returns a type. To declare an instance of a type, use its corresponding contstructor name in the declaration and an appropriate value for its type:

```ocaml
let realNum = Real 7.16
```

The following is a match function that might use our made up type:

```ocaml
let mult num1 num2 =
  match num1, num2 with
    | Zero, n
    | n, Zero -> Zero
    | Integer a, Integer b -> Integer (a * b)
    | Integer a, Real b
    | Real b, Integer a -> Real (float_of_int a *. b)
    | Real a, Real b -> Real (a *. b)
```

A few things to note here:

* An input where either input is Zero only requires an output of type Zero since this will be the result of any multiplication operation
* We're returning values of our made up type `num` however we could just as easily return floats, ints, strings or whatever we'd like out of this expression
* We used 'or' expressions to case match either of 2 cases where each value was a different type
