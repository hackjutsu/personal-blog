#Streamming from Browser

title: Streamming from Browser
date: 2015-09-30 18:00:00
tags: HTTP

---

#### **Question:**
When we stream a big file to the server, what is happening inside the browser? Are all the data chunks sent within a single `POST` request? Or each chunk is sent by an individual `POST` request and the whole file is streamed by sending multiple `POST` requests?

<!-- more -->

----------


#### **Answer:**
No need to do multiple POSTs, the browser takes care of that for you. If you're uploading a file you use a `<input type="file" />` HTML control and the very important `<form enctype="multipart/form-data" method="POST">`

The `multipart/form-data` attribute value on a `<form>` tells the browser to split up the binary data (your file) into chunks and sent up the stream one at a time. `multipart/form-data` is a MIME type and all browsers are coded to handle it as a chunked stream of binary data.

See more here in the specification:
http://www.w3.org/TR/html401/interact/forms.html#h-17.13.4.2