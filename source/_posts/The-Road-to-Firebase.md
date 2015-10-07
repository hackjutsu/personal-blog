title: The Road to Firebase
date: 2015-09-24 22:33:13
tags: 
- Firebase
- Javascript
---

## Quick Reference
> [Udemy course: Learning Firebase](https://www.udemy.com/learning-firebase/learn/#/)
   [Firebase Open Data Set](https://www.firebase.com/docs/open-data/)
   [GeoFire](https://github.com/firebase/geofire-js): Helper library that allows you query based on geographic location.

<!-- more -->

----------


## Understanding the Data
### It's a JSON Tree
All Firebase data are stored as **JSON objects**. When we add data to the JSON tree, it becomes a key in the existing JSON structure. For example, if we add a child named `widgets` under `users/mchen/`, our data looks as follow:
``` javascript
{
  "users": {
    "mchen": {
      "friends": { "brinchen": true },
      "name": "Mary Chen",
      // our child node appears in the existing JSON tree
      "widgets": { "one": true, "three": true }
    },
    "brinchen": { ... },
    "hmadi": { ... }
  }
}
```
### Creating a Firebase Database Reference
First, we need to create a database reference. The data to be loaded is specified by a URL.
``` javascript
new Firebase('https://publicdata-weather.firebaseio.com/');
```
Creating a reference **does not** create a connection to the server or begin downloading data. Data is not fetched until a read or write operation is invoked. Once it is retrieved, it stays cached locally until the last event listener is removed.

It is possible to access the child node directly as well. (e.g. weather information in Austin)
``` javascript
new Firebase('https://publicdata-weather.firebaseio.com/austin');
```
We can achieve the same result from an existing parent reference by using the `child()` API call:
``` javascript
var weatherRef = new Firebase('https://publicdata-weather.firebaseio.com/');
weatherRef.child('austin');
```
In a similar fashion, it's possible to drill down directly to the database data in the application Dashboard by simply adding the child path to the URL.

> There is an [array storage issue](https://www.firebase.com/docs/web/guide/understanding-data.html#section-arrays-in-firebase) in Firebase which deserves attention.


----------


## Saving Data
In this section, we will cover [four methods](https://www.firebase.com/docs/web/guide/saving-data.html#section-ways-to-save) for writing data to your database:
`set()`, `update()`, `push()` and `transaction()`.

| Method      |      |
| :-----------: | :-------- |
| **set()**    |   Write or replace data to a defined path, like `messages/users/<username>`.  |
| **update()**    |   Update some of the keys for a defined path without replacing all of the data.  |
| **push()**    |   Add to a list of data in the database. Every time you call `push()` your database generates a unique ID, like `messages/users/<unique-user-id>/<username>`.  |
| **transaction()**    |   Use our transactions feature when working with complex data that could be corrupted by concurrent updates  |

### set( )
The basic database operation is `set()`,  which saves new data to the specified database reference, **replacing** any data at that path. 

``` javascript
usersRef.child("alanisawesome").set({
  date_of_birth: "June 23, 1912",
  full_name: "Alan Turing"
});
usersRef.child("gracehop").set({
  date_of_birth: "December 9, 1906",
  full_name: "Grace Hopper"
});
```
The above operations will result in data saved to the database. Note that using `set()` will **overwirte** the data at the specified location, including any child nodes.
``` javascript
{
  "users": {
    "alanisawesome": {
      "date_of_birth": "June 23, 1912",
      "full_name": "Alan Turing"
 	  },
    "gracehop": {
      "date_of_birth": "December 9, 1906",
      "full_name": "Grace Hopper"
 	  }
  }
}
```

### update( )
If you want to write to multiple children of a database location at the same time **without overwriting** other child nodes, you can use the `update()` method as shown below:
``` javascript
var hopperRef = usersRef.child("gracehop");
hopperRef.update({
  "nickname": "Amazing Grace"
});
```
This will update Grace's data to include her nickname. If we had used `set()` here instead of `update()`, it would have deleted both full_name and date_of_birth from our hopperRef.

> **Note:** `update()` only updates data at the first child level, any data passed in beyond the first child level is a treated as a `set()` operation.

### push( )
Firebase JavaScript clients provide a `push()` function that generates a unique ID, or key, for each new child. By using unique child keys, several clients can add children to the same location at the same time without [worrying about write conflicts](https://www.firebase.com/docs/web/guide/saving-data.html#section-push).
``` javascript
  var postsRef = ref.child("posts");

  var newPostRef = postsRef.push();
  newPostRef.set({
    author: "gracehop",
    title: "Announcing COBOL, a New Programming Language"
  });

  // we can also chain the two calls together
  postsRef.push().set({
    author: "alanisawesome",
    title: "The Turing Machine"
  });
```
The [unique key](https://www.firebase.com/blog/2015-02-11-firebase-unique-identifiers.html) is based on a timestamp, so list items will automatically be ordered chronologically. Because we generate a unique ID for each blog post, no write conflicts will occur if multiple users add a post at the same time. Our database data now looks like this:
``` javascript
{
  "posts": {
    "-JRHTHaIs-jNPLXOQivY": {
      "author": "gracehop",
      "title": "Announcing COBOL, a New Programming Language"
    },
    "-JRHTHaKuITFIhnj02kE": {
      "author": "alanisawesome",
      "title": "The Turing Machine"
    }
  }
}
```
In JavaScript, the pattern of calling `push()` and then immediately calling `set()` is so common that we let you combine them by passing the data to be set directly to `push()` as follows:
``` javascript
// This is equivalent to the calls to push().set(...) above
  postsRef.push({
    author: "gracehop",
    title: "Announcing COBOL, a New Programming Language"
  });
```

> **Note:** A push ID can be generated on the client will work while offline and is optimized for performance.

### transaction( ) 
When working with complex data that could be corrupted by concurrent modifications, such as incremental counters, we can use a [transaction](https://www.firebase.com/docs/web/api/firebase/transaction.html) operation. You give this operation two arguments: an update function and an optional completion callback. The update function takes **the current state** of the data as an argument and will return the new desired state you would like to write. For example, if we wanted to increment the number of upvotes on a specific blog post, we would write a transaction like the following:
``` javascript
var upvotesRef = new Firebase('https://docs-examples.firebaseio.com/android/saving-data/fireblog/posts/-JRHTHaIs-jNPLXOQivY/upvotes');
upvotesRef.transaction(function (current_value) {
  return (current_value || 0) + 1;
});
```
We use `current_value || 0` to see if the counter is null or hasn't been incremented yet, since transactions can be called with null if no default value was written.

If the above code had been run without a transaction function and two clients attempted to increment it simultaneously, they would both write 1 as the new value, resulting in one increment instead of two.

>**Note:**  `transaction()` will be called multiple times and must be able to handle null data. Even if there is existing data in your database it may not be locally cached when the transaction function is run.


----------


## Retrieving Data
Data stored in a Firebase database is retrieved by attaching **an asynchronous listener** to a database reference. The listener will be triggered once for the initial state of the data and again anytime the data changes. 

### Read Event Types
>**Value**
The `value` event is used to read a static snapshot of the contents at a given database path, as they existed at the time of the read event. It is triggered once with the initial data and again every time the data changes. The event callback is passed a snapshot containing all data at that location, including child data. In our code example below, value returned all of the blog posts in our app. Everytime a new blog post is added, the callback function will return all of the posts.
``` javascript
// Get a database reference to our posts
var ref = new Firebase("https://docs-examples.firebaseio.com/web/saving-data/fireblog/posts");

// Attach an asynchronous callback to read the data at our posts reference
ref.on("value", function(snapshot) {
  console.log(snapshot.val());
}, function (errorObject) {
  console.log("The read failed: " + errorObject.code);
});
```

>**Child Added**
The `child_added` event is typically used when retrieving a list of items from the database. Unlike value which returns the entire contents of the location, `child_added` is triggered once for each existing child and then again **every time a new child is added to the specified path**. The event callback is passed a snapshot containing the new child's data. For ordering purposes, it is also passed a second argument containing the key of the previous child.

If we wanted to retrieve only the data on each new post added to our blogging app, we could use child_added:
``` javascript
// Get a reference to our posts
var ref = new Firebase("https://docs-examples.firebaseio.com/web/saving-data/fireblog/posts");

// Retrieve new posts as they are added to our database
ref.on("child_added", function(snapshot, prevChildKey) {
  var newPost = snapshot.val();
  console.log("Author: " + newPost.author);
  console.log("Title: " + newPost.title);
  console.log("Previous Post ID: " + prevChildKey);
});
```
In this example the snapshot will contain an object with an individual blog post. Because we converted our post to an object using `val()`, we have access to the post's author and title properties by calling author and title respectively. We also have access to the previous post ID from the second prevChildKey argument.

>**Child Changed**
The `child_changed` event is triggered any time a child node is modified. This includes any modifications to descendants of the child node. It is typically used in conjunction with `child_added` and `child_removed` to respond to changes to a list of items. The snapshot passed to the event callback contains the updated data for the child.
``` javascript
// Get a reference to our posts
var ref = new Firebase("https://docs-examples.firebaseio.com/web/saving-data/fireblog/posts");

// Get the data on a post that has changed
ref.on("child_changed", function(snapshot) {
  var changedPost = snapshot.val();
  console.log("The updated post title is " + changedPost.title);
});
```

>**Child Moved**
The `child_moved` event is used when working with ordered data, which is covered below.

### Event Guarantees
- Events will always be triggered when local state changes.
- Events will always eventually reflect the correct state of the data.
- Writes from a single client will always be written to the server and broadcast out to other users in-order.
- Value events are always triggered last and are guaranteed to contain updates from any other events which occurred before that snapshot was taken.

Since value events are always triggered last, the following example will always work:
``` javascript
var ref = new Firebase("https://docs-examples.firebaseio.com/web/saving-data/fireblog/posts");
var count = 0;

ref.on("child_added", function(snap) {
  count++;
  console.log("added", snap.key());
});

// length will always equal count, since snap.val() will include every child_added event
// triggered before this point
ref.once("value", function(snap) {
  console.log("initial data loaded!", Object.keys(snap.val()).length === count);
});
```

### Reading Data Once
In some cases it may be useful for a callback to be called once and then immediately removed. There is a helper function to make this easy:
``` javascript
ref.once("value", function(data) {
  // do some stuff once
});
```

### Detaching Callbacks
Callbacks are removed by specifying the event type and the callback function to be removed, like the following:
``` javascript
ref.off("value", originalCallback);

//If we passed a scope context into on(), it must be passed when detaching the callback.
ref.off("value", originalCallback, this);

// Remove all value callbacks
ref.off("value");

// Remove all callbacks of any type
ref.off();
```

> **Note:** If a callback has been added multiple times to a data location, it will be called multiple times for each event and we must detach it multiple times in order to remove it completely.

### Quering Data
To construct a query in your database, you start by specifying how you want your data to be ordered using one of the ordering functions: `orderByChild()`, `orderByKey()`, `orderByValue()`, or `orderByPriority()`. You can then combine these with five other methods to conduct complex queries: `limitToFirst()`, `limitToLast()`, `startAt()`, `endAt()`, and `equalTo()`.
Sample data:
``` javascript
{
  "lambeosaurus": {
    "height" : 2.1,
    "length" : 12.5,
    "weight": 5000
  },
  "stegosaurus": {
    "height" : 4,
    "length" : 9,
    "weight" : 2500
  }
}
```
#### Ordering by a specified child key
```javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByChild("height").on("child_added", function(snapshot) {
  console.log(snapshot.key() + " was " + snapshot.val().height + " meters tall");
});
```
#### Ordering by key name
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByKey().on("child_added", function(snapshot) {
  console.log(snapshot.key());
});
```
#### Ordering by value
Sample data
``` javascript
{
  "scores": {
    "bruhathkayosaurus" : 55,
    "lambeosaurus" : 21,
    "linhenykus" : 80,
    "pterodactyl" : 93,
    "stegosaurus" : 5,
    "triceratops" : 22
  }
}
```
To sort the dinosaurs by their score, we could construct the following query:
``` javascript
  var scoresRef = new Firebase("https://dinosaur-facts.firebaseio.com/scores");
  scoresRef.orderByValue().on("value", function(snapshot) {
  snapshot.forEach(function(data) {
    console.log("The " + data.key() + " dinosaur's score is " + data.val());
  });
});
```
If you want to use `orderByValue()` in a production app, you should add `.value` to your rules at the appropriate index. [Read the documentation](https://www.firebase.com/docs/security/guide/indexing-data.html) on the `.indexOn` rule for more information.
#### Ordering by priority
We can explicitly order nodes by priority by calling the `orderByPriority()` method. Details on priorities can be found in [the API reference](https://www.firebase.com/docs/web/api/firebase/setpriority.html).

### Complex Queries
#### Limit Queries
The `limitToFirst()` and `limitToLast()` queries are used to set a maximum number of children to be synced for a given callback. If we set a limit of 100, we will initially only receive up to 100 `child_added` events. If we have fewer than 100 messages stored in our database, a `child_added` event will fire for each message. However, if we have over 100 messages, we will only receive a `child_added` event for 100 of those messages. These will be the first 100 ordered messages if we are using `limitToFirst()` or the last 100 ordered messages if we are using `limitToLast()`. As items change, we will receive `child_added` events for items that enter the query and `child_removed` events for items that leave it, so that the total number stays at 100.
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByChild("weight").limitToLast(2).on("child_added", function(snapshot) {
  console.log(snapshot.key());
});
```
```javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByChild("height").limitToFirst(2).on("child_added", function(snapshot) {
  console.log(snapshot.key());
});
```
#### Range Queries
Using `startAt()`, `endAt()`, and `equalTo()` allows us to choose arbitrary starting and ending points for our queries. 
Find all dinosaurs that are at least three meters tall, we can combine `orderByChild()` and `startAt()`:
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByChild("height").startAt(3).on("child_added", function(snapshot) {
  console.log(snapshot.key())
});
```
We can use `endAt()` to find all dinosaurs whose names come before Pterodactyl lexicographically:
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByKey().endAt("pterodactyl").on("child_added", function(snapshot) {
  console.log(snapshot.key());
});
```
Use the `equalTo()` to find all dinosaurs which are 25 meters tall:
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.orderByChild("height").equalTo(25).on("child_added", function(snapshot) {
  console.log(snapshot.key());
});
```
#### Putting All Together
We can combine all of these techniques to create complex queries. For example, we can find the name of the dinosaur that is just shorter than Stegosaurus:
``` javascript
var ref = new Firebase("https://dinosaur-facts.firebaseio.com/dinosaurs");
ref.child("stegosaurus").child("height").on("value", function(stegosaurusHeightSnapshot) {
  var favoriteDinoHeight = stegosaurusHeightSnapshot.val();

  var queryRef = ref.orderByChild("height").endAt(favoriteDinoHeight).limitToLast(2)
  queryRef.on("value", function(querySnapshot) {
      if (querySnapshot.numChildren() == 2) {
        // Data is ordered by increasing height, so we want the first entry
        querySnapshot.forEach(function(dinoSnapshot) {
          console.log("The dinosaur just shorter than the stegasaurus is " + dinoSnapshot.key());

          // Returning true means that we will only loop through the forEach() one time
          return true;
        });
      } else {
        console.log("The stegosaurus is the shortest dino");
      }
  });
});
```
### How Data is ordered
[Reference link.](https://www.firebase.com/docs/web/guide/retrieving-data.html#section-ordered-data)

