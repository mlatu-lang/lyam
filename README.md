# Lean You a Mlatu for Great Good!

### About this tutorial

Welcome to **Learn You a Mlatu for Great Good!** If you're reading this, chances are you want to learn Mlatu. Well you've come to the right place.

**Learn You a Mlatu for Great Good!** is unabashedly copied off of [Learn You a Haskell for Great Good](https://learnyouahaskell.com/). This tutorial assumes you have some experience programming in an imperative programming language. If you have experience in a concatenative programming language or a functional programming language, good for you! You'll have an easier time, and should be able to follow along fairly easily.

### So what's Mlatu?

Mlatu is a **purely functional programming language**. In imperative languages you get things done by giving the computer a sequence of tasks and then it executes them. While executing them, it can change state. For instance, you set variable `a` to 5 and then do some stuff and then set it to something else. You have control flow structures for doing some action several times. In purely functional programming you don't tell the computer what to do as such but rather you tell it what stuff is. The factorial of a number is the product of all the numbers from 1 to that number, the sum of a list of numbers is the first number plus the sum of all the other numbers, and so on. You express that in the forms of functions. You also can't set a variable to something and then set it to something else later. If you say that `a` is 5, you can't say it's something else later because you just said it was `5`. What are you, some kind of liar? So in purely functional languages, a function has no side effects. The only thing a function can do is calculate something and return it as a result. At first, this seems kind of limiting but it actually has some very nice consequences: if a function is called twice with the same parameters, it's guaranteed to return the same result. That's called referential transparency and not only does it allow the compiler to reason about the program's behavior, but it also allows you to easily deduce (and even prove) that a function is correct and then build more complex functions by gluing simple functions together.

Mlatu is **concatenative**. Almost all imperative languages and functional languages are applicative. Applicative languages get stuff done by *applying* functions to their arguments, while concatenative languages get stuff done by *composing* functions. While I'll talk of applying functions in this tutorial, it's a bit of a facade because everything in Mlatu is a function, and a program is also a function formed by composing the component terms of the program. Values like `2` or `3` are really just functions that return numbers when called. In concatenative programming, the data flows in the order the program is written in, which means little-to-no backtracking when reading a concatenative programming. The best explanation I've seen for why concatenative programming matters is the aptly-named [Why Concatenative Programming Matters](https://evincarofautumn.blogspot.com/2012/02/why-concatenative-programming-matters.html).

Mlatu is **statically typed**. When you compile your program, the compiler knows which piece of code is a number, which is a string, and so on. That means a lot of possible errors are caught at compile time. If you try to add together a number and a string, the compiler will complain at you. Mlatu uses a very good type system that has `type inference`. That means that you don't have to explicitly label every piece of code with a type because the type system can intelligently figure out a lot about it. If you say `5 4 + -> a;`, you don't have to tell Mlatu that `a` is a number, it can figure that out by itself. Type inference also allows your code to be more general. If a function you make takes two parameters and adds them together and you don't explicitly state their type, the function will work on any two parameters that act like numbers.

Mlatu is **elegant and concise**. Because it uses a lot of high level concepts, Mlatu prograams are usually shorter than imperative equivalents. And shorter programs are easier to maintain than longer ones and have less bugs.

### What you need to dive in

A text editor, the Mlatu compiler, and an Erlang compiler. You probably already have your favorite text editor installed so we won't waste time on that. The easiest way to install Mlatu is to download a binary from the lastest nightly release, available [here](https://github.com/brightly-salty/mlatu/releases). The Mlatu compiler turns Mlatu programs into Erlang programs, so you will need `erlc` and `escript` to be able to run the output of the Mlatu compiler. You can install Erlang/OTP by following the instructions [here](https://erlang.org/doc/installation_guide/users_guide.html).

The Mlatu compiler can take a Mlatu program (they usually have a .mlt extension) and check it by running `mlatu check path/to/file.mlt`. This is helpful if you just want to check that the file looks okay but don't need to build it yet. To build a Mlatu program into BEAM bytecode, run `mlatu build path/to/file.mlt`. This will output a file named `mlatu.beam` that you can execute by running `escript mlatu.beam`. If you want to go ahead and run your Mlatu program without creating a BEAM file, run `mlatu run path/to/file.mlt`. Finally, if you want to format your Mlatu program so that its style is consistent with other Mlatu programs, run `mlatu fmt path/to/file.mlt` and your file will be updated with the proper formatting.

Throughout this tutorial, we will use `mlatu script INPUT` which allows you to run a program in the terminal without creating a file for your program to live in.

## Starting out

### Ready set, go!

Alright, let's get started! If you're the sort of horrible person who doesn't read introductions to things and you skipped it, you might want to read the last section in the introduction anyway because it explains what you need to follow this tutorial.

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
$ mlatu script "false"
ipt "5 5 eq"
true
$ mlatu script " 1 0 eq"
false 
$ mlatu script "5 5 neq"
false 
$ mlatu script "5 4 neq"
true
$ mlatu script "\"hello\" \"hello\" eq" 
```

What about doing `5 "llama" +` or `5 true eq`? Well, if we try the first snippet, we get a scary error message!

```shell
$ mlatu script "5 \"llama\" +"
input:1.3-10
I want to match the type "char list" with the type "nat" but I am not able to.
```

What the compiler is telling us here is that `"llama"` is not a number and so it doesn't know how to add it to 5. Even if it wasn't `"llama"` but `"four"` or `"4"`, Mlatu still wouldn't consider it to be a number. `+` expects both its inputs to be numbers. If we tried to do `true eq 5`, the mlatu compiler would tell us that the types don't match. Whereas `+` works only on numbers, `eq` works on any two things that can be compared. But the catch is that they both have to be the same type. You can't compare apples and oranges. We'll take a closer look at types a bit later.

You may not have known it but we've been using functions now all along. For instance, `*` is a function that takes two numbers and multiplies them. In other programming languages, these operators are infix functions, called like `1 * 5`. But in Mlatu, operators are no different than other functions, and are postfix.

In most imperative languages functions arecalled by writing the function name and thenwriting its parameters in parentheses, usually separated by commas. In Mlatu, functions are called by writing the parameters, separated by spaces. For a start, we'll try calling one of the most boring functions in Mlatu.

```shell
$ mlatu script "8 succ"
9
```

The `succ` function takes a number and returns the successor of that number, that is, the number incremented. As you can see, we just separate the function name from the parameter with a space. Calling a function with several parameters is also simple. The functions `min` and `max` take two things that can be put in an order (like numbers!). `min` returns the one that's lesser and `max` returns thm one that's greater. See for yourself:

```hs
$ mlatu script "9 10 min"
9
$ mlatu script "100 101 max"
101
```

Lots of people who come from imperative languages tend to stick to the notion that parentheses should denote function application. For example, in C, you use parentheses to call functions like `foo()`, `bar(1)`, or `baz(3, "haha")`. Like we said, spaces are used for function applicaation in Mlatu. So those functions in Mlatu would be `foo`, `1 bar`, and `"haha" 3 baz`.

### Baby's first functions

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
