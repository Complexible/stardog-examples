# Examples of using and extending Stardog

[![Gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/clarkparsia/stardog-examples?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

This is a small collection of examples of working with [Stardog](http://stardog.com) via its APIs, as
well as examples of how to use some of the extension points within Stardog.

## How to build examples


First, you'll need a valid Stardog download.

For the examples in `examples/api` and `examples/function`, you'll need [Ant](http://ant.apache.org/).  Copy 
the example properties file into the correct place:

```bash
cp project.properties.example project.properties
```

Then, using your text editor of choice, specify the paths to your Stardog installation location and home directory.
Once that's done, you can navigate into any of the directories below `examples/api` or `examples/function` where there is a `build.xml` file
and run `ant build`


For the examples in `examples/client-server`, you'll need [Maven](http://maven.apache.org/) and [Gradle](http://www.gradle.org/).  Install
the Stardog libraries into your local maven repository by executing the following command from the Stardog installation directory:

```bash
bin/mavenInstall
```

The examples in the directories below `examples/client-server` assume you're running the Stardog server in another process on the local host
using the default port.  Then, navigate into any of those directories where there is a `build.gradle` file and run `gradle run`

## Generating Documentation

The Stardog [documentation](http://docs.stardog.com) and its [javadocs](http://docs.stardog.com/java/snarl) are a good
place to start. But some examples in this repository are annotated using Markdown; they can be processed by
[Docco](http://jashkenas.github.io/docco/).

If you don't have Docco installed, it's pretty easy to get started:

```bash
sudo npm install -g docco
```

Then, you can run it directly against any example:

```bash
docco -o docs main/src/com/complexible/stardog/api/ConnectionAPIExample.java
```

Or you can use the supplied `ant docs` task in each build file that will run Docco against all annotated source files.

## Service Loading

You'll notice that a number of examples have in their source a directory `META-INF/services`, these are the service
registrations for each example.

Stardog uses the JDK [ServiceLoader](http://docs.oracle.com/javase/6/docs/api/java/util/ServiceLoader.html) to load
new services at runtime and make them available to the various parts of the system.  The files in the `services`
directory should be the fully qualified class name of the class/service, such as `com.complexible.stardog.plan.filter.functions.Function`,
and the contents of the file should be a list of the fully qualified class names of the implementations of that service.

These need to be a part of your classpath, usually embedded in the jar file with the compiled source, in order for
the `ServiceLoader` to make them up.
