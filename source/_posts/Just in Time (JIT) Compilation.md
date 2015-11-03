# Just in Time (JIT) Compilation

title: Just in Time (JIT) Compilation
date: 2015-10-02 18:00:45
tags: Java


----------


> In computing, just-in-time (JIT) compilation, also known as dynamic translation, is compilation done during execution of a program – at run time – rather than prior to execution. [-- Wikipedia](https://en.wikipedia.org/wiki/Just-in-time_compilation)

A JIT compiler runs after the program has started and compiles the code (usually bytecode or some kind of VM instructions) **on the fly** (or just-in-time, as it's called) into a form that's usually faster, typically the host CPU's native instruction set.

<!-- more -->

A JIT has access to dynamic runtime information whereas a standard compiler doesn't and can make better optimizations like inlining functions that are used frequently. This is in contrast to a traditional compiler that compiles all the code to machine language before the program is first run ([Ahead of Time, AOT](https://en.wikipedia.org/wiki/Ahead-of-time_compilation)).

### JIT Example
- In Java  JIT is in JVM (Java virtual machine)
- In C#, it is in .Net framework
- In Android, it is in DVM (Dalvik virtual machine)

### Why is Java faster when using a JIT?
Here's an interesting answer given by the [James Gosling](https://en.wikipedia.org/wiki/James_Gosling) himself in the Book *[Masterminds of Programming](http://www.amazon.com/Masterminds-Programming-Conversations-Creators-Languages-ebook/dp/B0043D2EEU/ref=sr_1_1?ie=UTF8&qid=1443843588&sr=8-1&keywords=mastermind%20of%20programming)*.
> **Well, I’ve heard it said that effectively you have two compilers in the Java world. You have the compiler to Java bytecode, and then you have your JIT, which basically recompiles everything specifically again. All of your scary optimizations are in the JIT.**

> **James:** Exactly. These days we’re beating the really good C and C++ compilers pretty much always. When you go to the dynamic compiler, you get two advantages when the compiler’s running right at the last moment. `One is you know exactly what chipset you’re running on.` So many times when people are compiling a piece of C code, they have to compile it to run on kind of the generic x86 architecture. Almost none of the binaries you get are particularly well tuned for any of them. You download the latest copy of Mozilla,and it’ll run on pretty much any Intel architecture CPU. There’s pretty much one Linux binary. It’s pretty generic, and it’s compiled with GCC, which is not a very good C compiler.

> When HotSpot runs, it knows exactly what chipset you’re running on. It knows exactly how the cache works. It knows exactly how the memory hierarchy works. It knows exactly how all the pipeline interlocks work in the CPU. It knows what instruction set extensions this chip has got. It optimizes for precisely what machine you’re on. `Then the other half of it is that it actually sees the application as it’s running. It’s able to have statistics that know which things are important. It’s able to inline things that a C compiler could never do.` The kind of stuff that gets inlined in the Java world is pretty amazing. Then you tack onto that the way the storage management works with the modern garbage collectors. With a modern garbage collector, storage allocation is extremely fast.

### HotSpot and JIT
JVM is a specification. Different vendors implement the spec. Eg: Sun (now Oracle), IBM, BEA (now Oracle), SAP all implement the specification and provide their own JVMs. Sun's specific implementation is called Hotspot. BEA's is called JRockit.

JIT is part of the JVM which takes Java bytecodes and compiles it to the native processor assembly code on the machine where the program is running. Each vendor implements the JIT leveraging unique sophisticated algorithms. For eg: JIT on JRockit is different from Hotspot's JIT.

There is a pretty nice explanation from [IBM Knowledge Center](https://www-01.ibm.com/support/knowledgecenter/SSYKE2_7.0.0/com.ibm.java.aix.71.doc/diag/understanding/jit_overview.html):
> In practice, methods are not compiled the first time they are called. For each method, the JVM maintains a call count, which is incremented every time the method is called. The JVM interprets a method until its call count exceeds a JIT compilation threshold. Therefore, often-used methods are compiled soon after the JVM has started, and less-used methods are compiled much later, or not at all. The JIT compilation threshold helps the JVM start quickly and still have improved performance. The threshold has been carefully selected to obtain an optimal balance between startup times and long term performance.