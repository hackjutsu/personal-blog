# Composite Pattern
title:  Composite Pattern
date: 2015-11-09 13:20:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

The composite pattern describes that a group of objects (composite) is to be treated in the same way as a single instance of an object (primitive). Having to query the "type" of each object before attempting to process it is not desirable.
<!--more-->

## Intent
- Compose objects into tree structures to represent whole-part hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
- Recursive composition.
- One-to-many **"HAS A"** up the **"IS A"** hierarchy.

## Implementation
![Class Diagram of Composite Pattern](http://i.imgur.com/25GqcXi.png)

In a small organization, there are 5 employees. At top position, there is one general manager. Under general manager, there are two employees, one is manager and other is developer and further manager has two developers working under him. We want to print name and salary of all employees from top to bottom.

First we will create component interface. It has all common operation that will be applicable to both manager and developer.
``` java
// Employee.java
public interface Employee {

    void add(Employee employee);
    void remove(Employee employee);
    Employee getChild(int i);

    String getName();
    double getSalary();
    void print();
}
```
Now we will create manager(`Composite` class).
``` java
public class Manager implements Employee {

    private String name;
    private double salary;

    public Manager(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    List<Employee> employees = new ArrayList<Employee>();

    public void add(Employee employee) {
        employees.add(employee);
    }

    public Employee getChild(int i) {
        return employees.get(i);
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public void print() {
        System.out.println();
        System.out.println("Name =" + getName());
        System.out.println("Salary =" + getSalary());

        Iterator<Employee> employeeIterator = employees.iterator();
        while (employeeIterator.hasNext()) {
            Employee employee = employeeIterator.next();
            employee.print();
        }
    }

    public void remove(Employee employee) {
        employees.remove(employee);
    }
}
```
We will create developer class. This class is `Leaf` node so all operations related to accessing children will be empty as it has no children.
``` java
// Developer.java
public class Developer implements Employee {

    private String name;
    private double salary;

    public Developer(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void add(Employee employee) {
        //this is leaf node so this method is not applicable to this class.
    }

    public Employee getChild(int i) {
        //this is leaf node so this method is not applicable to this class.
        return null;
    }

    public String getName() {
        return name;
    }

    public double getSalary() {
        return salary;
    }

    public void print() {
        System.out.println();
        System.out.println("Name =" + getName());
        System.out.println("Salary =" + getSalary());
    }

    public void remove(Employee employee) {
        //this is leaf node so this method is not applicable to this class.
    }
}
```
``` java
// DemoMain.java
public class DemoMain {

    public static void main(String[] args) {
        Employee emp1 = new Developer("John", 10000);
        Employee emp2 = new Developer("David", 15000);
        Employee manager1 = new Manager("Daniel", 25000);
        manager1.add(emp1);
        manager1.add(emp2);
        Employee emp3 = new Developer("Michael", 20000);
        Manager generalManager = new Manager("Mark", 50000);
        generalManager.add(emp3);
        generalManager.add(manager1);
        generalManager.print();
    }
}
```
**Output**:
>Name =Mark
Salary =50000.0

>Name =Michael
Salary =20000.0

>Name =Daniel
Salary =25000.0

>Name =John
Salary =10000.0

>Name =David
Salary =15000.0

## More
Here is a good discussion about whether the `Composite` class and the `Leaf` class should implement the same `Component` interface from [SourceMaking](https://sourcemaking.com/design_patterns/composite):

>*... Being able to treat a heterogeneous collection of objects atomically (or transparently) requires that the "child management" interface be defined at the root of the Composite class hierarchy (the abstract Component class). However, this choice costs you safety, because clients may try to do meaningless things like add and remove objects from leaf objects. On the other hand, if you "design for safety", the child management interface is declared in the Composite class, and you lose transparency because leaves and Composites now have different interfaces. ... Composite doesn't force you to treat all Components as Composites. It merely tells you to put all operations that you want to treat "uniformly" in the Component class. If add, remove, and similar operations cannot, or must not, be treated uniformly, then do not put them in the Component base class...*

## Reference
[Wikipedia](https://en.wikipedia.org/wiki/Composite_pattern)
[SourceMaking](https://sourcemaking.com/design_patterns/composite)