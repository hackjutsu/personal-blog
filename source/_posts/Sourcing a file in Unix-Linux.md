# Sourcing a file in Unix/Linux

title:  Sourcing a file in Unix/Linux
date: 2016-01-08 18:00:00
tags:
- Linux
- CommandLine
- bash

categories:
- Command Lines

---

Sourcing a file in Unix/Linux evaluates the file as a list of commands and executes them in the current context. Most people might have used this technique when updating the configurations for their terminals.

```bash
. ~/.bash_profile
source ~/.bash_profile
```

<!--more-->


----------


## What is sourcing a file?
As mentioned above, sourcing a file is to execute a file in the current context, making the changes applicable in the current shell itself. Usually, when we execute a script, a new sub-shell is created and the script is executed there. In this sense, any changes made by the script will remain in the child process. It's possible to pass these changes further to the additional grandchildren spawned by the child process, but nothing can be copied back to the parent process.

**Syntax:** (`.` is a synonym for `source`.)
```bash
. filename [arguments]
source filename [arguments]
```

>**Note:**  `./script` is not `. script`, but `. script` is equivalent to  `source script` 


----------

## Resource
[stackoverflow: How to source file from bash script](http://stackoverflow.com/questions/19621394/how-to-source-file-from-bash-script)
[stackoverflow: What does 'source' do?](http://superuser.com/questions/46139/what-does-source-do)