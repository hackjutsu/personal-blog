# HTTP Server in Node.js
title: HTTP Server in Node.js
date: 2015-09-29 18:00:00
tags:
- Javascript
- Node.js
categories:
- MEAN

---

Like most modules in Node's core, the http module favors simplicity which leaves opinions, syntactic sugar, ans specific details up to the community modules, such as Connect or Express.

This section will cover some of Node's low-level APIs.
<!-- more -->
### HTTP Server Fundamentals
Node provides HTTP server and client interfaces through its http module. To create a HTTP server, call the `http.createServer()` function, which accepts a call back function.
``` javascript
var http = require('http');
var server = http.createServer(function(req, res) {
	var body = 'Hello World';
	// Set the headers.
	res.setHeader('Content-Length', body.length);
	res.setHeader('Content-Type', 'text/plain');
	// Set the status code. 200 is default.
	res.statusCode = 200;
	// write(body) and end() can be combined as end(body)
	res.write(body);
	res.end();
});
server.listen(3000);
```
Notice that, Node will parse the request header but **NOT the request body** until the callback is fired. Node won't automatically write any response back to the client either. It's the developer's responsibility to end the response using the `res.end()` method, or the request will hang until the client times out or it will just remains open.

#### RESTful
> RESTful service is a service that utilizes the HTTP method verbs to expose a concise API. REST stands for **Representational State Transfer**.

By convention, HTTP verbs, such as `GET`, `POST`, `PUT` and `DELETE`, are mapping to retrieving, creating, updating and removing the resource specified by the URL.

#### Building a RESTful web service
Here is a simple example of a RESTful service, with creating resources with `POST`, fetching resources with `GET` and removing resources with `DELETE`. A complete RESTful service should implement a `PUT` for updating existing objects, which doesn't exist here.
``` javascript
var http = require('http');
var url = require('url');
var items = [];

var server = http.createServer(function(req, res){
    switch (req.method) {
        case 'POST':
            var item = '';
            req.setEncoding('utf8');
            req.on('data', function(chunk){
                item += chunk;
            });
            req.on('end', function(chunk){
                items.push(item);
                res.end('OK\n');
            });
            break;
        case 'GET':
            // Setting the Content-Length header can speed up the response,
            // since the body can be easily constructed ahead of time.
            var body = items.map(function(item, i) {
                return i + ')' + item;
            }).join('\n');
            body += '\n';
            // Content-Length value should represent the byte length,
            // not char length. Node provides the method Buffer.byteLength().
            res.setHeader('Content-Length', Buffer.byteLength(body));
            res.setHeader('Content-Type', 'text/plain; charset="utf-8"');
            res.end(body);
            break;
        case 'DELETE':
            var path = url.parse(req.url).pathname;
            var i = parseInt(path.slice(1), 10);

            if (isNaN(i)) {
                res.statusCode = 400;
                res.end('Invalid item id');
            } else if (!items[i]) {
                res.statusCode = 404;
                res.end('Item not found');
            } else {
                items.splice(i, 1);
                res.end('DELETE OK\n');
            }
            break;
    }
});

server.listen(3000);
```
Test the codes above using `cURL`. The DELETE request will delete the item with index "i".
``` bash
$ curl -d 'test string 0' http://localhost:3000
$ curl -d 'test string 1' http://localhost:3000
$ curl http://localhost:3000
$ curl -X "DELETE" http://localhost:3000/1
$ curl http://localhost:3000
```