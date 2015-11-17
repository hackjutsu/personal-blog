# Bash Redirection

title: Bash Redirection
categories: Command Lines
date: 2015-11-16 16:08:09
tags:
- CommandLine
- Bash

---

There are 3 file descriptors: `stdin`, `stdout` and `stderr`.  `1` stands for  `stdout` and `2` for `stderr`. Basically we can:

- redirect `stdout` to a file
- redirect `stderr` to a file
- redirect `stdout` to a `stderr`
- redirect `stderr` to a `stdout`
- redirect `stderr` and `stdout` to a file

<!--more-->

## stdout 2 file
This will cause the ouput of a program to be written to a file.
``` bash
ls -l > test.txt
```

## stderr 2 file
This will cause the `stderr` ouput of a program to be written to a file.
``` bash
grep da * 2> grep-errors.txt
```

## stdout 2 stderr
This will cause the `stdout` ouput of a program to be written to the same filedescriptor of `stderr`.
``` bash
grep da * 1>&2
```

## stderr 2 stdout
This will cause the `stderr` ouput of a program to be written to the same filedescriptor of `stdout`.
``` bash
grep * 2>&1
```

## stderr and stdout 2 file
This will place every output of a program to a file. This is suitable sometimes for cron entries, if you want a command to pass in absolute silence.
``` bash
grep * &> test.txt
```