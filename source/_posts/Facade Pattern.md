# Facade Pattern
title:  Facade Pattern
date: 2015-11-08 12:23:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

The Facade design pattern is often used when a system is very complex or difficult to understand because the system has a large number of interdependent classes or its source code is unavailable. This pattern hides the complexities of the larger system and provides a simpler interface to the client.
<!--more-->

## Intent
- Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
- Wrap a complicated subsystem with a simpler interface.

## Implementation
![Class Diagram of Facade Pattern](http://i.imgur.com/ZKvtLSY.png)
This is an abstract example of how a client interacts with a facade (the "computer") to a complex system (internal computer parts, like CPU and HardDrive).
``` java
// CPU.java
class CPU {
    public void freeze() {
        System.out.println("CPU: freezing...");
    }

    public void jump(long position) {
        System.out.println("CPU: jumping...");
    }

    public void execute() {
        System.out.println("CPU: executing...");
    }
}
```
``` java
// HardDrive.java
public class HardDrive {
    public byte[] read(long lba, int size) {
        System.out.println("HardDrive: reading...");
        return null;
    }
}
```
``` java
// Memory.java
public class Memory {
    public void load(long position, byte[] data) {
        System.out.println("Memory: loading... " );
    }
}
```
``` java
// ComputerFacade.java
public class ComputerFacade {
    private CPU processor;
    private Memory ram;
    private HardDrive hd;

    public ComputerFacade() {
        this.processor = new CPU();
        this.ram = new Memory();
        this.hd = new HardDrive();
    }

    public void start() {
        processor.freeze();
        ram.load(BOOT_ADDRESS, hd.read(BOOT_SECTOR, SECTOR_SIZE));
        processor.jump(BOOT_ADDRESS);
        processor.execute();
    }

    // Dummy values for simplicity
    static long BOOT_ADDRESS = 0;
    static long BOOT_SECTOR = 0;
    static int SECTOR_SIZE = 0;
}
```
``` java
// ClientMain.java
public class ClientMain {
    public static void main(String[] args) {
        ComputerFacade computer = new ComputerFacade();
        computer.start();
    }
}
```
**Output**:
> CPU: freezing...
HardDrive: reading...
Memory: loading...
CPU: jumping...
CPU: executing...

## Reference
[Wikipedia](https://en.wikipedia.org/wiki/Facade_pattern)
