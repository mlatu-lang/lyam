---
layout: default
title: An intro to lists
parent: Starting out
nav_order: 3
permalink: /intro-lists
---

## An intro to lists

Much like shopping lists in the real world, lists in Mlatu are very useful. It's the most used data structure and it can be used in a multitude of different ways to model and solve a whole bunch of problems. Lists are SO awesome. In this section we'll look at the basics of lists and strings (which are lists).

In Mlatu, lists are a *homogoenous* data structure. It stores several elements of the same type. That means that we can have a list of numbers or a list of characters but we can't have a list that has a few integers and then a few characters. And now, a list!

```shell
$ mlatu script "[4, 8, 15, 16, 23, 42]"
[4, 8, 15, 16, 23, 42]
```

As you can see, lists are denoted by square brackets and the values in the lists are separated by commas. If we tried a list like `[1,2,'a',3,'b','c',4]` Mlatu would complain that characters (which are, by the way, denoted as a character between single quotes) are not numbers. Speaking of characters, strings are just lists of characters. `"hello"` is just syntactic sugar for `['h', 'e', 'l', 'l', 'o']`. Because strings are lists, we can use list functions on them, which is really handy.

A common task is putting two lists together. This is done by using the `append` function.

```shell
$ mlatu script "[1,2,3,4] [9,10,11,12] append"
[1,2,3,4,9,10,11,12]  
$ mlatu script "\"hello\" \" \" append \"world\""
"hello world"
$ mlatu script "['w', 'o'] ['o', 't'] append"
"woot"
```

Watch out when repeatedly using the `append` function on long strings. When you put together two lists (even if you append a single element to a list, for instance: `[1,2,3] [4] append`), internally, Mlatu has to walk through the whole list of the leftmost parameter of `append`. That's not a problem when dealing with lists that aren't too big. But putting something at the end of a list that's fifty million entries long is going to take a while. However putting something at the beginning of a list using the `cons` function is instantaneous.

```shell
$ mlatu script "'A' \" SMALL CAT\" cons"
"A SMALL CAT"
$ mlatu script "5 [1,2,3,4,5] cons"
[5,1,2,3,4,5]
```

Notice how `cons` takes a number and a list of numbers or a character and a list of characters, whereas `append` takes two lists. Even if you're adding an element to the end of a list with `append`, you have to surround it with square brackets so it becomes a list.

`[1,2,3]` is actually just syntactic sugar for `1 2 3 nil cons cons cons`. `nil` is an empty list. If we prepend `3` to it, it becomes `[3]`. If we prepend `2` to that, it becomes `[2,3]`, and so on.

Lists can be compared if the stuff they contain can be compared. When using `eq`, `neq`, `lt`, `le`, `gt`, and `ge` to compare lists, they are compared in lexicographical order. First the heads are compared. If they are equal then the second elements are compared, etc.

```shell
$ mlatu script "[3,2,1] [2,1,0] gt"
true 
$ mlatu script "[3,2,1] [2,10,100] gt"
true 
$ mlatu script "[3,4,2] [3,4] gt" 
true 
$ mlatu script "[3,4,2] [2,4] gt"
true 
$ mlatu script "[3,4,2] [3,4,2] eq"
true
```

What else can you do with lists? Here are some basic functions that operate on lists.

`hd` takes a list and returns its head. The head of a list is basically its first element. If the list is empty, `hd` returns `none`.

```shell
$ mlatu script "[5,4,3,2,1] hd" 
5 some
```

`tl` takes a list and returns its tail. In other words, it chops off a list's head. If the list is empty, `tl` returns `none`.

```shell
$ mlatu script "[5,4,3,2,1] tl"
[4,3,2,1] some
```

`last` takes a list and returns its last element. If the list is empty, `last` returns `none`.

```shell
$ mlatu script "[5,4,3,2,1] last"
1 some
```

`init` takes a list and returns everything except its last element. If the list is empty, `init` returns `none`.

```shell
$ mlatu script "[5,4,3,2,1] init"
[5,4,3,2] some
```

`length` takes a list and returns its length, obviously.

```shell
$ mlatu script "[5,4,3,2,1] length"
5
```

`empty` checks if a list is empty. If it is, it returns `true`, otherwise it returns `false`. It's better to use `empty` rather than `[] eq` or `nil eq`.

```shell
$ mlatu script "[1,2,3] empty"
false 
$ mlatu script "[] empty"
true
```

`rev` reverses a list.

```shell
$ mlatu script "reverse [5,4,3,2,1]"
[1,2,3,4,5]
```

`mem` takes a thing and a list of things and tells us if that thing is an element of the list.

```shell
$ mlatu script "4 [3,4,5,6] mem"
true 
$ mlatu script "10 [3,4,5,6] mem"
false
```