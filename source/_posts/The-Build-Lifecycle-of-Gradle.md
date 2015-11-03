title: The Build Lifecycle of Gradle
date: 2015-09-25 00:27:57
tags: Gradle
---

We said earlier that the core of Gradle is a language for dependency based programming. In Gradle terms this means that you can define tasks and dependencies between tasks. Gradle guarantees that these tasks are executed in the order of their dependencies, and that each task is executed only once. These tasks form a Directed Acyclic Graph. There are build tools that build up such a dependency graph as they execute their tasks. Gradle builds the complete dependency graph before any task is executed.
<!-- more -->

----------


## Build phases
### Initialization
Gradle supports single and multi-project builds. During the initialization phase, Gradle determines which projects are going to take part in the build, and creates a [Project](https://docs.gradle.org/current/dsl/org.gradle.api.Project.html) instance for each of these projects.

### Configuration
During this phase the project objects are configured. The build scripts of **all** projects which are part of the build are executed. Gradle 1.4 introduced an [incubating](https://docs.gradle.org/current/userguide/feature_lifecycle.html) opt-in feature called **configuration on demand**. In this mode, Gradle configures only relevant projects (see [Section 59.1.1.1, “Configuration on demand”](https://docs.gradle.org/current/userguide/multi_project_builds.html#sec:configuration_on_demand)).

### Execution
Gradle determines the subset of the tasks, created and configured during the configuration phase, to be executed. The subset is determined by the task name arguments passed to the gradle command and the current directory. **Gradle then executes each of the selected tasks**.

> **Q:** So what happens when we run the command `./gradlew clean`?
> **A:**  Gradle will go through `initialization`, `configuration`  and `execution` phases. In  the `configuration` phase, gradle resolves all the  dependencies(check their availability?) and run the build scripts for the dependent projects. After all these are done, gradle will finally  reach the `execution` phase and run the clean task.


----------


## Reference
- https://docs.gradle.org/current/userguide/build_lifecycle.html
- https://goo.gl/z5nOVE
- http://goo.gl/mqw9n1