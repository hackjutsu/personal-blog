# Load Average on Unix-like Systems

title: Load Average on Unix-like Systems
date: 2016-06-21 18:00:01
tags:
- Linux

----------

<img src="http://i.imgur.com/ydCQUFz.png" style="max-width: 550px;"/>

Linux, Mac, and other [Unix-like](http://www.howtogeek.com/182649/htg-explains-what-is-unix/) systems display “load average” numbers. These numbers tell you how busy your system’s CPU, disk, and other resources are. They’re not self-explanatory at first, but it’s easy to become familiar with them.
<!--more-->
Whether you’re using a Linux desktop or server, a Linux-based router firmware, a NAS system based on Linux or BSD, or even Mac OS X, you’ve probably seen a “load average” measurement somewhere.

> This post is adapted from the [original article](http://www.howtogeek.com/194642/understanding-the-load-average-on-linux-and-other-unix-like-systems/) on [www.howtogeek.com](www.howtogeek.com) for study purpose. Please refer to the original post for the most up-to-date information.

## Load vs Load Average
On Unix-like systems, including Linux, the system load is a measurement of the computational work the system is performing. This measurement is displayed as a number. A completely idle computer has a load average of 0. **Each running process either using or waiting for CPU resources adds 1 to the load average.** So, if your system has a load of 5, five processes are either using or waiting for the CPU.

Unix systems traditionally just counted processes waiting for the CPU, but Linux also counts processes waiting for other resources — for example, processes waiting to read from or write to the disk.

On its own, the load number doesn’t mean too much. A computer might have a load of 0 one split-second, and a load of 5 the next split-second as several processes use the CPU. Even if you could see the load at any given time, that number would be basically meaningless.

That’s why Unix-like systems don’t display the current load. They display the **load average** — an average of the computer’s load over several periods of time. This allows you to see how much work your computer has been performing.

## Finding the Load Average
The load average is shown in many different graphical and terminal utilities, including in the top command and in the graphical GNOME System Monitor tool. However, the easiest, most standardized way to see your load average is to run the `uptime` command in a terminal. This command shows your computer’s load average as well as how long it’s been powered on.

The `uptime` command works on Linux, Mac OS X, and other Unix-like systems.

![](http://www.howtogeek.com/wp-content/uploads/2014/08/650x219xtop-command-load-average-on-linux.png.pagespeed.gp+jp+jw+pj+js+rj+rp+rw+ri+cp+md.ic.LyZvGdrp9p.png)

## Understanding the Load Average
The first time you see a load average, the numbers look fairly meaningless. Here’s an example load average readout:
```plain
load average: 1.05, 0.70, 5.09
```
From left to right, these numbers show you the average load over the last one minute, the last five minutes, and the last fifteen minutes. In other words, the above output means:
```plain
load average over the last 1 minute: 1.05

load average over the last 5 minutes: 0.70

load average over the last 15 minutes: 5.09
```
The time periods are omitted to save space. Once you’re familiar with the time periods, you can quickly glance at the load average numbers and understand what they mean.

## What do the numbers mean exactly?
Let’s use the above numbers to understand what the load average actually means. Assuming you’re using a single-CPU system, the numbers tell us that:
```plain
over the last 1 minute: The computer was overloaded by 5% on average. On average, .05 processes were waiting for the CPU. (1.05)

over the last 5 minutes: The CPU idled for 30% of the time. (0.70)

over the last 15 minutes: The computer was overloaded by 409% on average. On average, 4.09 processes were waiting for the CPU. (5.09)
```
You probably have a system with multiple CPUs or a multi-core CPU. The load average numbers work a bit differently on such a system. For example, if you have a load average of 2 on a single-CPU system, this means your system was overloaded by 100 percent — the entire period of time, one process was using the CPU while one other process was waiting. On a system with two CPUs, this would be complete usage — two different processes were using two different CPUs the entire time. On a system with four CPUs, this would be half usage — two processes were using two CPUs, while two CPUs were sitting idle.

To understand the load average number, you need to know how many CPUs your system has. A load average of 6.03 would indicate a system with a single CPU was massively overloaded, but it would be fine on a computer with 8 CPUs.

The load average is especially useful on servers and embedded systems. You can glance at it to understand how your system is performing. If it’s overloaded, you may need to deal with a process that’s wasting resources, provide more hardware resources, or move some of the workload to another system.

## Resource
[Understanding the Load Average on Linux and Other Unix-like Systems](http://www.howtogeek.com/194642/understanding-the-load-average-on-linux-and-other-unix-like-systems/)

@(Learning Cards)[Linux]


