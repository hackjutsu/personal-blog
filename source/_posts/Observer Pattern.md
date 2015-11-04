# Observer Pattern

title:  Observer Pattern
date: 2015-11-02 15:56:00
tags:
- Design Pattern
- Java

---


In some cases, we need an one-to-many relationship between objects. If one object is modified, its depenedent objects are to be notified automatically.

<!--more-->

## Intent
- Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.

## Implementation
![Class Diagram of Observer Pattern](http://i.imgur.com/RcDQFgP.png)
``` java
// Observable.java
public interface Observable {
    void registerObserver(Observer o);
    void removeObserver(Observer o);
    void notifyObservers();
}
```
``` java
// Observer.java
public interface Observer {
    void update(Data data);
}
```
``` java
// WeatherData.java
public class WeatherData implements Observable {

    public WeatherData() {
        observers = new LinkedList<>();
    }

    public void setData(Data data) {
        this.data = data;
        notifyObservers();
    }

    @Override
    public void registerObserver(Observer o) {
        observers.add(o);
    }

    @Override
    public void removeObserver(Observer o) {
        int i = observers.indexOf(o);
        if (i>=0) {
            observers.remove(i);
        }
    }

    @Override
    public void notifyObservers() {
        for(Observer observer : observers) {
            observer.update(data);
        }
    }

    private List<Observer> observers;
    private Data data;
}
```

> **Note:** Java has built-in classes/interfaces to facilitate the Observer Pattern. Check out [java.util.Observer](https://docs.oracle.com/javase/7/docs/api/java/util/Observer.html) and [java.util.Observable](https://docs.oracle.com/javase/7/docs/api/java/util/Observable.html) for more information.
