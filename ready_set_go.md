---
layout: default
title: Ready, set, go!
parent: Starting out
nav_order: 1
permalink: /ready-set-go
---

## Ready, set, go!

You can start by opening your terminal. Here's some simple arithmetic.

```shell
$ mlatu script "2 15 +"
17
$ mlatu script "49 100 *"
4900
$ mlatu script "1892 1472 -"
420 
$ mlatu script "5 2 /"
2
```

This is pretty self-explanatory. Note that Mlatu uses Reverse Polish Notation, so all operators are postfix, used after their number arguments. Also note that `/` is integer division, and will return the result of division, rounded down to the nearest whole number.

```shell
$ mlatu script "50 100 * 4999 -"
1 
$ mlatu script "(50 100 *) 4999 -"
1 
$ mlatu script "4999 100 - 50 *"
244950
```

Boolean algebra is also pretty straightforward. As you could probably guess, `and` means a boolean *and*, `or` means a boolean *or*, `not` negates a `true` or `false`. Just like the operators `+`, `*`, `-`, and `/`, the functions `and`, `or`, and `not` are written after their arguments.

```shell
$ mlatu script "true false and"
false 
$ mlatu script "true true and"
true 
$ mlatu script "false true or"
true 
$ mlatu script "false not"
true 
$ mlatu script "true true and not"
false
```

Testing for equality is done like so. (Note that we have to escape the quotation mark when using `mlatu script`)

```shell
$ mlatu script "5 5 eq"
true
$ mlatu script "1 0 eq"
false 
$ mlatu script "5 5 neq"
false 
$ mlatu script "5 4 neq"
true
$ mlatu script "\"hello\" \"hello\" eq" 
true
```

What about doing `5 "llama" +` or `5 true eq`? Well, if we try the first snippet, we get a scary error message!

```shell
$ mlatu script "5 \"llama\" +"
input:1.3-10
I want to match the type "char list" with the type "nat" but I am not able to.
```

What the compiler is telling us here is that `"llama"` is not a number and so it doesn't know how to add it to 5. Even if it wasn't `"llama"` but `"four"` or `"4"`, Mlatu still wouldn't consider it to be a number. `+` expects both its inputs to be numbers. If we tried to do `true 5 eq`, the mlatu compiler would tell us that the types don't match. Whereas `+` works only on numbers, `eq` works on any two things that can be compared. But the catch is that they both have to be the same type. You can't compare apples and oranges. We'll take a closer look at types a bit later.

You may not have known it but we've been using functions now all along. For instance, `*` is a function that takes two numbers and multiplies them. In other programming languages, these operators are infix functions, called like `1 * 5`. But in Mlatu, operators are no different than other functions, and are postfix.

In most imperative languages functions arecalled by writing the function name and thenwriting its parameters in parentheses, usually separated by commas. In Mlatu, functions are called by writing the parameters, separated by spaces. For a start, we'll try calling one of the most boring functions in Mlatu.

```shell
$ mlatu script "8 succ"
9
```

The `succ` function takes a number and returns the successor of that number, that is, the number incremented. As you can see, we just separate the function name from the parameter with a space. Calling a function with several parameters is also simple. The functions `min` and `max` take two things that can be put in an order (like numbers!). `min` returns the one that's lesser and `max` returns thm one that's greater. See for yourself:

```shell
$ mlatu script "9 10 min"
9
$ mlatu script "100 101 max"
101
```

Lots of people who come from imperative languages tend to have the notion that parentheses should denote function application. For example, in C, you use parentheses to call functions like `foo()`, `bar(1)`, or `baz(3, "haha")`. As mentioned earlier, spaces are used for function application in Mlatu. So those functions in Mlatu would be `foo`, `1 bar`, and `"haha" 3 baz`.
