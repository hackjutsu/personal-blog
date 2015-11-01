# Maven POM in Action

title: Maven POM in Action
date: 2015-10-30 18:00:00
tags: 
- Maven
- Java

---


A `Project Object Model` or `POM` is the fundamental unit of work in Maven. It is an XML file that contains information about the project and configuration details used by Maven to build the project. It contains default values for most projects. For example, the source directory, which is `src/main/java`; the test source directory, which is `src/test/java`; and so on.

POM also contains the goals and plugins. While executing a task or goal, Maven looks for the POM in the current directory. It reads the POM, gets the needed configuration information, then executes the goal. 
<!--more-->


----------


## Super POM
The Super POM is Maven's default POM. All POMs extend the Super POM unless explicitly set, meaning the configuration specified in the Super POM is inherited by the POMs we created for our projects.

The Super POM define the default `repositories`, `pluginRepositories`, `build`, `reporting` and `profiles` settings.


[Check out this link for more information about super POMs](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Super_POM) .


----------


## Minimal POM
The minimum POM contains
- project root
- modelVersion -  should be set to 4.0.0
- groupId - the id of the project's group.
- artifactId - the id of the artifact (project)
- version - the version of the artifact under the specified group
``` bash
<project>
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1</version>
</project>
```
A POM requires that its groupId, artifactId, and version be configured. These three values form the project's fully qualified artifact name. This is in the form of `<groupId>:<artifactId>:<version>`. As for the example above, its fully qualified artifact name is `com.mycompany.app:my-app:1`.

Every Maven project has a packaging type. If it is not specified in the POM, then the default value `jar` would be used.

Furthermore, as we can see that in the minimal POM, the repositories were not specified. If we build our project using the minimal POM, it would **inherit the repositories configuration in the Super POM**. Therefore when Maven sees the dependencies in the minimal POM, it would know that these dependencies will be downloaded from http://repo.maven.apache.org/maven2 which was specified in the Super POM.

----------

## Full list of POM Elements
This is a listing of the elements directly under the POM's project element.
``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>
 
  <!-- Build Settings -->
  <build>...</build>
  <reporting>...</reporting>
 
  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>
 
  <!-- Environment Settings -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>l
