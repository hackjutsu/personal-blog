# Proper way to quit iOS application
title:  Proper way to quit iOS application
date: 2016-01-05 18:00:00
tags:
- iOS

---

> **Q:**  How do I programmatically quit my iOS application?
**A:** There is no API provided for gracefully terminating an iOS application.

On the iPhone there is no concept of quitting an app. The only action that should cause an app to quit is touching the **Home** button on the phone, and that's not something developers have access to.

<!--more-->


----------


## What Apple suggests for termination?
In [Technical Q&A QA1561](https://developer.apple.com/library/ios/qa/qa1561/_index.html), Apple strongly discourages the use of `exit` as it makes the app appear to have crashed. Here is a brief summary of Apple's answer:
- The only way to quit an app is user pressing the `Home` button.
- Display an alert to users when the app fails to perform the intended functions.
- Do not call `exit` function.
	- The app will appear to the user to have crashed.
	- Data may not be saved, because `-applicationWillTerminate:` and similar `UIApplicationDelegate` methods will not be invoked if we call `exit`.


----------

## Brutal termination
The discussion in the stackoverflow post [Proper way to exit iPhone application?](http://stackoverflow.com/questions/355168/proper-way-to-exit-iphone-application) suggests some ungraceful ways to terminate the iOS app.

```objectivec
// approach 1
exit(0)
// or the equivalent approach 2
[[NSThread mainThread] exit]
```
Remember, calling `exit` is actually strongly discouraged by Apple.


----------

## System-initialized termination
According to the [App Programming Guide for iOS](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW9):

>Apps must be prepared for termination to happen at any time and should not wait to save user data or perform other critical tasks. System-initiated termination is a normal part of an app’s life cycle. The system usually terminates apps so that it can reclaim memory and make room for other apps being launched by the user, but the system may also terminate apps that are misbehaving or not responding to events in a timely manner.
>
>Suspended apps receive no notification when they are terminated; the system kills the process and reclaims the corresponding memory. If an app is currently running in the background and not suspended, the system calls the `applicationWillTerminate:` of its app delegate prior to termination. The system does not call this method when the device reboots.
>
>In addition to the system terminating your app, the user can terminate your app explicitly using the multitasking UI. *User-initiated termination has the same effect as terminating a suspended app.* The app’s process is killed and no notification is sent to the app.


