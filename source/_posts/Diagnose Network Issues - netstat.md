# Diagnose Network Issues - netstat

title:  Diagnose Network Issues - netstat
date: 2015-11-25 10:58:00
tags:
- CommandLine
- OSX
- Windows

categories:
- Command Lines

---

Netstat (NETwork STATistics) is a command-line tool that provides information about our network configuration and activity. Notice that the `netstat` command behaves differently on Windows and OSX.
<!--more-->


----------


## Display routing table (Win/OSX)
``` bash
netstat -rn
```
 -r: Kernel routing tables.
 -n: Shows numerical addresses instead of trying to determine hosts.


----------


## Display all opened sockets (Win/OSX)
``` bash
netstat -a
```
-a: All


----------


## Display opened TCP/UDP socket (OSX)
``` bash
netstat -ut
```
-u: UDP
-t: TCP


----------


##  Display all listening state sockets (OSX)
### Show all listening ports
``` bash
netstat -l
```
 -l: Listening state sockets
 
### Show all TCP listening ports
``` bash
netstat -lt
```
 -l: Listening state sockets
 -t: TCP

### Show all UDP listening ports
``` bash
netstat -lu
```
 -l: Listening state sockets
 -u: UDP



----------


##  Display summary statistics for each protocol (Win/OSX)
``` bash
netstat -s
```
 -s: Summary statistics for each protocol.


----------


## Reference

[OpenManiak](http://openmaniak.com/netstat.php)
[10 basic examples of linux netstat command](http://www.binarytides.com/linux-netstat-command-examples/)
