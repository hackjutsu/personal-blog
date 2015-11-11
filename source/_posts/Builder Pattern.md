# Builder Pattern

title:  Builder Pattern
date: 2015-11-07 18:37:00
tags:
- Design Pattern
- Java
categories:
- Design Pattern

---

The Builder Pattern has been describe by the Gang of Four:
> The builder pattern is a design pattern that allows for the step-by-step creation of complex objects using the correct sequence of actions. The construction is controlled by a director object that only needs to know the type of object it is to create.

<!--more-->

It solves the problem when the increase of object constructor parameter combination leads to an exponential list of constructors.  Instead of using numerous constructors, the builder pattern uses another object, a builder, that receives each initialization parameter step by step and then returns the resulting constructed object at once.

## Intent
- Separate the construction of a complex object from its representation so that the same construction process can create different representations.
- Parse a complex representation, create one of several targets.

## Implementation
![Class Diagram of Builder Pattern](http://i.imgur.com/xM5PhmB.png)
Here is an example from [Wikipedia](https://en.wikipedia.org/wiki/Builder_pattern):
``` java
public class StreetMap {
    private final Point origin;
    private final Point destination;

    private final Color waterColor;
    private final Color landColor;
    private final Color highTrafficColor;
    private final Color mediumTrafficColor;
    private final Color lowTrafficColor;

    public static class Builder {
        // Required parameters
        private final Point origin;
        private final Point destination;

        // Optional parameters - initialize with default values
        private Color waterColor = Color.BLUE;
        private Color landColor = new Color(30, 30, 30);
        private Color highTrafficColor = Color.RED;
        private Color mediumTrafficColor = Color.YELLOW;
        private Color lowTrafficColor = Color.GREEN;

        public Builder(Point origin, Point destination) {
            this.origin = origin;
            this.destination = destination;
        }

        public Builder waterColor(Color color) {
            waterColor = color;
            return this;
        }

        public Builder landColor(Color color) {
            landColor = color;
            return this;
        }

        public Builder highTrafficColor(Color color) {
            highTrafficColor = color;
            return this;
        }

        public Builder mediumTrafficColor(Color color) {
            mediumTrafficColor = color;
            return this;
        }

        public Builder lowTrafficColor(Color color) {
            lowTrafficColor = color;
            return this;
        }

        public StreetMap build() {
            return new StreetMap(this);
        }
    }

    private StreetMap(Builder builder) {
        // Required parameters
        origin = builder.origin;
        destination = builder.destination;

        // Optional parameters
        waterColor = builder.waterColor;
        landColor = builder.landColor;
        highTrafficColor = builder.highTrafficColor;
        mediumTrafficColor = builder.mediumTrafficColor;
        lowTrafficColor = builder.lowTrafficColor;
    }

    public static void main(String args[]) {
        Point origin = new Point(10, 10);
        Point destination = new Point(50, 50);
        StreetMap map = new StreetMap.Builder(origin, destination)
                .landColor(Color.GRAY)
                .waterColor(Color.BLUE.brighter())
                .build();
    }
}
```

## Reference
[Wikipedia](https://en.wikipedia.org/wiki/Builder_pattern)