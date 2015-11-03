# OO Basics and Principles

title: OO Basics and Principles
date: 2015-11-01 18:00:00
tags:
- Java
- OOP

---


`Object-oriented programming (OOP)` is a programming paradigm based on the concept of "objects", which are data structures that contain data, in the form of fields, often known as **attributes**; and code, in the form of procedures, often known as **methods**.

<!--more-->

People are sometimes confused when talking about OOP and even simply think OOP equals to Java. To clarify this issue, here is my reading notes summarized from multiple tutorials.

- [Wikipedia on OOP](https://en.wikipedia.org/wiki/Object-oriented_programming)
- [Encapsulate What Varies](http://blogs.msdn.com/b/steverowe/archive/2007/12/26/encapsulate-what-varies.aspx)
- [Program to an interface, not an implementation](http://www.fatagnus.com/program-to-an-interface-not-an-implementation/)

----------


## OO Basics

Object-oriented programming by definition uses objects, but not all of the associated techniques and structures are supported directly in languages which claim to support OOP. The features listed below are summaried from launguages considered strong OOP.

### Objects
In OO design, objects are accessed like variables with complex internal structure. They provide a layer of **abstraction** which can be used to separate internal from external code. External code can use an object by calling a specific instance method with a certain set of input parameters, read an instance variable, or write to an instance variable.

### Encapsulation
If a class disallows calling code from accessing internal object data and forces access through methods only, this is a strong form of **abstraction** or information hiding known as `encapsulation`.

This is useful for preventing the external code from being concerned with the internal workings of an object. This facilitates code **refactoring**, for example allowing the author of the class to change how objects of that class represent their data internally without changing any external code.

It also encourages programmers to put all the code that is concerned with a certain set of data in the same class, which organizes it for easy comprehension by other programmers. Encapsulation is often used as a technique for encouraging **decoupling**.

### Composition and Inheritance
- **Composition**
Objects can contain other objects in their instance variables; this is known as object composition.
- **Inheritance**
Inheritance allows classes to be arranged in a hierarchy that represents *"is-a-type-of"* relationships.

### Polymorphism
[Subtyping](https://en.wikipedia.org/wiki/Subtyping), a form of `polymorphism`, is when calling code can be agnostic as to whether an object belongs to a parent class or one of its descendants. This is another type of **abstraction**.

----------


## OO Principles
### Encapsulate Variation
When designing software, look for the portions most likely to change and prepare them for future expansion by shielding the rest of the program from that change. Hide the potential variation behind an interface. Then, when the implementation changes, software written to the interface doesn't need to change. This is called encapsulating variation

### Favor composition over inheritance
Composition over inheritance in object-oriented programming is the principle that classes should achieve polymorphic behavior and code reuse by composition (containing other classes that implement the desired functionality), instead of through inheritance (being a subclass)

Check out this [post](http://forums.oreilly.com/topic/2505-encapsulate-what-varies/) for more information.

### Program to interfaces, not implementations
The exposed part of a class is its interface. The important part here is that the interface represents “what” the class can do but not “how” it will do it, which is the actual implementation.

``` java
// Program with implementations
PepperoniPizza myPizza = new PepperoniPizza();
myPizza.Eat();

// Program with interfaces
IPizza myPizza = new PepperoniPizza();
myPizza.Eat();
```
Check out this [post](http://www.fatagnus.com/program-to-an-interface-not-an-implementation/) for more information.
