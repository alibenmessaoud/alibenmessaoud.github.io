# “But It Works on My Machine!”

# Benjamin Muschko

Have you ever joined a new team or project and had to try to find your way around the infrastructure needed to build the source code on your developer’s machine? You’re not alone, and you may have had questions:

- What JDK version and distribution are required to compile the code? 
- What if I’m running Linux, but everyone else is on Windows?
- What IDE do you use, and which version do I need?
- What version of Maven or other build tool do I need to install to properly run through developer workflows?

I hope the answer you got to these questions wasn’t “Let me have a look at the tools installed on my machine”—every project should have a clearly defined set of tools that are compatible with the technical requirements to compile, test, execute, and package the code. If you’re lucky, these requirements are documented in a playbook or wiki, although as we all know, documentation easily becomes outdated, and keeping the instructions in sync with the latest changes takes concerted effort.

There’s a better way to solve the problem. In the spirit of *infrastructure as code*, tooling providers came up with the *wrapper*, a solution that helps with provisioning a standardized version of the build tool runtime without manual intervention. It *wraps* the instructions required to download and install the runtime. In the Java space, you’ll find the [Gradle Wrapper](https://oreil.ly/CmZP1) and the [Maven Wrapper](https://oreil.ly/xu50T). Even other tooling, like Bazel, Google’s open source build tool, provides a [launching mechanism](https://oreil.ly/OY7R7).

Let’s see how the Maven Wrapper works in practice. You have to have the Maven runtime installed on your machine to generate the so-called Wrapper files. Wrapper files represent the scripts, configuration, and instructions every developer of the project uses to build the project with a predefined version of the Maven runtime. Consequently, those files should be checked into SCM alongside the project source code for further distribution. 

The following runs the Wrapper goal provided by the [Takari Maven plug-in](https://oreil.ly/sI2pO):

```
mvn -N io.takari:maven:0.7.6:wrapper
```

The following directory structure shows a typical Maven project augmented by the Wrapper files, marked in bold:

```
.
├── .mvn
│   └── wrapper
│       ├── MavenWrapperDownloader.java
│       ├── maven-wrapper.jar
│       └── maven-wrapper.properties
├── mvnw
├── mvnw.cmd
├── pom.xml
└── src
   └── ...
```

With the Wrapper files in place, building the project on any machine is straightforward: run your desired goal with the *mvnw* script. The script automatically ensures the Maven runtime will be installed with the predefined version set in *maven-wrapper.properties*. Of course, the installation process is only invoked if the runtime isn’t already available on the system. 

The following command execution uses the script to run the goals *clean* and *install* on a Linux, Unix, or macOS system:

```
./mvnw clean install
```

On Windows, use the batch script ending with the file extension .*cmd*:

```
mvnw.cmd clean install
```

What about running typical tasks in the IDE or from your CI/CD pipeline? You’ll find other execution environments derive the same runtime configuration from the Wrapper definition as well. You just have to ensure the Wrapper scripts are called to invoke the build.

Gone are the days of “But it works on my machine!”—standardize once, build everywhere! Introduce the wrapper concept to any JVM project to improve build reproducibility and maintainability.