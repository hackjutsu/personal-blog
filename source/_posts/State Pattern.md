# State Pattern
title:  State Pattern
date: 2015-11-09 20:28:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

A monolithic object's behavior is a function of its state, and it must change its behavior at run-time depending on that state. The State Pattern is a solution to the problem of how to make behavior depend on state.
<!--more-->

## Intent
- Allow an object to alter its behavior when its internal state changes.

## Implementation
![Class Diagram of State Pattern](http://i.imgur.com/ItEp6Wx.png)

Let's use the state of an mp3 player to give an example. First we set up a `Context` for the mp3 player.
``` java
// MP3PlayerContext.java
public class MP3PlayerContext {
    private State state;

    public MP3PlayerContext() {
        this.state = new StandbyState();
    }

    public void press() {
        state.pressPlay(this);
    }

    public void setState(State state) {
        this.state = state;
    }

    public void getState() {
        System.out.println(state.getState());
    }
}
```
Here is our `State` interface, on which the `pressPlay()` imitates the play botton.
``` java
// State.java
public interface State {

    void pressPlay(MP3PlayerContext context);
    String getState();
}
```
Now, we create states for `StandbyState` and `PlayingState`.
``` java
// PlayingState.java
public class PlayingState implements State {

    public void pressPlay(MP3PlayerContext context) {
        context.setState(new StandbyState());
    }

    @Override
    public String getState() {
        return "Playing...";
    }
}
```
``` java
// StandbyState.java
public class StandbyState implements State {

    public void pressPlay(MP3PlayerContext context) {
        context.setState(new PlayingState());
    }

    @Override
    public String getState() {
        return "Stand By...";
    }
}
```
Our client presses the button to turn on the mp3, and then presses again to turn it off.
``` java
// ClientDemo.java
public class ClientDemo {

    public static void main(String[] args) {

        MP3PlayerContext mp3Player = new MP3PlayerContext();
        mp3Player.press();
        mp3Player.getState();
        mp3Player.press();
        mp3Player.getState();
    }
}
```
**Output:**
> Playing...
Stand By...

## More
### State Pattern vs Finite State Machine
According to this [stackoverflow post](http://stackoverflow.com/a/20446959/3697757), the State Pattern is more decentralized while Finite State Machine is more monolithic:

>*The way I describe this difference to my colleagues is that state patterns are a more decentralized implementation of many stand alone encapsulated states whereas state machines are more monolithic. The monolithic nature of state machines means that a single state will be harder to reuse in a different machine and that it is harder to break a state machine up into multiple compilation units. On the other hand this monolithic design allows far better optimization of state machines and allows many implementations to represent all transition information in one place in a table...*

Notice that in State Pattern, switching states requires an allocation,  which actually kills speed. Use the State Pattern in cases where speed is a not an issue.

## Reference
[Code Project](http://www.codeproject.com/Articles/509234/The-State-Design-Pattern-vs-State-Machine)
[DZone](https://dzone.com/articles/design-patterns-state)