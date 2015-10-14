# Command Lines Miscellaneous
title: Command Lines Miscellaneous
date: 2015-10-13 18:00:00
tags: CommandLine

---

## OSX Terminal
#### List all the files that a specific process opens
This command is useful when we want to locate the log path of a specific process.
``` bash
sudo lsof -p <process id>
```
<!--more-->
 To find the process id, we can use the Activity Monitor or try the command:
``` bash
ps -A | grep <App Name>
```
 [Someone has suggested a more powerful command](http://stackoverflow.com/a/25229423/3697757):
``` bash
ps -Ac -o pid,comm | awk '/^ *[0-9]+<App Name>$/ {print $1}'
```

@(Learning Cards)[Marxico|Legacy Tools]