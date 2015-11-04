# Decorator Pattern
title:  Decorator Pattern
date: 2015-11-04 00:29:00
tags:
- Design Pattern
- Java

---

Extending an objects functionality can be done statically (at compile time) by using inheritance however it might be necessary to extend an objects functionality **dynamically** (at runtime) as an object is used.
<!--more-->

## Intent
- Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
- Client-specified embellishment of a core object by recursively wrapping it.

## Implemantation
![Class Diagram of Decorator Pattern](http://i.imgur.com/n89NTaF.png)
Below is an interface depicting an icecream
``` java
// Icecream.java
public interface Icecream {
    public String makeIcecream();
}
```
This is the base class on which the decorators will be added.
``` java
// SimpleIcecream.java
public class SimpleIcecream implements Icecream {

    @Override
    public String makeIcecream() {
        return "Base Icecream";
    }
}
```
Following class is the decorator class. It is the core of the decorator design pattern. It contains an attribute for the type of interface. Instance is assigned dynamically at the creation of decorator using its constructor. Once assigned that instance method will be invoked.
``` java
// IcecreamDecorator.java
abstract class IcecreamDecorator implements Icecream {

    protected Icecream specialIcecream;

    public IcecreamDecorator(Icecream specialIcecream) {
        this.specialIcecream = specialIcecream;
    }

    public String makeIcecream() {
        return specialIcecream.makeIcecream();
    }
}
```
These are two decorators, concrete class implementing the abstract decorator. When the decorator is created the base instance is passed using the constructor and is assigned to the super class.
``` java
// NuttyDecorator.java
public class NuttyDecorator extends IcecreamDecorator {

    public NuttyDecorator(Icecream specialIcecream) {
        super(specialIcecream);
    }

    public String makeIcecream() {
        return specialIcecream.makeIcecream() + addNuts();
    }

    private String addNuts() {
        return " + cruncy nuts";
    }
}
```
``` java
// HoneyDecorator.java
public class HoneyDecorator extends IcecreamDecorator {

    public HoneyDecorator(Icecream specialIcecream) {
        super(specialIcecream);
    }

    public String makeIcecream() {
        return specialIcecream.makeIcecream() + addHoney();
    }

    private String addHoney() {
        return " + sweet honey";
    }
}
```
**Execution of the decorator pattern**
We can use as many decorators in any order we want. This excellent flexibility and changing the behaviour of an instance of our choice at runtime is the main advantage of the decorator design pattern.
``` java
// TestDecorator.java
public class TestDecorator {

    public static void main(String args[]) {
        Icecream icecream = new HoneyDecorator(new NuttyDecorator(new SimpleIcecream()));
        System.out.println(icecream.makeIcecream());
    }
}
```
**Output**
``` bash
Base Icecream + cruncy nuts + sweet honey
```

## Decorator Pattern in Java API
The `java.io` classes are based on Decorator.
![Head First Design Pattern ---- Decorator Pattern](http://i.imgur.com/e1FtVTr.png)

## Reference
http://javapapers.com/design-patterns/decorator-pattern/