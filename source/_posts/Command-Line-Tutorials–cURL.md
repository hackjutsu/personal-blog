# Command Line Tutorials – cURL
title: Command Line Tutorials – cURL
date: 2015-09-29 18:00:45
tags: CommandLine

---

> A command line tool for getting or sending files using URL syntax. —— [Wikipedia](https://en.wikipedia.org/wiki/CURL)

<!-- more -->

`curl` is an easy to use command line tool to send and receive files, and it supports almost all major protocols(DICT, FILE, FTP, FTPS, GOPHER, HTTP, HTTPS,  IMAP, IMAPS,  LDAP,  LDAPS,  POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, TELNET and TFTP) in use. The name originally stood for "see URL".

### Post

``` bash
curl --data "birthyear=1905&press=%20OK%20"  http://www.example.com/when.cgi
```
This kind of POST will use the Content-Type application/x-www-form-urlencoded and is the most widely used POST kind. The data you send to the server MUST already be properly encoded, `curl` will not do that for you. 

Recent `curl` versions can in fact url-encode POST data for you, like this:
``` bash
curl --data-urlencode "name=I am Daniel" http://www.example.com
```

### Get
``` bash
curl "http://www.hotmail.com/when/junk.cgi?birthyear=1905&press=OK"
```

### Redirects
By default `curl` doesn’t follow the HTTP Location headers. It is also termed as Redirects. We can insists `curl` to follow the redirection using -L option, as shown below. 
``` bash
$ curl -L http://www.redirector.com
```

### DELETE
``` bash
$ curl -X "DELETE" http://www.example.com/1
```

### Authentication
By default `curl` uses Basic HTTP Authentication. We can specify other authentication method using `–ntlm | –digest`.
``` bash
$ curl -u username:password URL
```

### More about curl
http://curl.haxx.se/docs/httpscripting.html
http://www.thegeekstuff.com/2012/04/curl-examples/


@(Learning Cards)[Marxico|Legacy Tools]