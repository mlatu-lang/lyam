---
layout: default
title: What you need to dive in
parent: Introduction
nav_order: 2
permalink: /what-you-need
---

## What you need to dive in

A text editor, the Mlatu compiler, and an Erlang compiler. You probably already have your favorite text editor installed so we won't waste time on that. The easiest way to install Mlatu is to download a binary from the lastest nightly release, available [here](https://github.com/brightly-salty/mlatu/releases). The Mlatu compiler turns Mlatu programs into Erlang programs, so you will need `erlc` and `escript` to be able to run the output of the Mlatu compiler. You can install Erlang/OTP by following the instructions [here](https://erlang.org/doc/installation_guide/users_guide.html).

The Mlatu compiler can take a Mlatu program (they usually have a .mlt extension) and check it by running `mlatu check path/to/file.mlt`. This is helpful if you just want to check that the file looks okay but don't need to build it yet. To build a Mlatu program into BEAM bytecode, run `mlatu build path/to/file.mlt`. This will output a file named `mlatu.beam` that you can execute by running `escript mlatu.beam`. If you want to go ahead and run your Mlatu program without creating a BEAM file, run `mlatu run path/to/file.mlt`. Finally, if you want to format your Mlatu program so that its style is consistent with other Mlatu programs, run `mlatu fmt path/to/file.mlt` and your file will be updated with the proper formatting.

Throughout this tutorial, we will use `mlatu script INPUT` which allows you to run a program in the terminal without creating a file for your program to live in.