```


----------


## The Basics

### Maven Coordinates
 `groupId:artifactId:version` are all required fields (although, groupId and version need not be explicitly defined if they are inherited from a parent - more on inheritance later). The three fields act much like an address and timestamp in one. This marks a specific place in a repository, acting like a coordinate system for Maven projects.
 
### POM Relationships

#### Dependencies
The cornerstone of the POM is its dependency list. Maven brings in the dependencies of those dependencies (transitive dependencies), allowing our list to focus solely on the dependencies our project requires.

##### Dependency Version Requirement Specificatioin
Dependencies' version element define version requirements, used to compute effective dependency version. Version requirements have the following syntax:

- `1.0:` "Soft" requirement on 1.0 (just a recommendation, if it matches all other ranges for the dependency)
- `[1.0]`: "Hard" requirement on 1.0
- `(,1.0]`: x <= 1.0
- `[1.2,1.3]`: 1.2 <= x <= 1.3
- `[1.0,2.0)`: 1.0 <= x < 2.0
- `[1.5,)`: x >= 1.5
- `(,1.0],[1.2,)`: x <= 1.0 or x >= 1.2; multiple sets are comma-separated
- `(,1.1),(1.1,)`: this excludes 1.1 (for example if it is known not to work in combination with this library)

##### More about Dependencies
[Check out this post for more about dependencies](https://maven.apache.org/pom.html#Dependencies).
 
#### Inheritance and Aggregations
[Project Inheritance](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Project_Inheritance)
[Project Aggregation](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html#Project_Aggregation)

### Properties
Properties are the last required piece in understanding POM basics. Maven properties are value placeholder, like properties in Ant. Their values are accessible anywhere within a POM by using the notation `${X}`, where `X` is the property. They come in five different styles:

- **env.X**: Prefixing a variable with "env." will return the shell's environment variable. 
For example, `${env.PATH}` contains the PATH environment variable.


- **project.x**: A dot (.) notated path in the POM will contain the corresponding element's value. 
For example: `<project><version>1.0</version></project>` is accessible via `${project.version}`.


- **settings.x**: A dot (.) notated path in the `settings.xml` will contain the corresponding element's value. 
For example: `<settings><offline>false</offline></settings>` is accessible via `${settings.offline}`.


- **Java System Properties**: 
All properties accessible via `java.lang.System.getProperties()` are available as POM properties, such as `${java.home}`.


- **x**: Set within a `<properties/>` element in the POM. The value of `<properties><someVar>value</someVar></properies>` may be used as `${someVar}`.


----------


## Build Settings
Beyond the basics of the POM given above, there are two more elements that must be understood before claiming basic competency of the POM. They are the `build` element, that handles things like declaring our project's directory structure and managing plugins; and the `reporting` element, that largely mirrors the build element for reporting purposes.

### Build
The build element is conceptually divided into two parts: 
- `BaseBuild` type which contains the set of elements common to both build elements. 
- `Build` type, which contains the `BaseBuild` set as well as more elements for the top level definition. 
>**Note:** These different build elements may be denoted **project build** and **profile build**.
``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
  ...
  <!-- "Project Build" contains more elements than just the BaseBuild set -->
  <build>...</build>
 
  <profiles>
    <profile>
      <!-- "Profile Build" contains a subset of "Project Build"s elements -->
      <build>...</build>
    </profile>
  </profiles>
</project>
```


