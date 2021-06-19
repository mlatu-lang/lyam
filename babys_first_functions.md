---
layout: default
title: Baby's first functions
parent: Starting out
nav_order: 2
permalink: /babys-first-functions
---

## Baby's first functions

In the previous section we got a basic feel for calling functions. Now lets try making our own! Open up your favorite text editor and punch in this function that takes a number and multiplies it by two. Note: `dup` is a function which takes a value and returns the value, twice so we can use it more than once.

```ocaml
define double-me (nat -> nat) { dup + }
```

Functions are defined with the `define` keyword, the name of the function, the type signature of the function wrapped in parentheses, and then the body of the function wrapped in braces. Save this as `baby.mlt` or something. Now navigate to where its saved and run `mlatu check baby.mlt`, to make sure no mistakes were made.

```shell
$ mlatu check baby.mlt
$
```

To test our function, open `baby.mlt` back up again. Add a new line containing `9 double-me println` after our definition of the function, which will call `double-me` with `9` as its argument and then print the result to the terminal.

```shell
$ mlatu run baby.mlt
18
```

Success! Let's make a function that takes two numbers and multiplies each by two and then adds them together. Remember to add it to `baby.mlt` and then save the file.

```ocaml
define double-us (nat, nat -> nat) { + dup + }
9 4 double-us println
```

```shell
$ mlatu run baby.mlt 
18
26
```

Simple. Testing it out produces pretty predictable results. As expected, you can call your own functions that you made. With that in mind, we could redefine `double-us` like this:

```ocaml
define double-us (nat, nat -> nat) { + double-me }
```

As you might have noticed, we replaced `dup +` (the body of `double-me`) with a call to the function `double-me`. This is a very simple example of a common pattern you will see throughout Mlatu. Making basic functions that are obviously correct and then combining them into more complex functions. This way you also avoid repetition. What if some mathematicians figured out that 2 is actually 3 and you had to change your program? You could just redefine `double-me` to be `dup dup + +` (which produces three copies of a value and then adds them all together) and since `double-us` calls `double-me`, it would automatically work in this strange new world where 2 is 3.

Functions in Mlatu don't have to be in any particular order, so it doesn't matter if you define `double-me` first and then `double-us` or if you do it the other way around. 
Now we're going to make a function that multiplies a number by 2 but only if that number is smaller than or equal to 100 because numbers bigger than 100 are big enough as it is!

```ocaml
define double-small-number (nat -> nat) {
    dup 100 gt 
    match 
    | true { }
    | false { 2 * }
}
```

Right here we introduced Mlatu's if statement. You're probably familiar with if statements in imperative languages. The difference between Mlatu's if statement and if statements in imperative langagues is that the else part (`| false { .. }`) is mandatory in Mlatu. In imperative languages you can just skip a couple of steps if the condition isn't satisfied but in Mlatu every expression and function must return something. We could also have written that if statement in one line but I find this way more readable. Another thing about the if statement in Mlatu is that it is an *expression*. An expression is basically a piece of code that returns a value. `5` is an expression because it returns `5`, `4 8 +` is an expression, `+` is an expression because it returns the sum of the parameters given. Because the `| false { .. }` part is mandatory, an if statement will always return something and that's why it's an expression. If we wanted to add one to every number that's produced in our previous function, we could have written its body like this.

```ocaml
define double-small-number' (nat -> nat) {
    dup 100 gt 
    match 
    | true { }
    | false { 2 * }
    1 +
}
```

Note the `'` at the end of the function name. That apostrophe doesn't have any special meaning in Mlatu's syntax. It's a valid character to use in a function name. We usually use `'` to denote a slightly modified version of a function or a variable. Because `'` is a valid character in functions, we can make a function like this.

```ocaml
define conan-o'brien (-> char list) {"It's a-me, Conan O'Brien!"}
```

There are two noteworthy things here. The first is that in the function name we didn't capitalize Conan's name. That's because, by convention in Mlatu, function names use `kebab-case`, where all letters are lowercase and and spaces are replaced by `-`. The second is that this function doesn't take any parameters. Because we can't change what functions mean once we've defined them, `conan-o'brien` and the string `"It's a-me, Conan O'Brien!"` can be used interchangeably.
