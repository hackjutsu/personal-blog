#What is Node.js' Connect, Express and “middleware”?

title: What is Node.js' Connect, Express and “middleware”?
date: 2015-10-1 11:00:45
tags:
- Node.js
- Javascript
- Express
- Connect

---

This is a [stackoverflow question](http://stackoverflow.com/questions/5284340/what-is-node-js-connect-express-and-middleware) about the difference among Node.js, Connect and Express.
> Despite knowing JavaScript quite well, I'm confused what exactly these three projects in Node.js ecosystem do. Is it something like Rails' Rack? Can someone please explain?

The following section is adopted from [the most comprehensive answer](http://stackoverflow.com/a/23957864/3697757) from [basarat](http://stackoverflow.com/users/390330/basarat), even though it is not the accepted answer.
<!-- more -->


----------


What Node.js comes with
-----------------------
[http][1] / [https][2] `createServer` which simply takes a callback(req,res) e.g.

    var server = http.createServer(function (request, response) {

        // respond
        response.write('hello client!');
        response.end();

    });

    server.listen(3000);


What connect adds
-----------------
**Middleware** is basically any software that sits between your application code and some low level API. Connect extends the built-in HTTP server functionality and adds a plugin framework. The plugins act as middleware and hence connect is a *middleware framework*

The way it does that is pretty simple ([and in fact the code is really short!][3]). As soon as you call `var connect = require('connect'); var app = connect();` you get a function `app` that can:

 1.  Can handle a request and return a response. This is because you basically get [this function][4]
 2.  Has a member function `.use` ([source][7]) to manage *plugins* ([that comes from here][5] because of [this simple line of code][6]).

Because of 1.) you can do the following :

    var app = connect();

    // Register with http
    http.createServer(app)
        .listen(3000);

Combine with 2.) and you get:


    var connect = require('connect');

    // Create a connect dispatcher
    var app = connect()
          // register a middleware
          .use(function (req, res, next) { next(); });

    // Register with http
    http.createServer(app)
        .listen(3000);

Connect provides a utility function to register itself with `http` so that you don't need to make the call to `http.createServer(app)`. Its called `listen` and the code simply creates a new http server, register's connect as the callback and forwards the arguments to `http.listen`. [From source][8]

    app.listen = function(){
      var server = http.createServer(this);
      return server.listen.apply(server, arguments);
    };

So, you can do:

    var connect = require('connect');

    // Create a connect dispatcher and register with http
    var app = connect()
              .listen(3000);
    console.log('server running on port 3000');

It's still your good old `http.createServer` with a plugin framework on top.

What ExpressJS adds
-------------------
ExpressJS and connect are parallel projects. Connect is *just* a middleware framework, with a nice `use` function. *Express does not depend on Connect* ([see package.json][9]). However it does the everything that connect does i.e:

 1. Can be registered with `createServer` like connect since it too just just a function that can take a `req`/`res` pair ([source][10]).
 2. A [use function to register middleware][11].
 3. A utility `listen` function to [register itself with http][12]

In addition to what connect provides (which express duplicates), it has a bunch of more features. e.g.

 1. Has [view engine support][13].
 2. Has top level [verbs (get/post etc.) for its router][14].
 3. Has [application settings][15] support.

The middleware is *shared*
------------------------
The `use` function of ExpressJS *and* connect is compatible and therefore the *middleware is shared*. Both are middleware frameworks, express just has more than *a simple middleware framework*.


----------


Which one should you use?
------------------------

My opinion. You are informed enough ^based on above^ to make your own choice.

 - Use `http.createServer` if you are creating something like connect / expressjs from scratch.
 - Use connect if you are authoring middleware, testing protocols etc. since it is a nice abstraction on top of `http.createServer`
 - Use ExpressJS if you are authoring websites.

Most people should just use ExpressJS.

What's wrong about the [accepted answer](http://stackoverflow.com/questions/5284340/what-is-node-js-connect-express-and-middleware/5290324#5290324)
------------------------


These might have been true as some point in time, but wrong now:

> that inherits an extended version of http.Server

Wrong. It doesn't extend it and as you have seen ... *uses it*

> Express does to Connect what Connect does to the http module

Express 4.0 doesn't even depend on connect. [see the current package.json dependencies section][16]


  [1]: http://nodejs.org/api/http.html
  [2]: http://nodejs.org/api/https.html
  [3]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/proto.js
  [4]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/connect.js#L28
  [5]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/proto.js
  [6]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/connect.js#L29
  [7]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/proto.js#L62
  [8]: https://github.com/senchalabs/connect/blob/a9d71168013454635cf069ff2954977df355979f/lib/proto.js#L231-L234
  [9]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/package.json#L49-L68
  [10]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/express.js#L27
  [11]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/application.js#L174
  [12]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/application.js#L538-L541
  [13]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/application.js#L463
  [14]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/application.js#L408-L409
  [15]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/lib/application.js#L307
  [16]: https://github.com/visionmedia/express/blob/311e83e591a149a7549bab543dfd126d3223f7fd/package.json#L49-L68