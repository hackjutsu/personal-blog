# Simple Factory
title:  Simple Factory
date: 2015-11-02 23:01:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

Simple Factory is one of most used design pattern in Java. It allows interfaces for creating objects without exposing the object creation logic to the client.
<!--more-->

## Intent
- Creates objects without exposing the instantiation logic to the client.
- Refers to the newly created object through a common interface.

## Implementation
![Class Diagram of Simple Factory](http://i.imgur.com/zYv6h8X.png)
The client needs a product, but instead of creating it directly using the new operator, it asks the factory object for a new product, providing the information about the type of object it needs.
``` java
// SimpleProductFactory.java
public class SimpleProductFactory {
    public Product createProduct(String ProductID) {
        if (id == ID1)
            return new ProductOne();
        if (id == ID2) return
        return new ProductTwo();
        ... // so on for the other Ids

        return null; //if the id doesn't have any of the expected values
    }

    ...
}
```

Sometimes, the `createProduct()` is defined as static. Defining a simple factory as a static method is a common technique and is often called a **static factory**. Why use a static method? Because you don’t need to instantiate an object to make use of the create method. But remember it also has the disadvantage that you can’t subclass and change the behavior of the create method.

