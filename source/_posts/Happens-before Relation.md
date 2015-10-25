# Happens-before Relation
title: Happens-before Relation
date: 2015-10-24 18:00:00
tags: 
- Java

---

> In computer science, the Happened-before relation is a relation between the result of two events, such that if one event should happen before another event, the result must reflect that, even if those events are in reality executed out of order. ----[Wikipedia](https://en.wikipedia.org/wiki/Happened-before)

<!--more-->
The name of "Happens-before" could be confusing.  In particular:
- A happens-before B does not imply A happening before B.
- A happening before B does not imply A happens-before B.


The Happens-before relation is **all about the codes visibility**, roughly speaking: 
> Let A and B represent operations performed by a multithreaded process. If A happens-before B, then the memory effects of A effectively become visible to the thread performing B before B is performed.

## Happens-before Order in Java

According to the [Java documentation](https://docs.oracle.com/javase/specs/jls/se7/html/jls-17.html#jls-17.4.5), two actions can be ordered by a happens-before relationship, where if one action happens before another, then the first is visible to and ordered before the second.

The Java Happens-before defines the actions relationship in various aspects of the languages, namely:

- An unlock on a monitor happens-before every subsequent lock on that monitor.
- A write to a volatile field happens-before every subsequent read of that field.
- A call to `start()` on a thread happens-before any actions in the started thread.
- All actions in a thread happen-before any other thread successfully returns from a `join()` on that thread.
- The default initialization of any object happens-before any other actions (other than default-writes) of a program.

When a program contains two conflicting accesses that are not ordered by a happens-before relationship, it is said to contain a data race.

### The Java volatile Happens-before Guarantee

Since Java 5 the `volatile` keyword guarantees more than just the reading from and writing to main memory of variables. Actually, the `volatile` keyword guarantees this:

- If Thread A writes to a `volatile` variable and Thread B subsequently reads the same `volatile` variable, then all variables visible to Thread A **before** writing the `volatile` variable, will also be visible to Thread B after it has read the `volatile` variable. 
- The reading and writing instructions of `volatile` variables cannot be reordered by the JVM (the JVM may reorder instructions for performance reasons as long as the JVM detects no change in program behaviour from the reordering). Instructions before and after can be reordered, but the `volatile` read or write cannot be mixed with these instructions. **Whatever instructions follow a read or write of a volatile variable are guaranteed to happen after the read or write.**

When a thread writes to a volatile `variable`, then not just the `volatile` variable itself is written to main memory. Also all other variables changed by the thread before writing to the volatile variable are also flushed to main memory. When a thread reads a `volatile` variable it will also read all other variables from main memory which were flushed to main memory together with the volatile variable.

The volatile keyword guarantees that all reads of a `volatile` variable are read directly from main memory, and all writes to a `volatile` variable are written directly to main memory.

#### When to use volatile?
Multiple threads could be writing to a shared `volatile` variable, and still have the correct value stored in main memory, if the new value written to the variable does not depend on its previous value. 

If a thread needs to first read the value of a `volatile` variable, and based on that value generate a new value for the shared `volatile` variable, a `volatile` variable is no longer enough to guarantee correct visibility. The short time gap in between the reading of the `volatile` variable and the writing of its new value, creates an race condition where multiple threads might read the same value of the `volatile` variable, generate a new value for the variable, and when writing the value back to main memory - overwrite each other's values. In this case, we need to use the `synchronized` keyword to guarantee the reading and writing processes atomic.

As an alternative to a `synchronized` block we could also use one of the many atomic data types found in the `java.util.concurrent package`,  for instance, the `AtomicLong` or `AtomicReference` or one of the others.

Coming back to the original question ""When to use volatile", the short answer will be:
>In the single writer - multiple reader situation a volatile variable may be enough. Using a volatile is faster than synchronization, and it doesn't lead to contention of threads trying to enter the synchronized block.

#### Performance Consideration of volatile
Reading and writing of `volatile` variables causes the variable to be read or written to main memory. Reading from and writing to main memory is more expensive than accessing the CPU cache. Accessing `volatile` variables also prevent instruction reordering which is a normal performance enhancement technique. Thus, we should only use volatile variables when you really need to enforce visibility of variables.

## Reference
https://goo.gl/prDzf4
http://goo.gl/xr8FDZ
http://goo.gl/gMg6qW
http://goo.gl/FqFW1P

----------

@(Learning Cards)[Marxico|Java]
 


