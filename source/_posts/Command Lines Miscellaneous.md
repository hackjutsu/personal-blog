# Command Lines Miscellaneous
title: Command Lines Miscellaneous
date: 2015-10-13 18:00:00
tags: CommandLine

---

## OSX Terminal
### List all the files that a specific process opens
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

### Kill Process
#### kill
The `kill` command will kill a process using the kill signal and PID given by the user. To use the SIGKILL signal with "kill", type one of the following for a process with a PID of 1710.
``` bash
kill -9 1710
kill -SIGKILL 1710
kill -KILL 1710
```
#### killall
`killall [-SIGNAL] [process name]` - The killall command kills all process with a particular name.  Its default signal is `TERM`.

``` bash
killall firefox 
```
We can specify other specific signal such as KILL(9)
```bash
killall -9 firefox
killall -KILL fireforx
```
To check the available signals
``` bash
killall -l lists signals
```
Here is a list of available signals for reference:
`HUP` `INT` `QUIT` `ILL` `TRAP` `ABRT` `EMT` `FPE` `KILL` `BUS` `SEGV` `SYS` `PIPE` `ALRM` `TERM` `URG` `STOP` `TSTP` `CONT` `CHLD` `TTIN` `TTOU` `IO` `XCPU` `XFSZ` `VTALRM` `PROF` `WINCH` `INFO` `USR1` `USR2` 
#### pkill
This command is a lot like killall except it allows partial names. So, `pkill -9 unity` will kill any process whose name begins with "unity". `pkill` can also kill all processes owned by a particular user - `pkill -9 -u USERNAME`.
#### More...
http://www.linux.org/threads/kill-signals-and-commands-revised.8096/

----------


@(Learning Cards)[Marxico|Legacy Tools]
