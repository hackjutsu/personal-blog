# Autoboxing and Unboxing

title:  Autoboxing and Unboxing
date: 2016-01-12 18:00:01
tags:
- Java

---

<img src="http://i.imgur.com/tuNnNbs.png" style="max-height: 280px;"/>
Autoboxing and unboxing is introduced in Java 1.5 to automatically change the primitive type into the wrapper class and vice verse. With this feature, we can use primitives(`int`, `double`, `float`...) and wrapper classes(`Integer`, `Double`, `Float`...) in many places interchangeably.
<!--more-->

The following table lists the primitive types and their corresponding wrapper classes, which are used by the Java compiler for autoboxing and unboxing:

| Primitive type | Wrapper class |
| :-------: | :--------:|
| boolean   | Boolean   |
| byte      | Byte      |
| char      | Character |
| float	    | Float     |
| int	    | Integer   |
| long	    | Long      |
| short	    | Short     |
| double	| Double    |


----------


## Autoboxing and Unboxing Examples
### Autoboxing
Autoboxing is the automatic conversion that the Java compiler makes to change primitive types to their corresponding object wrapper classes. Here is an example for autoboxing.
```java
List<Integer> li = new ArrayList<>();
for (int i = 1; i < 50; i += 2) {
    li.add(i);
}
```
According to the [Java Docs](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html), The Java compiler applies autoboxing when a primitive value is:
- Passed as a parameter to a method that expects an object of the corresponding wrapper class.
- Assigned to a variable of the corresponding wrapper class.

### Unboxing
Unboxing is the opposite process of autoboxing.
```java
Integer myWrapperInt = 13;
int myPrimitive = myWrapperInt;
```
The Java compiler applies unboxing when an object of a wrapper class is:
- Passed as a parameter to a method that expects a value of the corresponding primitive type.
- Assigned to a variable of the corresponding primitive type.


----------



## Caveats
Autoboxing and unboxing lets developers write cleaner code, making it easier to read, however there are some caveats we need to understand before utilizing them in production code.

### Unnecessary Object creation due to Autoboxing
As shown in the example from [Javarevisited](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html),  autoboxing could throw away object which gets created if autoboxing occurs in a loop. This could potentially slow down the system with frequent garbage collection.
```java
Integer sum = 0;
for(int i=1000; i<5000; i++){
    sum+=i;
    // Integer sum = new Integer(sum.intValue() + i);
}
```
Refer to [Javarevisited](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html) for more details.

### Complicated method overloading
As discussed in  [Javarevisited](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html), we need method overloading to distinguish `value(int)` and `value(Integer)` since autoboxing/unboxing will potentially introduce subtle bugs if only either method exists.

For example, `ArrayList.remove()` is overloaded by `remove(index)` and `remove(Object)`, so that autoboxing/unboxing won't occur to confuse us by mixing removing object by index and removing object itself (e.g. especially when `Integer` is the object).

### Tricky "==" operator
I would like to borrow the example again from [Javarevisited](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html). More details can be found on the original post.
```java
public class AutoboxingTest {

    public static void main(String args[]) {

        // Example 1: == comparison pure primitive – no autoboxing
        int i1 = 1;
        int i2 = 1;
        System.out.println("i1==i2 : " + (i1 == i2)); // true

        // Example 2: equality operator mixing object and primitive
        Integer num1 = 1; // autoboxing
        int num2 = 1;
        System.out.println("num1 == num2 : " + (num1 == num2)); // true

        // Example 3: special case - arises due to autoboxing in Java
        Integer obj1 = 1; // autoboxing will call Integer.valueOf()
        Integer obj2 = 1; // same call to Integer.valueOf() will return same
                          // cached Object
        System.out.println("obj1 == obj2 : " + (obj1 == obj2)); // true

        // Example 4: equality operator - pure object comparison
        Integer one = new Integer(1); // no autoboxing
        Integer anotherOne = new Integer(1);
        System.out.println(
                "one == anotherOne : " + (one == anotherOne)); // false
    }
}
```
Here is the output.
```bash
i1==i2 : true
num1 == num2 : true
obj1 == obj2 : true
one == anotherOne : false
```

I would like to put the insightful explanation from the [original post](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html) here.
>In first example both argument of == operator is primitive int type so no autoboxing occurs and since `1==1` it prints true.
>
>While in second example during assignment to num1, autoboxing occurs which converts primitive 1 into `Integer(1)` and when we compare `num1==num2` unboxing occurs and `Integer(1)` is converted back to 1 by calling Integer.intValue() method  and since `1==1` result is true.
>
>In Third example which is a corner case in autoboxing, both Integer object are initialized automatically due to autoboxing and since `Integer.valueOf()` method is used to convert int to Integer and it caches object ranges from -128 to 127, it returns same object both time. In short obj1 and obj2 are pointing to same object and when we compare two object with == operator it returns true without any autoboxing.
>
>In last example object are explicitly initialized and compared using equality operator , this time == return false because both one and anotherOne reference variables are pointing to different object.


----------



## Resource
[Oracle Docs](https://docs.oracle.com/javase/tutorial/java/data/autoboxing.html)
[Javarevisited Tutorial: What is Autoboxing and Unboxing in Java](http://javarevisited.blogspot.com/2012/07/auto-boxing-and-unboxing-in-java-be.html)
[Javarevisited Tutorial: Why Comparing Integer using == in Java 5 is Bad?](http://javarevisited.blogspot.sg/2010/10/what-is-problem-while-using-in.html)

