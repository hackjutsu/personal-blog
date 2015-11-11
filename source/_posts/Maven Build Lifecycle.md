# Maven Build Lifecycle

title: Maven Build Lifecycle
date: 2015-10-31 18:00:00
tags:
- Maven
- Java
categories:
- Build Tools

---

Maven is based around the central concept of a build lifecycle. There are three built-in build lifecycles: `default`, `clean` and `site`.

- The `default` lifecycle handles your project deployment
- The `clean` lifecycle handles project cleaning
- The `site` lifecycle handles the creation of your project's site documentation.

<!-- more -->

----------

## A Build Lifecycle is Made Up of Phases
For example, the default lifecycle comprises of the following phases (for a complete list of the lifecycle phases, refer to the [Lifecycle Reference](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference)):

- **validate** - validate the project is correct and all necessary information is available
- **compile** - compile the source code of the project
- **test** - test the compiled source code using a suitable unit testing framework. These tests should not require the code be packaged or deployed
- **package** - take the compiled code and package it in its distributable format, such as a JAR.
- **integration-test** - process and deploy the package if necessary into an environment where integration tests can be run
- **verify** - run any checks to verify the package is valid and meets quality criteria
- **install** - install the package into the local repository, for use as a dependency in other projects locally
- **deploy** - done in an integration or release environment, copies the final package to the remote repository for sharing with other developers and projects.

These lifecycle phases are executed **sequentially**. When the default lifecycle is used, Maven will first validate the project, then will try to compile the sources, run those against the tests, package the binaries, run integration tests against that package, verify the package, install the verifed package to the local repository, then deploy the installed package in a specified environment.

To do all those, you only need to call the last `build` phase to be executed, in this case, deploy:
``` bash
mvn deploy
```
That is because if you call a build phase, it will execute not only that build phase, but also every build phase prior to the called build phase.

## Lifecycle Reference
Check out this post for the complete reference of the [Maven build lifecycle](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference).

## Reference
This post is a reading note of [the Maven build lifecycle document](https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html#Lifecycle_Reference). Refer to the original post for the most up-to-date information.
