# Maven in 5 Minutes
title: Maven in 5 Minutes
date: 2015-10-29 18:00:00
tags:
- Maven
- Java

---

## Introduction
Maven is build automation tool mainly for Java Programmer, by addressing two aspects of building software: 1. how software is built and 2. how dependencies are managed. It provides easy way to build project, a way to share Jars and include project dependency.

Maven's objectives are:
- Making easy build process - Builds
- Providing Uniform build system - Release/Distribution
- Guidelines for best practices development

Maven is based on Plain Object Model(POM) which provides all the configuration for single project. The `pom.xml` file provides all details for how software can be built.
<!--more-->
## What is a Build Tool?
As stated in the [Maven Tutorial by Jakob Jenkov](http://tutorials.jenkov.com/maven/maven-tutorial.html#what-is-a-build-tool):

> A build tool is a tool that automates everything related to building the software project. Building a software project typically includes one or more of these activities:
>
>- Generating source code (if auto-generated code is used in the project).
>- Generating documentation from the source code.
>- Compiling source code.
>- Packaging compiled code into JAR files or ZIP files.
>- Installing the packaged code on a server, in a repository or somewhere else.
>
>Any given software project may have more activities than these needed to build the finished software. Such activities can normally be plugged into a build tool, so these activities can be automated too.
>
>The advantage of automating the build process is that you minimize the risk of humans making errors while building the software manually. Additionally, an automated build tool is typically faster than a human performing the same steps manually.


----------
## How to create a Java project with Maven
### Create a Project from Maven Template
In a terminal, navigate to the folder you want to create the Java project.
``` bash
mvn archetype:generate -DgroupId=ninja.hackjustu
   -DartifactId=hackjustuDemo
   -DarchetypeArtifactId=maven-archetype-quickstart
   -DinteractiveMode=false
```
Maven will create a Java project from the Maven `maven-archetype-quickstart template`. In above case, a new Java project `hackjustuDemo` and the entire project directory structure are created automatically.

### Maven Directory Layout
With mvn `archetype:generate` + `maven-archetype-quickstart` template, the following project directory structure is created.
``` bash
hackjustuDemo
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── ninja
    │           └── hackjustu
    │               └── App.java
    └── test
        └── java
            └── ninja
                └── hackjustu
                    └── AppTest.java
```
In simple, all source code puts in folder `/src/main/java/`, all unit test code puts in `/src/test/java/`. Check out this [post](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) for more details.

In additional, a standard `pom.xml` is generated. This POM file is like the Ant `build.xml` file, it describes the entire project information, everything from directory structure, project plugins, project dependencies, how to build this project and etc, read [this official POM guide](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html).

Here is the content of the generated `pom.xml`.
``` bash
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>ninja.hackjustu</groupId>
  <artifactId>hackjustuDemo</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>hackjustuDemo</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### Eclipse Project
To make this as an Eclipse project, in terminal, navigate to `hackjustuDemo` project, type in the following commands:
``` bash
mvn eclipse:eclipse
```
Then import the project into the Eclipse IDE:
`File -> Import… -> General->Existing Projects into Workspace`

> **Note:** Nowadays, Eclipse is smart enough to handle the Maven project:
> `File -> Import… -> Maven->Existing Maven Projects`

### Maven Packaging
``` bash
mvn package
```
This command compiles, run unit test and package the project into a `jar` file and put it in the `project/target` folder.

Here is the directory structure after the `mvn package` command:
``` bash
hackjustuDemo
├── pom.xml
├── src
│   ├── main
│   │   └── java
│   │       └── ninja
│   │           └── hackjustu
│   │               └── App.java
│   └── test
│       └── java
│           └── ninja
│               └── hackjustu
│                   └── AppTest.java
└── target
    ├── classes
    │   └── ninja
    │       └── hackjustu
    │           └── App.class
    ├── hackjustuDemo-1.0-SNAPSHOT.jar
    ├── maven-archiver
    │   └── pom.properties
    ├── maven-status
    │   └── maven-compiler-plugin
    │       ├── compile
    │       │   └── default-compile
    │       │       ├── createdFiles.lst
    │       │       └── inputFiles.lst
    │       └── testCompile
    │           └── default-testCompile
    │               ├── createdFiles.lst
    │               └── inputFiles.lst
    ├── surefire-reports
    │   ├── TEST-ninja.hackjustu.AppTest.xml
    │   └── ninja.hackjustu.AppTest.txt
    └── test-classes
        └── ninja
            └── hackjustu
                └── AppTest.class
```

### Run the Application
``` bash
java -cp target/hackjustuDemo-1.0-SNAPSHOT.jar ninja.hackjustu.App
```


----------


## Reference
http://www.mkyong.com/maven/how-to-create-a-java-project-with-maven/

----------


@(Learning Cards)[Marxico|Java|Legacy Tools]