# Implementing a ConcurrentHashSet

title:  Implementing a ConcurrentHashSet
date: 2015-12-03 19:23:00
tags:
- Java
- Concurrency

---

The *java.util.concurrent* package provides a fine-designed *ConcurrentHashMap* which enables efficient concurrent access to objects in a map container. However, there is a lack of concurrent *Set* in this package.

<!--more-->
Fortunately, we can make our own *ConcurrentHashSet* based on *ConcurrentHashMap* by the following codes.

``` java
Map<K, Boolean> concurrentMap = new ConcurrentHashMap();
Set<K> concurrentHashSet = Collections.newSetFromMap(concurrentMap);
//make sure concurrentMap is empty.
```

According to *Collections.newSetFromMap()*’s [documentation](http://docs.oracle.com/javase/7/docs/api/java/util/Collections.html):

> Returns a set backed by the specified map. The resulting set displays the same ordering, concurrency, and performance characteristics as the backing map.

Therefore, our *ConcurrentHashSet* will exhibit the same concurrency properties as the underlying *ConcurrentHashMap* does. Since our set's interface doesn't have defined methods corresponding to the features *ConcurrentHashMap* brings over *Map*, like *putIfAbsent()* and numerous other new Java 8 methods like *computerIfAbsent()*, it is just a *Set* that happens to be concurrent instead of designed to be.


