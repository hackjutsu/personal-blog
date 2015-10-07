title: Javascript Express
date: 2015-09-27 18:00:45
tags: Javascript

---

In this section, we talk about the [Express](http://expressjs.com/) web framework for Node.js. It is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.

``` javascript
 $ npm install express --save
 ```

<!-- more -->

#### Routes
[Routing](http://expressjs.com/guide/routing.html) refers to the definition of end points (URIs) to an application and how it responds to client requests. 

A route is a combination of a URI, a HTTP request method (GET, POST, and so on), and one or more handlers for the endpoint. The following is an example of an Express server with basic routes.
``` javascript
var express = require('express');
var app = express();
var port = process.env.PORT || 3000;

// GET method route
app.get('/', function (req, res) {
  res.send('GET request to the homepage');
});

// POST method route
app.post('/', function (req, res) {
  res.send('POST request to the homepage');
});

app.listen(port);
```
Examples of routes paths and paramaters:
``` javascript
// GET mothod with route path, will match req to /api
app.get('/api', function (req, res) {
  res.send('GET request to the homepage/api');
});

// GET mothod with parameter "id", will match req to /person/id
app.get('/person/:id', function (req, res) {
  res.send('GET request to the homepage/person/id ' + req.params.id);
});
```
Examples of route paths based on string patterns:
``` javascript
// will match acd and abcd
app.get('/ab?cd', function(req, res) {
  res.send('ab?cd');
});

// will match abcd, abbcd, abbbcd, and so on
app.get('/ab+cd', function(req, res) {
  res.send('ab+cd');
});

// will match abcd, abxcd, abRABDOMcd, ab123cd, and so on
app.get('/ab*cd', function(req, res) {
  res.send('ab*cd');
});

// will match /abe and /abcde
app.get('/ab(cd)?e', function(req, res) {
 res.send('ab(cd)?e');
});
```

#### Static Files and Middleware
> **Middleware**:  Codes that sit between two layers of software. In the case of Express, middleware sits between the request and the response.

Express provides `.use()` method to define middleware behaviors, including the routing to static files. Let's look at an example. First, we create a style.css file in the `./public` folder with the following contents.
``` css
body {
    font-family: Arial, Helvetica, sans-serif
}
```
Then we create a server which will look for the above style.css when presenting the webpage.

``` javascript
var express = require('express');
var app = express();
var port = process.env.PORT || 3000;

// Route the path '/assets' to the public folder for static files
app.use('/assets', express.static(__dirname + '/public'));

// Middleware
app.use('/', function(req, res, next) {
    console.log('Request Url:' + req.url);
    next(); // Run the next middleware, here the next one is get()
});

// GET method route, which is also a kind of middleware
app.get('/', function (req, res) {
  res.send('<html><head><link href=assets/style.css type=text/css rel=stylesheet /></head><body><h1>Hello World!</h1></body></html>');
});

app.listen(port);
```
#### Templates and Template Engines
Express is non-opinioned on templates, which means we can pick up the template language we like, such as EJS and jade. In this part,  we introduce the [EJS templating languages](http://ejs.co/).

Install EJS:
``` javascript
$ npm install ejs --save
```
A 	quick example about EJS syntax. In the following example, `<%`, and `%>` are delimiters between which we can write some javascript codes. The `<%=` and `%>` (notice the "=" sign) mean the content inside will be output as strings. *(The syntax is not highlighted correctly in the following snippet.)*
``` html
<% if (user) { %>
  <h2><%= user.name %></h2>
<% } %>
```
Here is a more complicated example. First, let's create the template file `views/person.ejs`:
``` html
<html>   
    <head>
        <link href=assets/style.css type=text/css rel=stylesheet />
    </head>
    <body>
        <h1>Person: <%= ID %></h1>
    </body>
</html>
```
Then create the file `server.js`:
``` javascript
var express = require('express');
var app = express();
var port = process.env.PORT || 3000;

 // ejs belove is the file extension we specify
app.set('view engine', 'ejs');

// Route the path '/assets' to the public folder for static files
app.use('/assets', express.static(__dirname + '/public'));

// GET method route
app.get('/person/:id', function (req, res) {
  res.render('person', { ID: req.params.id });
});

app.listen(port);
```
Run `node server.js` and go to `localhost:3000/person/12345` to see the result. Notice that the `<%= ID %>` is replaced by the string "12345" we put in the path. 

Let's look more closely at `server.js`. Line `app.set('view engine', 'ejs')` tells the Express to use files with extension `.ejs` as template for rendering. By default, Express will search the folder `./views`. Line `res.render('person', { ID: req.params.id })` tells Express to search for the template file with the name `person` and the extension `.ejs`, and then replace the placeholder `ID` in the template with the value of `req.params.id`.

####  Querystring and Post Parameters
It's super easy to retrieve the query strings in Node.js. Just simply call `req.query.query_key` to get the value of the key `query_key`.

But if we want to deal with the form or JSON data been posted, we need to parse the body of the http request. So we need something from the middleware which can help us parse the request body, e.g. `body-parser`.
As usual, we need to download the 'body-parser' via npm.
``` javascript
$npm install body-parser --save
```
Here's an example about how to parse the value posted by a form.
``` javascript
var bodyParser = require('body-parser');
var urlencodeParser = bodyParser.urlencoded({ extended:false });

// some codes to create an Express app

app.post('/person', urlencodedParser, function(req, res) {
	res.send('Thank you!');
	console.log(req.body.firstname);
	console.log(req.body.lastname);
});
```
Add the following html codes to create a form which posts data to `/person`.
``` html 
<form method="POST" action="/person">
	First name: <input type="text" id="firstname" name="firstname" /><br />
	Last name: <input type="text" id="lastname" name="lastname" /><br />
	<input type="submit" value="Submit">
</form>
```
If the form sends the body data as a JSON data, we can create a JSON parser from the `body_parser`.
``` javascript
var bodyParser = require('body-parser');
var urlencodeParser = bodyParser.urlencoded({ extended:false });
var jsonParser = bodyParser.json(); 

// some codes to create an Express app

app.post('/person', jsonParser, function(req, res) {
	res.send('Thank you for sending the JSON data!');
	console.log(req.body.firstname);
	console.log(req.body.lastname);
});
```

#### RESTful
> RESTful service is a service that utilizes the HTTP method verbs to expose a concise API. REST stands for Representational State Transfer(REST).

By convention, HTTP verbs, such as GET, POST, PUT and DELETE, are mapping to retrieving, creating, updating and removing the resource specified by the URL.