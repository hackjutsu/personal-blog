# Recover Hidden Password in Browser

title:  Recover Hidden Password in Browser
date: 2015-11-12 21:26:00
tags:
- Hacking

---


Sometimes we might forget our password,  but it is stored in our browser. The password is hidden by "dots" and we cannot get its value directly. Even the trick `"copy and paste to a text file"` doesn't help here.

<!--more-->

![Password Hidden by Dots](http://i.imgur.com/zCY5Rzy.png)

To recover our password, we need to do a little bit HTML hacking here. Let's use Chrome as example.

First, right click and select `Inspect Element`.  (Click on the image for details.)

![Right Click and Inspect Element](http://i.imgur.com/OzoN3yA.png)

Find out the section which describe the password input block. (Click on the image for details.)

![Password Input Block](http://i.imgur.com/53cJ9o6.png)

Change the value of the `type` from `password` to `text`.  (Click on the image for details.)

![type="text"](http://i.imgur.com/gs5gK5l.png)

Welcome home, password!!

![Password Shown as Text](http://i.imgur.com/p3zp2Zc.png)