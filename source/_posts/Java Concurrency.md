# Java Concurrency

title: Java Concurrency
date: 2016-01-20 20:15:45
tags:
- Java
- Concurrency

---

<img src="http://i.imgur.com/LOBPL1M.jpg" style="max-height: 350px;"/>

Java is a multi threaded programming language. A multi-threaded program contains two or more parts that can run concurrently and each part can handle different task at the same time making optimal use of the available resources specially when your computer has multiple CPUs.

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

### Yielding a Thread
According to the Java documentation, `yield()` is:
> a hint to the scheduler that the current thread is willing to yield its current use of a processor.

Let's use the following snippet as an example.
``` java
public class HelloWorld {

    public static void main(String[] args) throws InterruptedException {
        Thread myThread = new Thread() {
            public void run() {
                System.out.println("Hello from new thread");
            }
        };
        myThread.start();
        Thread.yield();
        System.out.println("Hello from main thread");
        myThread.join();
    }
}
```
Without the call to `yield()`, the startup overhead of the new thread would mean that the main thread would almost certainly get to its `println()` first, although it is not guaranteed to be the case.

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

> A thread pool can be described as a collection of *Runnable* objects (work queue) and a connections of running threads. These threads are constantly running and are checking the work query for new work. If there is new work to be done they execute this Runnable. The Thread class itself provides a method, e.g. *execute(Runnable r)* to add a new *Runnable* object to the work queue. [-- By vogella Java Tutorial](http://www.vogella.com/tutorials/JavaConcurrency/article.html#threadpools)

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
- ***boolean cancel(boolean mayInterruptIfRunning)***
	- Attempts to cancel execution of this task.
- ***V get()***
	- Waits if necessary for the computation to complete, and then retrieves its result.
- ***V get(long timeout, TimeUnit unit)***
	- Waits if necessary for at most the given time for the computation to complete, and then retrieves its result, if available.
- ***boolean isCancelled()***
	-  Returns true if this task was cancelled before it completed normally.
- ***boolean isDone()***
	- Returns true if this task completed.

> **Note:** Check out the Oracle documentations for more about [Callable](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Callable.html) and [Future](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/Future.html).

### Java 8's CompletableFuture
`CompletableFuture` extends the functionality of the `Future` interface with the possibility to notify the caller once a task is done by utilizing function-style callbacks.

Tutorials about `CompletableFuture` can be found here:
- [Java Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/CompletableFuture.html)
- [Functional-Style Callbacks Using Java 8's CompletableFuture](http://www.infoq.com/articles/Functional-Style-Callbacks-Using-CompletableFuture)
- [Java 8: CompletableFuture in action](http://www.javacodegeeks.com/2013/05/java-8-completablefuture-in-action.html)

----------

## Synchornized Keyword
The Java `synchronized`keyword serves as Java's intrinsic locks. It marks a Java block or method as `synchronized` to avoid race conditions. These synchronized blocks or methods only allow one thread executing their codes at one time. As [summarized by Jakob Jenkov](http://tutorials.jenkov.com/java-concurrency/synchronized.html) , the `synchronized` keyword can be used to mark four different types of blocks:
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
Note that declaring a method as synchronized is just syntactic sugar for surround the method's body with the following:
``` java
synchronized(this) {
    <<method body>>
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
As the name suggested, thread signaling should enable threads to send signals to each other. At the same time, it should also allow threads to wait signals from other threads.

### Busy Waiting
The most intuitive way for thread signaling is let threads send signals to and retrieve signals from a shared object.
``` java
public class SharedSignal{

    protected boolean mShouldContinue = false;

    public synchronized boolean shouldContinue(){
        return mShouldContinue;
    }

    public synchronized void setShouldContinue(boolean continue){
        mShouldContinue = continue;
    }
}
```
Thread A could do a busy waiting for Thread B to signal the `SharedSignal` object.
``` java
protected SharedSignal signal = new SharedSignal();
// Some codes here

while(!signal.shouldContinue()) {
// busy waiting
}
```
### wait(), notify() and notifyAll()
The busy waiting consumes the CPU while waiting, which is not very efficient. Java `Object` has a built-in mechanism for a more smarter wait. The thread will sleep while waiting until some other thread sends a signal to wait it up.

`Object` defines three methods `wait()`, `notify()` and `notifyAll()` to facilitate this smart wait.

A thread that calls `wait()` on any object becomes inactive until another thread calls `notify()` on that object. In order to call either `wait()` or notify the calling thread must first obtain the lock on that object. In other words, the calling thread must call `wait()` or `notify()` from inside a `synchronized` block.

Once a thread calls `wait()` it **releases** the lock it holds on the monitor object. Once a thread is awakened it cannot exit the `wait()` call until the thread calling `notify()` has left its `synchronized` block.  If multiple threads are awakened using `notifyAll()` only one awakened thread at a time can exit the `wait()` method, since each thread must obtain the lock on the monitor object in turn before exiting `wait()`.

[Here](http://stackoverflow.com/a/2536999/3697757) is a good example illustrating this mechanism:

``` java
class MyHouse {
    private boolean pizzaArrived = false;

    public void eatPizza() {
        synchronized(this) {
            while(!pizzaArrived) { // Never do if(!pizzaArrived)
                wait();
            }
        }
        System.out.println("yumyum..");
    }

    public void pizzaGuy() {
        synchronized(this) {
             this.pizzaArrived = true;
             notifyAll();
             // Stick with notifyAll() until you know what you are doing
        }
    }
}
```
The post lists some important points:
- Always use `while(!pizzaArrized)` insteads of `if(!pizzaArrived)` to avoid the suspicious wake up.
- We must hold the lock (synchronized) before invoking wait/nofity. Threads also have to acquire lock before waking.
- Try to avoid acquiring any lock within your synchronized block and strive to not invoke alien methods (methods you don't know for sure what they are doing). If you have to, make sure to take measures to avoid deadlocks.
- Be careful with notify(). Stick with `notifyAll()` until you know what you are doing.
- Last, but not least, read [Java Concurrency in Practice](http://www.amazon.com/gp/product/0321349601?ie=UTF8&tag=none0b69&linkCode=as2&camp=1789&creative=9325&creativeASIN=0321349601)!

>  **Note:**  Don't call `wait()` on constant Strings or global objects!!  The JVM/Compiler internally translates constant strings into the same object.

----------

## Re-entrant Locks and Condition Variables
In Java 5.0, a new addition called `ReentrantLock` was made to enhance intrinsic locking capabilities. Prior to this,  `synchronized` and `volatile` were the means for achieving concurrency.

### Re-entrant Locks and synchroinzed
The `synchronized` uses intrinsic locks or monitors, [this article](https://dzone.com/articles/what-are-reentrant-locks) gives insightful comparation between the intrinsic locking mechanism and the Re-eantrant lock mechanism.  In short......

> The main difference between `synchronized` and `ReentrantLock` is ability to trying for lock interruptibly, and with timeout.

`ReentrantLock` is a concrete implementation of `Lock` interface.  It is mutual exclusive lock, similar to implicit locking provided by `synchronized` keyword in Java, with extended feature like **fairness**, which can be used to provide lock to longest waiting thread. Lock is acquired by `lock()` method and held by Thread until a call to `unlock()` method. **Fairness**  parameter is provided while creating instance of `ReentrantLock` in constructor. `ReentrantLock` provides same visibility and ordering guarantee, provided by implicitly locking, which means, `unlock()` happens before another thread get `lock()`.

### Re-entrant Locks Example
Here is a example using Re-entrant lock to increment the counter.  [Check out the original post here :)](http://javarevisited.blogspot.com/2013/03/reentrantlock-example-in-java-synchronized-difference-vs-lock.html)
``` java
public class ReentrantLockTest {

    private final ReentrantLock lock = new ReentrantLock();
    private int count = 0;

    //Locking using Lock and ReentrantLock
    public int getCount() {
        lock.lock();
        try {
            System.out.println(
                    Thread.currentThread().getName() + " gets Count: " + count);
            return count++;
        } finally {
            lock.unlock();
        }
    }

    public static void main(String args[]) {
        final ReentrantLockTest counter = new ReentrantLockTest();
        Thread t1 = new Thread() {

            @Override
            public void run() {
                while (counter.getCount() <= 6) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException ex) {
                        ex.printStackTrace();
                    }
                }
            }
        };

        Thread t2 = new Thread() {

            @Override
            public void run() {
                while (counter.getCount() <= 6) {
                    try {
                        Thread.sleep(100);
                    } catch (InterruptedException ex) {
                        ex.printStackTrace();
                    }
                }
            }
        };

        t1.start();
        t2.start();
    }
}
```
Note that since the lock is not automatically released when the method exits, you should wrap the `lock()` and the `unlock()` methods in a `try/finally` clause.

### Conditional Variable

The `Condition` interface factors out the `java.lang.Object` monitor methods `wait()/notify()/notifyAll()` into distinct objects to give the effect of having multiple wait-sets per object, by combining them with the use of arbitrary `Lock` implementations. Where `Lock` replaces `synchronized` methods and statements, `Condition` replaces `Object` monitor methods.

> **Note:** The main difference between *synchroinzed/wait/notify* and *Lock* is *Lock* API isn't block bound and we can have many groups of *wait/notify* by using many *Condition* instances.

Here is a sample given by the Java document:
``` java
class BoundedBuffer {
    final Lock lock = new ReentrantLock();
    final Condition notFull = lock.newCondition();
    final Condition notEmpty = lock.newCondition();

    final Object[] items = new Object[100];
    int putptr, takeptr, count;

    public void put(Object x) throws InterruptedException {
        lock.lock();
        try {
            while (count == items.length)
                notFull.await();
            items[putptr] = x;
            if (++putptr == items.length)
                putptr = 0;
            ++count;
            notEmpty.signal();
        } finally {
            lock.unlock();
        }
    }

    public Object take() throws InterruptedException {
        lock.lock();
        try {
            while (count == 0)
                notEmpty.await();
            Object x = items[takeptr];
            if (++takeptr == items.length)
                takeptr = 0;
            --count;
            notFull.signal();
            return x;
        } finally {
            lock.unlock();
        }
    }
}
```

----------


## Semaphores
> **Note:** [Jakob Jenkov](http://jakob.jenkov.com/) gives a good [tutorial](http://tutorials.jenkov.com/java-util-concurrent/semaphore.html) on the *Semaphore*.  I would like to adapt his tutorial here. For more information, please refer to [the original post](http://tutorials.jenkov.com/java-util-concurrent/semaphore.html).

The `java.util.concurrent.Semaphore` class is a counting semaphore. That means that it has two main methods:

- acquire()
- release()

The counting semaphore is initialized with a given number of "permits". For each call to `acquire()` a permit is taken by the calling thread. For each call to `release()` a permit is returned to the semaphore. Thus, at most N threads can pass the `acquire()` method without any `release()` calls, where N is the number of permits the semaphore was initialized with. The permits are just a simple counter.

### Semaphore Usage
As semaphore typically has two uses:
- To guard a critical section against entry by more than N threads at a time.
- To send signals between two threads.

#### Guarding Critical Sections
If we use a semaphore to guard a critical section, the thread trying to enter the critical section will typically first try to acquire a permit, enter the critical section, and then release the permit again after. Like this:
``` java
Semaphore semaphore = new Semaphore(1);

//critical section
semaphore.acquire();

...

semaphore.release();
```
#### Sending Signals Between Threads
If we use a semaphore to send signals between threads, then we would typically have one thread call the `acquire()` method, and the other thread to call the `release()` method.

If no permits are available, the `acquire()` call will block until a permit is released by another thread. Similarly, a `release()` calls is blocked if no more permits can be released into this semaphore.

### Fairness
No guarantees are made about fairness of the threads acquiring permits from the Semaphore. That is, there is no guarantee that the first thread to call acquire() is also the first thread to obtain a permit.

To enforce fairness, the `Semaphore` class has a constructor that takes a boolean telling if the semaphore should enforce fairness.

``` java
Semaphore semaphore = new Semaphore(1, true);
```

> **Note:** Enforcing fairness comes at a performance / concurrency penalty, so don't enable it unless you need it.

----------

## Blocking Queue

[BlockingQueue](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/BlockingQueue.html) is a queue interface which is thread safe to insert or retrieve elements from it, which is a nice candidate for concurrent development. Here is an [example](http://examples.javacodegeeks.com/core-java/util/concurrent/java-blockingqueue-example/) about utilizing BlockingQueue for a Producer-Consumer pattern.

|   Methods |    Throws Exception | Special Value |Blocks|Times Out|
|:---------:|:-------------------:|:-------------:|:----:|:-------:|
| Insert    |add(o)|offer(o)|put(o)|offer(o, timeout, timeunit)|
| Remove    |remove(o)|poll(o)|take()|poll(timeout, timeunit)|
| Examine   |element()|peek()| N/A | N/A |

- **Throws Exception**
If the attempted operation is not possible immediately, an exception is thrown.
- **Special Value**
If the attempted operation is not possible immediately, a special value is returned (often true / false).
- **Blocks**
If the attempted operation is not possible immedidately, the method call blocks until it is.
- **Times Out**
If the attempted operation is not possible immedidately, the method call blocks until it is, but waits no longer than the given timeout. Returns a special value telling whether the operation succeeded or not (typically true / false).

----------

## ConcurrentHashMap
`ConcurrentHashMap` performs better than  `Hashtable` or `synchronized Map` because it only locks a **portion** of Map.

In the book Clean Code, the author [claims the ConcurrentHashMap performs better than HashMap in nearly all situations.](http://stackoverflow.com/questions/6692008/java-concurrenthashmap-is-better-than-hashmap-performance-wise)

> When Java was young Doug Lea wrote the seminal book *Concurrent Programming in Java*. Along with the book he developed several thread-safe collection, which later became part of the JDK in the  *java.util.concurrent* package. The collections in that package are safe for multithreaded situations and they perform well. **In fact, the  ConcurrentHashMap implementation performs better than HashMap in nearly all situations.** It also allows for simultaneous concurrent reads and writes, and it has methods supporting common composite operations that are otherwise not thread safe. If Java 5 is the deployment environment, start with  *ConcurrentHashMap*.

Check out [here](http://javarevisited.blogspot.com/2013/02/concurrenthashmap-in-java-example-tutorial-working.html) and [here](http://javarevisited.blogspot.com/2011/04/difference-between-concurrenthashmap.html) for more information.

----------

## Atomic Class
Here is a small toolkit of classes that support lock-free thread-safe programming on single variables. Check out this [link](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/package-summary.html) for more atomic classes.

| Class      | Description |
| :--------: | :--------:|
| [AtomicBoolean](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/AtomicBoolean.html)    |   A boolean value that may be updated atomically. |
| [AtomicInteger](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/AtomicInteger.html)    |   A int value that may be updated atomically. |
| [AtomicIntegerArray](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/AtomicIntegerArray.html)    |   An int array in which elements may be updated atomically.|
| [AtomicLong](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/AtomicLong.html)    |   A long value that may be updated atomically.|
| [AtomicLongArray](http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/atomic/AtomicLongArray.html)    |   A long array in which elements may be updated atomically.|


----------


## Reference
[Java Concurrency References](http://tutorials.jenkov.com/java-concurrency/references.html)

