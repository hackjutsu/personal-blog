# Java Concurrency (In Progress)

title: Java Concurrency (In Progress)
date: 2015-10-10 18:00:45
tags: 
- Java
- Concurrency
- Android

---

Java is a multi threaded programming language. A multi threaded program contains two or more parts that can run concurrently and each part can handle different task at the same time making optimal use of the available resources specially when your computer has multiple CPUs.

In this post, we will discover how to write effective and efficient multi threaded program in Java.

<!--more-->

----------


## Thread Basics
### Creating and Starting Threads
Java Threads have to be instances of `java.lang.Thread` or instances of subclasses of this class. Creating and starting a thread can be simply done like this:
``` java
Thread thread = new Thread();
thread.start(); // NOT the run() method!!
```
The codes above don't have any specific codes to run. The thread stops right after it starts. To specify some logic for the new thread, we either subclass the `Thread` or pass an implementation of `java.lang.Runnable` to the `Thread`'s constructor.
#### Subclassing Thread
Subclass the `Thread` and override its `run()` method.
``` java
public class CuteThread extends Thread {

    public void run() {
        System.out.println("CuteThread is running~");
    }
}
```
To start the thread:
``` java
CuteThread cuteThread = new CuteThread();
cuteThread.start();
```
#### Implementing Runnable
``` java
Thread thread = new Thread(new Runnable() {

   public void run(){
       System.out.println("MyRunnable running");
    }
}).start();
```
Implementing `Runnable` is the [preferred way to run](http://stackoverflow.com/questions/541487/implements-runnable-vs-extends-thread) specific codes on a new thread, since we are not specializing the thread's interface.
#### Get the Current Thread
`Thread.currentThread()` returns a reference to the `Thread` instance executing the `currentThread()`.
#### Name of a Thread
We can assign a name to a Java Thread by passing it to the constructor. We can retrieve the name by calling `getName()`.
``` java
MyRunnable runnable = new MyRunnable();
Thread thread = new Thread(runnable, "New Thread");

thread.start();
System.out.println(thread.getName());
```
#### Pausing Execution with Sleep
`Thread.sleep()` causes the **current** thread to suspend execution for a specified period.
``` java
// pause for 4 seconds
Thread.sleep(4000);
```
### Joining a Thread
If `join()` is called on a `Thread` instance, the currently running thread will block until the `Thread` instance has finished executing.
``` java
// The current thread will be blocked until threadA finishes
threadA.join();
// The current thread waits at most 2000ms
threadB.join(2000);
// The current thread waits at most 2000ms + 100ns
threadC.join(2000, 100);
```
### Java Volatile Keyword
Let's look at an example. In the following codes, we start *thread_B* from *thread_A*. Then we send a stop signal from *thread_A* to *thread_B* to stop the latter thread.
``` java
class Processor extends Thread {

    private boolean running = true; // Pitfall!!

    public void run() {

        while(running) {
            System.out.println("Hello");
        
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
    
    public void shutdown() {    
        
        running = false;
    }
}

public class BasicSync {

    public static void main(String[] args) {
        
        // Start thread_B
        Processor proc = new Processor();
        proc.start();
        
        System.out.println("Please enter return key to stop...");
        Scanner scanner = new Scanner(System.in);
        scanner.nextLine();
        
        // Send shutdown signal from thread_A to thread_B
        proc.shutdown();
    }   
}
```
The program looks fine at the first glance, but actually it could fail to stop the *thread_B* depending on how the compiler optimizes the program. In some compiler, `while(running){...}` in `Processor` could be optimized as `while(true){...}`. The compiler has no idea `running` would be changed by other thread, and it optimizes `running` to `true` according to its knowledge.

To avoid this, we need to declare the `running` as `volatile`, which means the `running`variable could be changed by the codes outside and the compiler should not optimize it.
``` java
private volatile boolean running = true;
```


----------


## Threads pools with the Executor Framework


----------


## Thread Signaling


----------


## Synchornized Keyword


----------


## Re-entrant Locks and Condition Variables


----------


## Semaphores


----------



## Blocking Queue


----------


Reference:
http://tutorials.jenkov.com/java-concurrency/references.html
https://www.udemy.com/java-multithreading

@(Learning Cards)[Marxico|Java|Multi-threading]