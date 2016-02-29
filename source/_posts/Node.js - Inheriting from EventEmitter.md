# Node.js - Inheriting from EventEmitter

title:  Node.js - Inheriting from EventEmitter
date: 2016-01-01 18:00:01
tags:
- Javascript
- Node.js


----------


I recently came across [metamatt](http://stackoverflow.com/users/275581/metamatt)'s answer about how to inherit from the `EventEmitter` on [StackOverflow](http://stackoverflow.com/a/23918654/3697757).  Matt provides a concise snippet showing good practice for Javascript object inheritance.  I would like to reproduce his answer in my blog.

<!--more-->

----------

To inherit from another Javascript object, Node.js's `EventEmitter` in particular but really any object in general, you need to do two things:

- provide a constructor for your object, which completely initializes the object; in the case that you're inheriting from some other object, you probably want to delegate some of this initialization work to the super constructor.
- provide a prototype object that will be used as the [[proto]] for objects created from your constructor; in the case that you're inheriting from some other object, you probably want to use an instance of the other object as your prototype.

This is more complicated in Javascript than it might seem in other languages because Javascript separates object behavior into "constructor" and "prototype". These concepts are meant to be used together, but can be used separately.

Javascript is a very malleable language and people use it differently and there is no single true definition of what "inheritance" means.

In many cases, you can get away with doing a subset of what's correct, and you'll find tons of examples to follow  that seem to work fine for your case.

For the specific case of Node.js's `EventEmitter`, here's what works:
```javascript
var EventEmitter = require('events').EventEmitter;
var util = require('util');

// Define the constructor for your derived "class"
function Master(arg1, arg2) {
   // call the super constructor to initialize `this`
   EventEmitter.call(this);
   // your own initialization of `this` follows here
};

// Declare that your class should use EventEmitter as its prototype.
// This is roughly equivalent to: Master.prototype = Object.create(EventEmitter.prototype)
util.inherits(Master, EventEmitter);
```

Possible foibles:

- If you set the prototype for your subclass (Master.prototype), with or without using `util.inherits`, but don't call the super constructor (`EventEmitter`) for instances of your class, they won't be properly initialized.
- If you call the super constructor but don't set the prototype, `EventEmitter` methods won't work on your object
- You might try to use an initialized instance of the superclass (`new EventEmitter`) as `Master.prototype` instead of having the subclass constructor Master call the super constructor `EventEmitter`; depending on the behavior of the superclass constructor that might seem like it's working fine for a while, but is not the same thing (and won't work for `EventEmitter`).
- You might try to use the super prototype directly (`Master.prototype = EventEmitter.prototype`) instead of adding an additional layer of object via Object.create; this might seem like it's working fine until someone monkeypatches your object Master and has inadvertently also monkeypatched `EventEmitter` and all its other descendants. Each "class" should have its own prototype.

Again: to inherit from `EventEmitter` (or really any existing object "class"), you want to define a constructor that chains to the super constructor and provides a prototype that is derived from the super prototype.

## Resource
[StackOverflow: Node.js - inheriting from EventEmitter](http://stackoverflow.com/questions/8898399/node-js-inheriting-from-eventemitter)
