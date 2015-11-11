# Command Pattern

title:  Command Pattern
date: 2015-11-07 11:36:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---


The Command Pattern issues requests to objects without knowing about the operation being requested or the receiver of the request. It wraps a request under an object as command and passes it to the invoker object. This sounds like a `callback` in Javascript, as the Gang of Four later says:
> Commands are an object-oriented replacement for callbacks.

<!--more-->

## Intent
- Encapsulate a request in an object.
- Allows the parameterization of clients with different requests.
- Allows saving the requests in a queue.

## Implementation

![Class Diagram of Command Pattern](http://i.imgur.com/xEaYStJ.png)


The `Receiver` object knows how to perform the actions.
```java
// ReceiverA.java
public class ReceiverA {

    public void doAction() {
        System.out.println("ReceiverA do action!!");
    }

    public void undoAction() {
        System.out.println("ReceiverA undo action!!");
    }
}
```
Another `Reciever` implementation.
``` java
// ReceiverB.java
public class ReceiverB {

    public void doAction() {
        System.out.println("ReceiverB do action!!");
    }

    public void undoAction() {
        System.out.println("ReceiverB undo action!!");
    }
}
```
The Command interface defines methods for operations. Some applications require us to log all the actions and be able to recover after a crash by reinvoking the logged actions. The Command Pattern can support this with the addition of two methods: `store()` and `load()`.
```java
// Command.java
public interface Command {

    void execute();
    void undo();
    // void store();// Optional method to log the executed commands.
    // void load();// Optional method to load the logged commands.
}
```
The `ConcreteCommand` extends the `Command` interface, implementing the `execute()` method by invoking the corresponding operations on `Receiver`. It defines a link between the `Receiver` and the command action.
```java
// CommandA.java
public class CommandA implements Command{

    public CommandA(ReceiverA receiverA) {
        this.receiverA = receiverA;
    }

    @Override
    public void execute() {
        receiverA.doAction();
    }

    @Override
    public void undo() {
        receiverA.undoAction();
    }

    private ReceiverA receiverA;
}
```
Another `ConcreteCommand` implementation.
```java
// CommandB.java
public class CommandB implements Command {

    public CommandB(ReceiverB receiverB) {
        this.receiverB = receiverB;
    }

    @Override
    public void execute() {
        receiverB.doAction();
    }

    @Override
    public void undo() {
        receiverB.undoAction();
    }

    private ReceiverB receiverB;
}
```
The `NoCommand` object is an example of a null object. A null object is useful when we don't have a meaningful object to return, and yet we want to remove the responsibility of handling **null** from the client. Sometimes, this is called [Null Object Pattern](http://www.tutorialspoint.com/design_pattern/null_object_pattern.htm).
```java
// NoCommand.java
public class NoCommand implements Command {
    @Override
    public void execute() {
        System.out.println("No Command!");
    }

    @Override
    public void undo() {
        System.out.println("No Command!");
    }
}
```
`Invoker` asks the command to carry out the request.
```java
// Invoker.java
public class Invoker {

    public Invoker() {
        commands = new Command[2];
        undoCommand = new NoCommand();
    }

    public void setCommand(int slot, Command command) {
        //We don't check the range of slot for simplicity
        commands[slot] = command;
    }

    public void onButtonWasPushed(int slot) {
        //We don't check the range of slot for simplicity
        commands[slot].execute();
        undoCommand = commands[slot];
    }

    public void undoButtonWasPushed() {
        undoCommand.undo();
    }

    private Command[] commands;
    private Command undoCommand;
}
```
Here is our client class.
``` java
// ClientMain.java
public class ClientMain {

    static public void main(String[] args) {
        CommandA commandA = new CommandA(new ReceiverA());
        CommandB commandB = new CommandB(new ReceiverB());

        Invoker invoker = new Invoker();
        invoker.setCommand(0, commandA);
        invoker.setCommand(1, commandB);

        invoker.onButtonWasPushed(0);
        invoker.onButtonWasPushed(1);
        invoker.undoButtonWasPushed();
        invoker.onButtonWasPushed(1);
    }
}
```
**Output:**
> ReceiverA do action!!
ReceiverB do action!!
ReceiverB undo action!!
ReceiverB do action!!

## Java 8
Since Java 8, we can simply the Command Pattern by using the lambda expression `() -> {}` and Method Refrences.

For the `Command` interface,  we cannot define two methods with the same types when using lamdba expression.

```java
// Command.java
public interface Command {
    void execute();
    // void undo();
}
```
``` java
// ClientMain.java
public class ClientMain {

    static public void main(String[] args) {

        ReceiverA receiverA = new ReceiverA();
        ReceiverB receiverB = new ReceiverB();

        Invoker invoker = new Invoker();
        invoker.setCommand(0, () -> {receiverA.doAction();}); // Lambda
     // invoker.setCommand(0, () -> receiverA.doAction());
        invoker.setCommand(1, receiverB::doAction); // method reference

        invoker.onButtonWasPushed(0);
        invoker.onButtonWasPushed(1);
        invoker.onButtonWasPushed(1);
    }
}
```
Check out this post for more information about [lambda expression](http://tutorials.jenkov.com/java/lambda-expressions.html) and [method reference](https://blog.idrsolutions.com/2015/02/java-8-method-references-explained-5-minutes/).


## Reference
[OO Design](http://www.oodesign.com/command-pattern.html)
[Game Programming Patterns](http://gameprogrammingpatterns.com/command.html)