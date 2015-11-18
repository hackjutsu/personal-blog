# Bash Logical Operators
title: Bash Logical Operators
categories: Command Lines
date: 2015-11-17 16:32:09
tags:
- CommandLine
- Bash

---

In the POSIX shell grammar, the operator `&&` is referred to as `AND_IF` and `||` to `OR_IF`.
<!--more-->
Here are a couple of rules about logical operators:
- If you write `test && command`, the command will only be executed if the test succeeds.
- If you write `test || command`, the command will only be executed if the test fails.

Examples:
``` bash
$ true  || echo "Oops"  #Nothing
$ false || echo "Oops"  #Print "Oops"
$ false && echo "Oops"  #Nothing
$ true  && echo "Oops"  #Print "Oops"
```

>**Note:** Don't confuse the logical operator `||` with the pipe operator `|`. Check out [this post](http://hackjustu.ninja/2015/11/16/Bash-Redirection/) for bash piping.