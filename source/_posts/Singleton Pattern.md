# Singleton Pattern

title:  Singleton Pattern
date: 2015-11-01 20:00:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---


Sometimes it's important to have only one instance for a class. For example, in a system there should be only one window manager (or only a file system or only a print spooler). Usually singletons are used for centralized management of internal or external resources and they provide a global point of access to themselves.

<!--more-->

## Intent
- Ensure that only one instance of a class is created.
- Provide a global point of access to the object.

## Implementation
The implementation involves a static member in the "Singleton" class, a private constructor and a static public method that returns a reference to the static member.

Instead of marking the `getInstance()` as `synchronized`, we use “[double-checked locking](https://en.wikipedia.org/wiki/Double-checked_locking)” to reduce the use of synchronization in `getInstance()`.
``` java
public class Singleton {
    private volatile static Singleton uniqueInstance;
    // The volatile keyword makes sure multi threads handle the
    // uniqueInstance variable correctly

    private Singleton();

    public static Singleton getInstance() {
        if (uniqueInstance == null) {
        // Check for an instance and if there isn't one, enter a
        // synchronized block
            synchronized (Singleton.class) {
                if (uniqueInstance == null) {
                    // Once in the block, check again and if still null,
                    // create an instance
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

> **Note:** Double-checked locking doesn’t work in Java 1.4 or earlier!