#### The BaseBuild Element Set
`BaseBuild` is exactly as it sounds: the base set of elements between the two build elements in the POM.
``` xml
<build>
  <defaultGoal>install</defaultGoal>
  <directory>${basedir}/target</directory>
  <finalName>${artifactId}-${version}</finalName>
  <filters>
    <filter>filters/filter1.properties</filter>
  </filters>
  ...
</build>
```
[Check out this post for more details about the BaseBuild element](https://maven.apache.org/pom.html#The_Basics).

##### Resources
Resources are not (usually) code. They are not compiled, but are items meant to be bundled within our project or used for various other reasons.
``` xml
  <build>
    ...
    <resources>
      <resource>
        <targetPath>META-INF/plexus</targetPath>
        <filtering>false</filtering>
        <directory>${basedir}/src/main/plexus</directory>
        <includes>
          <include>configuration.xml</include>
        </includes>
        <excludes>
          <exclude>**/*.properties</exclude>
        </excludes>
      </resource>
    </resources>
    <testResources>
      ...
    </testResources>
    ...
  </build>
```
`testResources` are similar to resource elements, but are naturally used during test phases. 
##### Plugins & Plugin Management
[Check out this post for more information.](https://maven.apache.org/pom.html#Plugins)

#### The Build Element Set
The `Build` type in the [XSD](http://searchsoa.techtarget.com/definition/XSD) denotes those elements that are available only for the "project build". Despite the number of extra elements (six), there are really only two groups of elements that project build contains that are missing from the profile build: directories and extensions.
##### Directories
The set of directory elements live in the parent build element, which set various directory structures for the POM as a whole. Since they do not exist in profile builds, these cannot be altered by profiles.
``` xml
  <build>
    <sourceDirectory>${basedir}/src/main/java</sourceDirectory>
    <scriptSourceDirectory>${basedir}/src/main/scripts</scriptSourceDirectory>
    <testSourceDirectory>${basedir}/src/test/java</testSourceDirectory>
    <outputDirectory>${basedir}/target/classes</outputDirectory>
    <testOutputDirectory>${basedir}/target/test-classes</testOutputDirectory>
    ...
  </build>
```
If the values of a `*Directory` element above is set as an absolute path (when their properties are expanded) then that directory is used. Otherwise, it is relative to the base build directory: `${basedir}`.
##### Extensions
[Check out this post for more information about Extensions](https://maven.apache.org/pom.html#Extensions).
### Report
Reporting contains the elements that correspond specifically for the site generation phase. 
[Check out this post for more information about Report](https://maven.apache.org/pom.html#Reporting).

----------

## More Project Information
Although the above information is enough to get a firm grasp on POM authoring, there are far more elements to make developer's live easier. Many of these elements are related to site generation, but like all POM declarations, they may be used for anything, depending upon how certain plugins use it. 

### License
A project should list only licenses that may apply directly to this project, and not list licenses that apply to this project's dependencies. Maven currently does little with these documents other than displays them on generated sites. 
``` xml
<licenses>
  <license>
    <name>Apache License, Version 2.0</name>
    <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
    <distribution>repo</distribution>
    <comments>A business-friendly OSS license</comments>
  </license>
</licenses>
```

### Organizations
Here is the most basic information about an organization.
``` xml
<organization>
  <name>Hackjustu</name>
  <url>http://hackjustu.ninja</url>
</organization>
```

### Developers
This section is self-explained.
``` xml
<developers>
    <developer>
      <id>hackjustn</id>
      <name>Hackjustu Ninja</name>
      <email>hackjustn@example.com<email>
      <url>http://hackjustu.ninja/someone</url>
      <organization>SomeOrganization</organization>
      <organizationUrl>http://www.example.com</organizationUrl>
      <roles>
        <role>architect</role>
        <role>developer</role>
      </roles>
      <timezone>America/New_York</timezone>
      <properties>
        <picUrl>http://www.example.com/hackjustu/pic</picUrl>
      </properties>
    </developer>
  </developers>

```

### Contributors
This section is self-explained.
``` xml
  <contributors>
    <contributor>
      <name>Noelle</name>
      <email>some.name@gmail.com</email>
      <url>http://noellemarie.com</url>
      <organization>Noelle Marie</organization>
      <organizationUrl>http://noellemarie.com</organizationUrl>
      <roles>
        <role>tester</role>
      </roles>
      <timezone>America/Vancouver</timezone>
      <properties>
        <gtalk>some.name@gmail.com</gtalk>
      </properties>
    </contributor>
  </contributors>
```


----------

## Environment Settings

### Repositories
Whenever a project has a dependency upon an artifact, Maven will **first attempt to use a local copy of the specified artifact**. If that artifact does not exist in the local repository, it will then attempt to download from a remote repository. The repository elements within a POM specify those alternate repositories to search.

The default central Maven repository lives on http://repo.maven.apache.org/maven2/.

[Check out this post for more information.](https://maven.apache.org/pom.html#Repositories)

### Profiles
A Build profile is a set of configuration values which can be used to set or override default values of Maven build. Using a build profile, we can customize build for different environments such as *Production* vs *Development* environments.

``` xml
<profiles>
  <profile>
    <id>production</id>
    <activation>
      <property>
        <name>build</name>
        <value>release</value>
      </property>
    </activation>
    [...]
  </profile>
  <profile>
    <id>development</id>
    <activation>
      <property>
        <name>build</name>
        <value>develop</value>
      </property>
    </activation>
    [...]
  </profile>
<profiles>
```
For example, we can use a property value to activate a specific build profile.
``` bash
mvn -Dbuild=develop package
mvn -Dbuild=develop test

mvn -Dbuild=release release:prepare
mvn -Dbuild=release release:perform
```

### More about Environment Settings
[Check out this post for more information.](https://maven.apache.org/pom.html#Environment_Settings)

----------


## Reference
This post is adapted from the [Maven official document](https://maven.apache.org/pom.html) as reading notes. Please refer to the original document for the most up-to-date information.

@(Learning Cards)[Marxico|Maven]