# Abstract Factory Pattern
title:  Abstract Factory Pattern
date: 2015-11-05 00:29:00
tags:
- Design Pattern
- Java

---

The pattern is best utilized when our system has to create multiple families of products or we want to provide a library of products without exposing the implementation details.
<!--more-->
An example of an Abstract Factory in use could be UI toolkits. Across Windows, Mac and Linux, UI composites such as windows, buttons and textfields are all provided in a widget API like SWT. However, the implementation of these widgets vary across platforms. We could write a platform independent client thanks to the Abstract Factory implementation.

## Intent
- Provide an interface for creating a family of related objects, without explicitly specifying their concrete classes.
- The `new` operator considered harmful.

## Implementation

![Class Diagram of Abstract Factory Pattern](http://i.imgur.com/qRasrDZ.png)
`Window` is our AbstractProduct.
``` java
//Our AbstractProduct
public interface Window{
    public void setTitle(String text);
    public void repaint();
}
```
This is our concrete product for Microsoft Windows.
``` java
//ConcreteProductA1
public class MSWindow implements Window{
    public void setTitle(String text){
      //MS Windows specific behaviour
    }
    public void repaint(){
      //MS Windows specific behaviour
    }
}
```
This is our concrete product for Mac OSX.
``` java
//ConcreteProductA2
public class MacOSXWindow implements Window{
    public void setTitle(String text){
      //Mac OSX specific behaviour
    }
    public void repaint(){
      //Mac OSX specific behaviour
    }
}
```
Now we need to provide our AbstractFactory.
``` java
//AbstractFactory
public interface AbstractWidgetFactory{
    public Window createWindow();
}
```
Next we need to provide ConcreteFactory implementations for MS Windows.
``` java
//ConcreteFactory1
public class MsWindowsWidgetFactory implements AbstractWidgetFactory{
    //create an MSWindow
    public Window createWindow(){
        MSWindow window = new MSWindow();
        return window;
    }
}
```
And for Mac OSX.
``` java
//ConcreteFactory2
public class MacOSXWidgetFactory implements AbstractWidgetFactory{
    //create a MacOSXWindow
    public Window createWindow(){
        MacOSXWindow window = new MacOSXWindow();
        return window;
    }
}
```
Here is our client to take advantage of all this functionality.
``` java
//Client
public class Main{
    public static void main(String[] args){
        AbstractWidgetFactory widgetFactory = null;
        //check what platform we're on
        if(Platform.currentPlatform()=="MACOSX"){
            widgetFactory = new MacOSXWidgetFactory();
        } else {
            widgetFactory = new MsWindowsWidgetFactory();
        }
        Window window = widgetFactory.createWindow();
        window.setTitle("New Window");
    }
}
```
## Reference
https://dzone.com/articles/design-patterns-abstract-factory
