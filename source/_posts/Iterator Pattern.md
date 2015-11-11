# Iterator Pattern
title:  Iterator Pattern
date: 2015-11-10 12:39:00
tags:
- Design Pattern
- Java

categories:
- Design Pattern

---

The Iterator Pattern is used to get a way to access the elements of a collection object in sequential manner without any need to know its underlying representation. The C++ and Java standard library abstraction utilize it to decouple collection classes and algorithms.
<!--more-->

## Intent
- Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.


## Implementation
![Class Diagram of Iterator Pattern](http://i.imgur.com/8W1rZ8v.png)

Here is an example using Iterator Pattern to retrieve the names of all the pets in the pet repository.

Create interfaces.
``` java
// Iterator.java
public interface Iterator {

    boolean hasNext();
    Object next();
}
```
``` java
// Container.java
public interface Container {

    Iterator getIterator();
}
```
Create concrete class implementing the `Container` interface. This class has inner class `NameIterator` implementing the `Iterator` interface.
``` java
// PetRepository.java
public class PetRepository implements Container {

    public String pets[] = {
            "Tyrannosaurus Rex",
            "Stegosaurus",
            "Velociraptor",
            "Triceratops",
            "Pterosauria",
            "Ichthyosaur",
            "Tanystropheus"};

    @Override
    public Iterator getIterator() {
        return new NameIterator();
    }

    private class NameIterator implements Iterator {

        int index = 0;

        @Override
        public boolean hasNext() {
            return index < pets.length;
        }

        @Override
        public Object next() {

            if (hasNext()) {
                return pets[index++];
            }
            return null;
        }
    }
}
```
Let's print out our lovely pets ;)
``` java
// ClientMain.java
public class ClientMain {

    public static void main(String[] args) {

        PetRepository pets = new PetRepository();

        for (Iterator iter = pets.getIterator(); iter.hasNext(); ) {
            String name = (String) iter.next();
            System.out.println(name);
        }
    }
}
```

**Output**
>Tyrannosaurus Rex
Stegosaurus
Velociraptor
Triceratops
Pterosauria
Ichthyosaur
Tanystropheus

## Reference
[TutorialsPoint](http://www.tutorialspoint.com/design_pattern/iterator_pattern.htm)
[OODesign](http://www.oodesign.com/iterator-pattern.html)