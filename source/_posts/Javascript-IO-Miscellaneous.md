title: Javascript I/O Miscellaneous
date: 2015-09-26 23:10:54
tags: Javascript
---
In this section we deal with how Javascritp handles files both synchronously and asynchronously. The following topics will get involved.
- Event Driven Non-Blocking I/O in V8 Javascript
- Binary Data, Character Sets, and Encodings
- Buffer
- Callbacks
- Files and fs
- Streams
- Pipes

<!-- more -->

#### Event Driven Non-Blocking I/O in V8 Javascript
Javascript running in `V8`(which is embedded in the `Node`) is synchronous, it can only handle one thing at one time. However, the entire process of `Node` is asynchronous, because there are things happening in `libuv` and `V8` at the same time and all of these are inside `Node` itself. 

When we are talking about `Node`, we will encounter a headline "**Event Driven Non-Blocking I/O in V8 javascript**". The `event driven` part means we are asking for `I/O`s to happen, and we get an event notification when they are done (`libuv`'s event loop is constantly checking for the events in its queue and calls their callback methods).  Those `I/O`s are happening in the OS level (opening files, connecting to database, retrieving and sending information over the internet etc.). Since our Javascript codes can continue to run, which means the `Node` will continue to do something else without waiting the current task to finish, we say this process is `Non-Blocking`. 

> Node lets us wirte synchronous codes in Javascript which is easy to manage, and still responds to things happening asynchronously. We just say "Please run these codes, and notify me when these codes done,  while keeping running the codes over there.". 

`libuv` has its own website: http://libuv.org/ .

![](non_blocking_io.png)


#### Binary Data, Character Sets, and Encodings
- **Binary Data**
Data stored in Binary (sets of 1s and 0s).

- **Character Set**
A representation of characters as numbers. Each character gets a number. Unicode and ASCII are character sets. e.g. "hello" can be represented in Unicode as h (104) e (101) l (108) l (108) o (111). A character set decides what number and which number can be assigned to each character.

- **Encodings**
How characters are stored in binary, especially how many bits are used to represent a number in memory, e.g. UTF-8.

> When we are talking about character set, we are talking about **what number** is assigned to a character. When we are talking about encoding, we are talking about **how many bits** we are using to store that number.

#### Buffer
Buffer in Node is global. It holds raw binary data and it let us switch those binary data into strings according to the specified encoding methods.
```  javascript
var buf = new Buffer('Hello', 'utf8');// utf8 is default
buf.write('wo');//buf now holds 'wollo'
```

#### Callbacks
A function passed to some other function, which we assume will be invoked at some point. The function 'calls back' back invoking the function you give it when it is done doing its work.
``` javascript
function greet(callback) {
	var data = {
		name: 'John Doe'
	}
	callback(data);
}

greet(function(data) {
	console.log('The callback was invoked!');
	console.log(data);
});
```

#### Files and fs
Synchronous way to read a file. It is useful when reading some configuration files which have be loaded before we can continue.
``` javascript
var fs = require('fs');

// synchronous approach to read a file
var greet = fs.readFileSync(__dirname + '/greet.txt', 'utf8');
console.log(greet);
```
Asynchronous way to read a file which returns a buffer in memory. It is a good approach to read some large files since it won't block the codes from running. However, if the file is super big or we don't want to put all the content in the memory, we should use the streaming way instead.
``` javascript
var fs = require('fs');

// Asynchronous approach to read a file
var greet = fs.readFile(__dirname + '/greet.txt', function(err, data) {
    // data is a buffer
    console.log(data); // prints out binary data
});


var greet = fs.readFile(__dirname + '/greet.txt', 'utf8', function(err, data) {
    // data is a string
    console.log(data); // prints out the data represented as a stirng
});
```

> **Error-First Callback**: Callbacks take an error object as their first parameter. null if no error, otherwise will contain an object defining the error. 

#### Streams
A stream is just a sequence of pieces of data. The data is broken up into chunks.

Example about creating a readable stream.
``` javascript
var fs = require('fs');

var readable1 = fs.createReadStream(__dirname + '/greet.txt');
readable1.on('data', function(chunk) {
    // chunk is returned as buffer
    console.log(chunk);
});

var readable2 = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8' });
readable2.on('data', function(chunk) {
    // chunk is returned as a string in a default 64KB sized buffer
    console.log(chunk);
});

var readable3 = fs.createReadStream(__dirname + '/greet.txt', { encoding: 'utf8', highWaterMark: 16 * 1024});
readable3.on('data', function(chunk) {
    // chunk is returned as a string in a 16KB sized buffer
    console.log(chunk);
});
```
Examples about creating a writable stream.
``` javascript
var fs = require('fs');

var readable = fs.createReadStream(__dirname + '/greet.txt');

var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');

readable.on('data', function(chunk) {
    writable.write(chunk);
});
```
We have a smarter way to deal with copying contents from one stream to another stream than the snippet above. That is pipe.

#### Pipes
Connecting two streams by writing to one stream what is being read from another. In Node, we can pipe from a Readable stream to a Writable stream.
![](pipes.png)
A simple example about pipes between two files.
``` javascript
var fs = require('fs');

var readable = fs.createReadStream(__dirname + '/greet.txt');
var writable = fs.createWriteStream(__dirname + '/greetcopy.txt');
readable.pipe(writable);// the pipe method return the dest, which is the writable
```
The following example shows how to use pipe to create a compressed file.
``` javascript
var fs = require('fs');
var zlib = require('zlib');

var readable = fs.createReadStream(__dirname + '/greet.txt');
var compressed = fs.createWriteStream(__dirname + '/greet.txt.gz');

var gzip = zlib.createGzip();
// readable pipes the content to gzip, which returns the pipe with the  compressed data, and then these data are finally piped to the compressed file.
readable.pipe(gzip).pipe(compressed);
```

#### .call and .apply
`.call()` and `.apply()` are helper methods for calling a method, while passing a variable that the object's `this` keyword will point to.

``` javascript
var obj = {
	name: 'John Doe';
	greet: function(param1, param2) {
		console.log('Hello ${ this.name }');
	}
}
```
For example, we have the following three ways to invoke the `greet()` mtheod. The difference between `.call` and `.apply` is that, `.call` passes in the parameters one by one, while `.apply` passes the parameters that grouped in an array.
``` javascript
// normal way
obj.greet(param1, param2);
// object's name is now 'Jane Doe'
obj.greet().call({ name: 'Jane Doe'}, param1, param2);
// object's name is now 'Jane Doe'
obj.greet().apply({ name: 'Jane Doe'}, [param1, param2]);
```

