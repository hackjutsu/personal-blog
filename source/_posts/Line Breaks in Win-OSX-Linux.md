# Line Breaks in Win/OSX/Linux
title:  Line Breaks in Win/OSX/Linux
date: 2015-12-07 17:36:00
categories:
- Command Lines
tags:
- Linux
- OSX
- Windows
- bash
- CommandLine

----------


In computing, a newline, also known as a line ending, end of line (EOL), or line break, is a special character or sequence of characters signifying the end of a line of text and the start of a new line. 

<!-- more -->

The concepts of `line feed (LF)` and `carriage return (CR)` are closely associated, and can be either considered separately together. 

The line feed indicates that one line of paper should feed out of the printer thus instructed the printer to advance the paper one line, and a carriage return indicated that the printer carriage should return to the beginning of the current line.

> `Line Feed(LF)` is represented as  `\n`, `0x0A` in hexadecimal and `10` in decimal.
> `Carriage Return(CR)` is represented as  `\r`, `0x0D` in hexadecimal and `13` in decimal.

The actual codes representing a newline vary across operating systems, which can be a problem when exchanging text files between systems with different newline representations.
- **Windows**, and **DOS** before it, uses a pair of CR and LF characters to terminate lines (`0x0D0A`).
- **UNIX** (Including Linux and FreeBSD) uses an LF character only (`0x0A`).
- **OSX** also uses a single LF character(`0x0A`), but the classic Mac operating system used a single CR character for line breaks(`0x0D`).

## Conversions

### DOS to UNIX  
Removing CRs on Linux and BSD based OS that haven't GNU extensions.
``` bash
# awk
awk '{gsub("\r",""); print;}' inputfile > outputfile
# cat
cat inputfile | tr -d "\r" > outputfile
# sed
sed -e 's/\r$//' inputfile > outputfile
```

### Unix to DOS
Adding CRs on Linux and BSD based OS that haven't GNU extensions.
``` bash
# awk
awk '{sub("$","\r\n"); printf("%s",$0);}' inputfile > outputfile
# sed. Not work for OSX because OSX uses a older version of sed.
sed -e 's/$/\r/' inputfile > outputfile 
```

## Resource
[Hex Fiend, a fast and clever open source hex editor for Mac OS X.](http://ridiculousfish.com/hexfiend/)
[StackOverflow](http://stackoverflow.com/questions/6373888/converting-newline-formatting-from-mac-to-windows)
