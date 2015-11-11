# Proxy Pattern
title:  Proxy Pattern
date: 2015-11-08 13:59:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

Proxy Pattern is designed to address the issue where you might not want to instantiate a resource-hungry objects unless and until they are actually requested by the client.

According to the [SourceMaking's design pattern tutorial,](https://sourcemaking.com/design_patterns/proxy) there are four common situations when Proxy Pattern is applicable:
<!--more-->
1. A virtual proxy is a **placeholder** for "expensive to create" objects. The real object is only created when a client first requests/accesses the object.

2. A remote proxy provides a **local representative** for an object that resides in a different address space. This is what the "stub" code in RPC and CORBA provides.

3. A protective proxy **controls access** to a sensitive master object. The "surrogate" object checks that the caller has the access permissions required prior to forwarding the request.

4. A smart proxy interposes **additional actions** when an object is accessed. Typical uses include:
 - Counting the number of references to the real object so that it can be freed automatically when there are no more references (aka smart pointer),
 - Loading a persistent object into memory when it's first referenced,
 - Checking that the real object is locked before it is accessed to ensure that no other object can change it.

<!--more-->

## Intent
- Provide a surrogate or placeholder for another object to control access to it.
- Use an extra level of indirection to support distributed, controlled, or intelligent access.
- Add a wrapper and delegation to protect the real component from undue complexity.

## Implementation
![Class Diagram of Proxy Pattern](http://i.imgur.com/4was628.png)
The following Java example illustrates the "virtual proxy" pattern. The `ProxyImage` class is used to access a remote method.

The example creates first an interface against which the pattern creates the classes. This interface contains only one method to display the image, called `displayImage()`, that has to be coded by all classes implementing it.

The proxy class `ProxyImage` is running on another system than the real image class itself and can represent the real image `RealImage` over there. The image information is accessed from the disk. Using the proxy pattern, the code of the `ProxyImage` avoids multiple loading of the image, accessing it from the other system in a memory-saving manner. It should be noted, however, that the lazy loading demonstrated in this example is not part of the proxy pattern, but is merely an advantage made possible by the use of the proxy.
``` java
// Image.java
public interface Image {
   void display();
}
```
``` java
// RealImage.java
public class RealImage implements Image {

   private String fileName;

   public RealImage(String fileName){
      this.fileName = fileName;
      loadFromDisk(fileName);
   }

   @Override
   public void display() {
      System.out.println("Displaying " + fileName);
   }

   private void loadFromDisk(String fileName){
      System.out.println("Loading " + fileName);
   }
}
```
``` java
// ProxyImage.java
public class ProxyImage implements Image{

   private RealImage realImage;
   private String fileName;

   public ProxyImage(String fileName){
      this.fileName = fileName;
   }

   @Override
   public void display() {
      if(realImage == null){
         realImage = new RealImage(fileName);
      }
      realImage.display();
   }
}
```
``` java
// ProxyPatternDemo.java
public class ProxyPatternDemo {

   public static void main(String[] args) {
      Image image = new ProxyImage("test_10mb.jpg");

      //image will be loaded from disk
      image.display();
      System.out.println("");

      //image will not be loaded from disk
      image.display();
   }
}
```
**Outputs:**
> Loading test_10mb.jpg
Displaying test_10mb.jpg
Displaying test_10mb.jpg

## More
- Adapter provides a different interface to its subject. Proxy provides the same interface. Decorator provides an enhanced interface.
- Decorator and Proxy have different purposes but similar structures. Both describe how to provide a level of indirection to another object, and the implementations keep a reference to the object to which they forward requests.

## Reference
[Wikipedia](https://en.wikipedia.org/wiki/Proxy_pattern#Java)
[SourceMaking](https://sourcemaking.com/design_patterns/proxy)

