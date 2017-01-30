Build /BiVeS Web Service 
=========================
When you've cloned the source code:

```
#!sh
git clone git://sems.uni-rostock.de/bivesws
```

There are two supported options to build this project:

* [Build with Maven](#//BuildwithMaven)
* [Build with Ant](#//BuildwithAnt)



Build with Maven 
-----------------
[Maven](https://maven.apache.org/) is a build automation tool. We ship a [source:pom.xml pom.xml] together with the sources which tells maven about versions and dependencies. Thus, maven is able to resolve everything on its own and, in order to create the library, all you need to call is ```mvn package```:

```
#!sh
usr@srv $ mvn package

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running de.unirostock.sems.bives.webservice.TestWeb
Tests run: 4, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.635 sec

Results :

Tests run: 4, Failures: 0, Errors: 0, Skipped: 0
```

That done, you'll find the binaries in the ```target``` directory.

Build with Ant 
---------------
[Ant](https://ant.apache.org/) is an Apache tool for automating software build processes. There is a [source:build.xml build.xml] file included in the source code that tells ant what to do. Since ant is not able to resolve the dependencies you need to create a directory ```lib``` containing the following libraries:
* [BiVeS](bives:wiki) (download latest binary from http://bin.sems.uni-rostock.de or see http://sems.uni-rostock.de/trac/bives/wiki//BuildBives)
* [https://code.google.com/p/json-simple/ json-simple]
* [Servlet API](http://repo1.maven.org/maven2/javax/servlet/javax.servlet-api/)

We defined multiple targets in the ```build.xml`. They can be displayed by calling `ant -p```:

```
#!sh
usr@srv $ ant -p
Buildfile: /path/to/bivesws/build.xml

Main targets:

 clean    Delete old build and dist directories
 compile  Compile Java sources
 dist     Create war file
 sign     sign a dist
Default target: dist
```

* ```clean up``` will delete all compiled files and produced libraries
* ```compile``` compiles the source code
* ```dist` bundles all compiled binaries into a `war``` package

For example, to create the ```war` package just run `ant dist```:

```
#!sh
usr@srv $ ant dist
Buildfile: /path/to/bivesws/build.xml

prepare:

compile:
    [javac] Compiling 1 source file to /path/to/bivesws/build/WEB-INF/classes

dist:
     [copy] Copying 1 file to /path/to/bivesws/build/WEB-INF/lib
      [jar] Building jar: /path/to/bivesws/dist/BiVeS-WS-1.2.2.war

BUILD SUCCESSFUL
Total time: 2 seconds
```

