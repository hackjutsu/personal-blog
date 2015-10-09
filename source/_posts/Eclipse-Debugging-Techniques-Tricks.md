# Eclipse Debugging Techniques & Tricks
title: Eclipse Debugging Techniques & Tricks
date: 2015-10-08 18:00:00
tags: 
- Java
- Debug

---

### *Resume* and *Terminate*

When we start debugging, **Eclipse** will suggest you entering the `Debug Perspective`. The `Break Points View` lies on the top-right corner of the `Debug Perspective`.

If we put a break point at *Line 8*, then the process will highlight and pause at *Line 8*. Note that *Line 8* has not been executed yet at this moment.

- `Resume (F8)` will resume the process until it hits the next break point.
- `Terminate (Command + F2)` will terminate a debugging process.

<!--more-->
![](./Debug_Perspective.png)

----------


### *Step Over*, *Step Into* and *Step Return*

- `Step Over (F6)`: Execute current and pause at the next line.
- `Step Into (F5)`: Go into methods.
- `Step Return (F7)`: Return from the current method and go a level back.

---

### *Step into Selection*

``` java
int firstPersonAge = DataUtil.getPersonData().get(0).getAge();
```

For the chained methods above, if we want to debug the `getPersonData()`, we can use the `Step Into` method to get inside. However, if we want to debug the `getAge()`, then `Step Into` won't help. To achieve our goal, select the method `getAge()`, right click and choose `Step Into Selection`. Note that we can also achieve this by putting a break point inside `getAge()`.

![](./Step_Into_Selection.png) 


----------


### Evaluating Expression using *Inspect*, *Execute* and *Watch*
- `inspect`: right click and choose inspect, superset of `execute`. We can even inspect an expression before our debug reach that line, if all the variable it needs are already defined.
- `execute`: right click and choose execute, will execute the selected expression. 
- `watch`: right click and choose watch, will create a customized expression in the "Expression" tab on top right corner. 


----------


### *Show Logical Structures*
`Show logcial structures` is a switch on the `Break Points View`. Without this tunred on, the view will show up the internal representations of the collection objects. With it tunred on, the view will show up its children information will be far more useful. 

![](./Show_Logical_View.png) 
*Left: before, Right: after*

----------

### Changing of Variable's Values during Debugging
Simply double click the variables on the break point windows and input the new values we want. 

![](./Changing_Variable_Values.png)


----------

### Utilize the *Display View*
The `Display View` displays the result of evaluating an expression in the context of the current stack frame. We can evaluate and display a selection either from the editor or directly from the `Display View`.  
> Windows --> Show View --> Display

![](./Display_View.png)


----------

### Conditional Breakpoint and Hit Count

The debug process pause at a conditional break point *only when it meets the break point's condition*. 
> Right click the break point --> Break Point Properties... (Command + Double Click)

For example, the following codes pause at the break point only when the `Person` instance's country equals to US.

![](./Conditional_Break_Point.png)

By checking the `Hit count` box in the `Line Breakpoint` panel above, we can specify the "hit count". For example, if we set it as 6, the process will not pause for the first 5 valid iterations.

----------

@(Learning Cards)[Marxico|Legacy Tools|Java]