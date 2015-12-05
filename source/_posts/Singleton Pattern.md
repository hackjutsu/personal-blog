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

## Double-Checked Locking is Broken

### Background
Double-Checked Locking is widely cited and used as an efficient method for implementing lazy initialization in a multithreaded environment.

However, the following snippet won't work in the presence of either optimizing compilers or shared memory multiprocessors. 
``` java
// Broken multithreaded version
// "Double-Checked Locking" idiom
class Foo { 
    private Helper helper = null;
    public Helper getHelper() {
        if (helper == null) 
            synchronized(this) {
            if (helper == null) 
            helper = new Helper();
        }    
      return helper;
  }
  // other functions and members...
}
```
The most obvious reason it doesn't work it that the writes that initialize the `Helper` object and the write to the helper field can be done or perceived out of order. Thus, a thread which invokes `getHelper()` could see a non-null reference to a helper object, but see the default values for fields of the helper object, rather than the values set in the constructor.

There are some other reasons blocking the usages of the Double-Checked Locking. As stated in the [The "Double-Checked Locking is Broken" Declaration](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html):

>There are lots of reasons it doesn't work. The first couple of reasons we'll describe are more obvious. After understanding those, you may be tempted to try to devise a way to "fix" the double-checked locking idiom. Your fixes will not work: there are more subtle reasons why your fix won't work. Understand those reasons, come up with a better fix, and it still won't work, because there are even more subtle reasons.

>Lots of very smart people have spent lots of time looking at this. There is no way to make it work without requiring each thread that accesses the helper object to perform synchronization.

### volatile
As of J2SE 5.0, this problem has been fixed. The `volatile` keyword now ensures that multiple threads handle the singleton instance correctly.  The new Java memory model and `volatile` keyword ensures a happen-before relationship, which requires any write before the read to the volatile variable has to really happen before the read. In a nutshell, once the helper is declared as `volatile`, we won't read the `Helper` object between it is initialized and the write to its fields. Its constructor is guaranteed to be called before we read the `helper`'s value.

>Don't forget the `volatile` keyword!!

### Initialization-on-demand holder idiom
In software engineering, the Initialization on Demand Holder (design pattern) idiom is a lazy-loaded singleton. In all versions of Java, the idiom enables a safe, highly concurrent lazy initialization with good performance.
``` java
public class Something {
    private Something() {}

    private static class LazyHolder {
        private static final Something INSTANCE = new Something();
    }

    public static Something getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```
From the wikipedia on this topic:
>Since the class initialization phase is guaranteed by the JLS to be serial, i.e., non-concurrent, no further synchronization is required in the static getInstance method during loading and initialization. And since the initialization phase writes the static variable INSTANCE in a serial operation, all subsequent concurrent invocations of the getInstance will return the same correctly initialized INSTANCE without incurring any additional synchronization overhead

The semantics of Java guarantee that the field will not be initialized until the field is referenced, and that any thread which accesses the field will see all of the writes resulting from initializing that field.