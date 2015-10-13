# Java Concurrency (In Progress)

title: Java Concurrency (In Progress)
date: 2015-10-11 18:00:45
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


## Thread pools with the Executor Framework
### Runnable
**Executors framework** is used to run the `Runnable` objects without creating new threads every time and mostly re-using the already created threads. There is a **thread pool** managing a pool of worker thread. Each submittd task to the thread pool will enters a queue waiting to be executed. 

> A thread pool can be described as a collection of `Runnable` objects (work queue) and a connections of running threads. These threads are constantly running and are checking the work query for new work. If there is new work to be done they execute this Runnable. The Thread class itself provides a method, e.g. `execute(Runnable r)` to add a new `Runnable` object to the work queue.     [--- vogella Java concurrency Tutorial](http://www.vogella.com/tutorials/JavaConcurrency/article.html#threadpools)

A thread pool is represented by an instance of the class `ExecutorService`. With the `ExecutorService` instance, we can submit tasks to be executed in the future.

`Executors` provide utilities and factory methods for `ExecutorService`, for example `Executors.newFixedThreadPool(int n)` will create n worker threads. 
``` java
ExecutorService pool = Executors.newFixedThreadPool(4);

for(int i = 0; i < 10; i++){
   pool.submit(new MyRunnable());
}
```
In the codes above 10 `Runnable`instances will be submitted to a thread pool with the size 4. We are responsible to shutdown the thread pool in order to terminate all the threads, or the JVM will risk not to shutdown.
``` java
// This will make the executor accept no new threads
// and finish all existing threads in the queue
pool.shutdown();
// Wait until all threads are finish
pool.awaitTermination();
```
We can also force the shutdown of the pool using `shutdownNow()`, with that the currently running tasks will be interrupted and the tasks not started will be returned.
### Futures and Callables
The Executor framework works with a `Runnable` instance as shown above. However, `Runnable` cannot return a result to the caller. To get the computed result, Java provides the `Callable` interface. 

The `Callable` object uses generics to define the return value. 
``` java
public class MyCallable implements Callable<Integer> {
    
    @Override
    public Integer call() throws Exception {
        int sum = 0;
        for (long i = 0; i <= 100; i++) {
            sum += i;
        }
        return sum;
    }
}
```
When we submit a `Callable` instance to the thread pool, we will get a `Future` object,  which exposes methods for us to monitor the progress that the task being executed.
``` java
ExecutorService executor = Executors.newFixedThreadPool(5);
Future<Integer> future = executor.submit(new MyCallable());
int result = future.get();
```
The `Future`'s `get()`will waits if necessary for the computation to complete, and then retrieves the result. Here is a list of methods provided by `Future`:
- `boolean cancel(boolean mayInterruptIfRunning)`
	- Attempts to cancel execution of this task.
- `V get()`
	- Waits if necessary for the computation to complete, and then retrieves its result.
- `V get(long timeout, TimeUnit unit)`
	- Waits if necessary for at most the given time for the computation to complete, and then retrieves its result, if available.
- `boolean isCancelled()`
	-  Returns true if this task was cancelled before it completed normally.
- `boolean isDone()`
	- Returns true if this task completed.

> **Note:** Check out the Oracle documentations for more about [Callable](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Callable.html) and [Future](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html).

### Java 8's CompletableFuture
`CompletableFuture` extends the functionality of the `Future` interface with the possibility to notify the caller once a task is done by utilizing function-style callbacks. 

> Tutorials about `CompletableFuture` can be found here:
> https://goo.gl/RNfz61
> http://goo.gl/Y87j8A
> http://goo.gl/OoVePS

----------

## Synchornized Keyword
The Java `synchronized`keyword marks a Java block or method as synchronized to avoid race conditions. These synchronized blocks or methods only allow one thread executing their codes at one time. As [summarized by Jakob Jenkov](http://tutorials.jenkov.com/java-concurrency/synchronized.html) , the synchronized keyword can be used to mark four different types of blocs:
- Instance methods
- Static methods
- Code blocks inside instance methods
- Code blocks inside static methods

### Synchronized Instance Methods
A synchronized instance method in Java is synchronized on the instance (object) owning the method.  Only one thread can execute the synchronized method **on the same instance** at one time.
``` java
public synchronized void add(int value){
    this.count += value;
}
```
### Synchronized Static Methods
Only one thread can execute inside a static synchronized method **in the same class**.
``` java
public static synchronized void add(int value){
     count += value;
}
```
### Synchronized Blocks in Instance Methods
Sometimes, we don't need to synchronize the whole method, insteads, we can only synchronize a block of codes. 
``` java
public void add(int value){
    // Some codes before the synchronized block
    synchronized(this){
        this.count += value;   
    }
    // Some codes after the synchronized block
}
```
The object taken in the parentheses by the synchronized construct is called a monitor object. Only one thread can execute inside a Java code block synchronized on the same monitor object. In the codes below, the synchroinzed codes take `this` as the monitor object.
### Synchronized Blocks in Static Methods
Only one thread can execute inside the synchronized block **in the same class** (`MyClass.class`in the codes below).
``` java
public class MyClass {

    public static void log(String msg1, String msg2){
	    // Some codes before the synchronized block
        synchronized(MyClass.class){
            System.out.println("Hello World!");
        }
        // Some codes after the synchronized block
    }
}
```



## Thread Signaling

Comming soon...


----------


## Re-entrant Locks and Condition Variables
Comming soon...

----------


## Semaphores
Comming soon...


----------



## Blocking Queue
Comming soon...


----------

## Producer-Consumer Pattern
Comming soon...

----------


Reference:
http://tutorials.jenkov.com/java-concurrency/references.html
https://www.udemy.com/java-multithreading

@(Learning Cards)[Marxico|Java|Multi-threading]