# Gradle - Display project dependency
title: Gradle - Display project dependency
date: 2015-10-27 15:00:00
tags:
- Gradle
- Java
- Android
categories:
- Build Tools

---


In this tutorial, we will show how to display an Android project's dependencies via gradle. Let's use the [RetrofitDemo project](https://github.com/registercosmo/RetrofitDemo) as an example.
<!--more-->
If we want to list the dependencies in the module `app`, first **navigate to the module directory**.

- **List all dependencies:**
``` bash
gradle dependencies
```

- **List dependencies at compile time:**
``` bash
gradle dependencies --configuration compile
```
Here is the result:
``` bash
:app:dependencies

------------------------------------------------------------
Project :app
------------------------------------------------------------

compile - Classpath for compiling the main sources.
+--- com.android.support:appcompat-v7:22.2.1
|    \--- com.android.support:support-v4:22.2.1
|         \--- com.android.support:support-annotations:22.2.1
+--- com.android.support:design:22.2.1
|    +--- com.android.support:appcompat-v7:22.2.1 (*)
|    \--- com.android.support:support-v4:22.2.1 (*)
+--- com.squareup.picasso:picasso:2.5.2
+--- com.squareup.retrofit:retrofit:2.0.0-beta2
|    \--- com.squareup.okhttp:okhttp:2.5.0
|         \--- com.squareup.okio:okio:1.6.0
+--- com.squareup.retrofit:adapter-rxjava:2.0.0-beta2
|    +--- com.squareup.retrofit:retrofit:2.0.0-beta2 (*)
|    \--- io.reactivex:rxjava:1.0.14
\--- com.squareup.retrofit:converter-gson:2.0.0-beta2
     +--- com.squareup.retrofit:retrofit:2.0.0-beta2 (*)
     \--- com.google.code.gson:gson:2.3.1

(*) - dependencies omitted (listed previously)

BUILD SUCCESSFUL

Total time: 10.579 secs
```