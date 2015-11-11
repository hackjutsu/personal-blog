# Strategy Pattern

title:  Strategy Pattern
date: 2015-11-01 20:01:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---


There are common situations when classes differ only in their behavior. For this cases is a good idea to isolate the algorithms in separate classes in order to have the ability to select different algorithms at runtime.

<!--more-->

## Intent
- Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.
- Capture the abstraction in an interface, bury implementation details in derived classes.

## Implementation
We are going to create a Strategy interface defining an action and concrete strategy classes implementing the Strategy interface. Context is a class which uses a Strategy.
``` java
// Strategy.java
public interface Strategy {
   public int doOperation(int num1, int num2);
}
```
``` java
// OperationAdd.java
public class OperationAdd implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 + num2;
   }
}
```
``` java
// OperationSubstract.java
public class OperationSubstract implements Strategy{
   @Override
   public int doOperation(int num1, int num2) {
      return num1 - num2;
   }
}
```
``` java
// Context.java
public class Context {
   private Strategy strategy;

   public Context(Strategy strategy){
      this.strategy = strategy;
   }

   public int executeStrategy(int num1, int num2){
      return strategy.doOperation(num1, num2);
   }
}
```
``` java
// StrategyPatternDemo.java
public class StrategyPatternDemo {
   public static void main(String[] args) {
      Context context = new Context(new OperationAdd());
      System.out.println("10 + 5 = " + context.executeStrategy(10, 5));

      context = new Context(new OperationSubstract());
      System.out.println("10 - 5 = " + context.executeStrategy(10, 5));
   }
}
```

## Reference
[TutorialsPoint](http://www.tutorialspoint.com/design_pattern/strategy_pattern.htm)