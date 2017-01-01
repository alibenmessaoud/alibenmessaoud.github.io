---
layout: post
title: Java Updates Guide
tags: java
---



This page aims to tell and keep a log about updates in Java. Java is adopting a new cadence-based release cycle of the new releases and versions, and if you haven’t caught up yet – this is a good place to start. You can come back after each release to read and know more about it.

**Last update: 2021.03.17** *

## A word about the new Release Cycle

Now, a Java version arrives every six months, and this speeds up the development of Java. It is a good idea on its own but can confuse people that got used to waiting for a new Java version to be released after years and not six months. 
We’re also more expected to see new types of features labeled Preview, Incubating, and Experimental and an LTS versions will be released each three years.

## Java 9, 21 September 2017

Java 9 features [81 JEPs](https://openjdk.java.net/projects/jdk9/).

- The [Module System](https://openjdk.java.net/jeps/261) (aka Project Jigsaw) 
- [Variable Handles](https://openjdk.java.net/jeps/193) – equivalents of `java.util.concurrent.atomic` and `sun.misc.Unsafe` operations upon object fields and array elements

- [JShell](https://openjdk.java.net/jeps/222) – a dedicated REPL
- [G1](https://openjdk.java.net/jeps/248) – make G1 the default garbage collector on 32- and 64-bit server configurations
- [Stalk-Walking API](https://openjdk.java.net/jeps/259) – provide an API to access lazily the information in stack traces
- [Convenience Factory Methods for Collections](https://openjdk.java.net/jeps/269) – collections can be now initialized using `of()` methods

## Java 10, 20 March 2018

Java 10 features [12 JEPs](https://openjdk.java.net/projects/jdk/10/) and various small API additions.

- First one to be released under the new release cycle
-  [Local-Variable Type Inference (var)](https://openjdk.java.net/jeps/286) – enhance the language to extend type inference to declarations of local variables with initializers
-  [Garbage Collector interface](https://openjdk.java.net/jeps/304) – improve the source code isolation of different GCs by introducing a clean GC interface
- [Parallel Full GC for G1](https://openjdk.java.net/jeps/307) – improve G1 worst-case latencies by making the full GC parallel
- [Application Class-Data Sharing ("CDS")](https://openjdk.java.net/jeps/310) – improve startup and footprint, extend the existing CDS feature to allow application classes to be placed in the shared archive.
- Collectors to be used with unmodifiable collections

## Java 11, 25 September 2018

Java 11 is the first LTS release and includes [17 JEPs](https://openjdk.java.net/projects/jdk/11/).

- [HTTP Client](https://openjdk.java.net/jeps/321) – standardize the incubated HTTP Client API introduced in Java 9, and updated in Java 10
- [Flight Recorder](https://openjdk.java.net/jeps/328) – provide a low-overhead data collection framework for troubleshooting Java applications and the HotSpot JVM
- [A Scalable Low-Latency GC](https://openjdk.java.net/jeps/333)
- [A No-Op Garbage Collector](https://openjdk.java.net/jeps/318) – handles memory allocation but does not implement any actual memory reclamation mechanism
- [Java EE and CORBA modules](https://openjdk.java.net/jeps/320) got removed
- [Nashorn](https://openjdk.java.net/jeps/335) deprecated
- Addition to `Optional` API, `String` API

## Java 12, 19 March 2019

Java 12 features [12 JEPs](https://openjdk.java.net/projects/jdk/12/).

- [Switch Expressions](https://openjdk.java.net/jeps/325) – can be used as either a statement or an expression
- G1 improvements – [abortable mixed collections for G1](https://openjdk.java.net/jeps/344), [promptly return unused committed memory from G1](https://openjdk.java.net/jeps/346)
- [Shenandoah](https://openjdk.java.net/jeps/189) – a Low-Pause-Time Garbage Collector

## Java 13, 17 September 2019

Java 13 features [5 JEPs](https://openjdk.java.net/projects/jdk/13/).

- [Switch Expressions](https://openjdk.java.net/jeps/354)
- [Socket API](https://openjdk.java.net/jeps/353) – replace the underlying implementation used by the `java.net.Socket` and `java.net.ServerSocket` APIs
- [ZGC](https://openjdk.java.net/jeps/351) - support for Linux/AArch64

## Java 14, 17 March 2020

Java 12 features [16 JEPs](https://openjdk.java.net/projects/jdk/14/).

-   [Pattern Matching for instanceof (Preview)](https://openjdk.java.net/jeps/305)
-   [Records (Preview)](https://openjdk.java.net/jeps/359)
-   [Switch Expressions (Standard)](https://openjdk.java.net/jeps/361)
-   [Text Blocks (Second Preview)](https://openjdk.java.net/jeps/368)
-   [Helpful NullPointerExceptions](https://openjdk.java.net/jeps/358)
-   [Packaging Tool (Incubator)](https://openjdk.java.net/jeps/343)
    -   to package Java applications as installation files (exe, msi, pkg, dmg, deb, rpm) for easy installation.
    -   `$ jpackage --name myapp --input lib --main-jar main.jar --main-class myapp.Main --type rpm`
-   [NUMA-Aware Memory Allocation for G1](https://openjdk.java.net/jeps/345)  
    -   Improve G1 performance on large machines by implementing NUMA-aware memory allocation.
-   [JFR (Java Flight Recorder) Event Streaming](https://openjdk.java.net/jeps/349)
    -   Expose JDK Flight Recorder data for continuous monitoring.
-   [Non-Volatile Mapped Byte Buffers](https://openjdk.java.net/jeps/352)
    -   Add new JDK-specific file mapping modes so that the FileChannel API can be used to create MappedByteBuffer instances that refer to non-volatile memory.
-   [Deprecate the Solaris and SPARC Ports](https://openjdk.java.net/jeps/362)
-   [Remove the Concurrent Mark Sweep (CMS) Garbage Collector](https://openjdk.java.net/jeps/363)
-   [ZGC on macOS](https://openjdk.java.net/jeps/364)
    -   [Z Garbage Collector](https://wiki.openjdk.java.net/display/zgc/Main) port to mac OS
-   [ZGC on Windows](https://openjdk.java.net/jeps/365)
    -   [Z Garbage Collector](https://wiki.openjdk.java.net/display/zgc/Main) port to Windows
-   [Deprecate the ParallelScavenge + SerialOld GC Combination](https://openjdk.java.net/jeps/366)
-   [Remove the Pack200 Tools and API](https://openjdk.java.net/jeps/367)
-   [Foreign-Memory Access API (Incubator)](https://openjdk.java.net/jeps/370)
    -   Introduce an API to allow Java programs to safely and efficiently access foreign memory outside of the Java heap.

## Java 15, 15 September 2020

Java 15 features [14 JEPs](https://openjdk.java.net/projects/jdk/15/).

-   [Edwards-Curve Digital Signature Algorithm (EdDSA)](https://openjdk.java.net/jeps/339)
    - NamedParameterSpec.ED25519 to generate key pair, sign and verify messages.
-   [Sealed Classes (Preview)](https://openjdk.java.net/jeps/360)
-   [Hidden Classes](https://openjdk.java.net/jeps/371)
    -   only relevant for frameworks. They cannot be used directly by the bytecode of other classes, they are intended for use by the framework itself that generate classes at run time and use them indirectly, via reflection. 
-   [Remove the Nashorn JavaScript Engine](https://openjdk.java.net/jeps/372)
    -   Java 11 deprecated the Nashorn JavaScript Engine, now it's removed.
-   [Reimplement the Legacy DatagramSocket API](https://openjdk.java.net/jeps/373)
    -   Replace the underlying implementations of the `java.net.DatagramSocket` and `java.net.MulticastSocket` APIs with simpler and more modern implementations that are easy to maintain and debug.
-   [Disable and Deprecate Biased Locking](https://openjdk.java.net/jeps/374)
    -   Disable biased locking by default, and deprecate all related command-line options. Biased locking is an optimization technique used in the HotSpot VM to reduce the overhead of uncontended locking.
-   [Pattern Matching for instanceof (Second Preview)](https://openjdk.java.net/jeps/375)
    -   `if (msg instanceof String s && s.length() > 5)`
-   [ZGC: A Scalable Low-Latency Garbage Collector](https://openjdk.java.net/jeps/377)
    -   Change the Z Garbage Collector from an experimental feature into production feature.
-   [Text Blocks (Preview)](https://openjdk.java.net/jeps/378)
-   [Shenandoah: A Low-Pause-Time Garbage Collector](https://openjdk.java.net/jeps/379)
    -   Change the Shenandoah garbage collector from an experimental feature into production feature. 
-   [Remove the Solaris and SPARC Ports](https://openjdk.java.net/jeps/381)
    -   Remove the source code and build support for the Solaris/SPARC, Solaris/x64, and Linux/SPARC ports.
-   [Foreign-Memory Access API (Second Incubator)](https://openjdk.java.net/jeps/383)
    -   Introduce an API to allow Java programs to safely and efficiently access foreign memory outside of the Java heap.
-   [Records (Second Preview)](https://openjdk.java.net/jeps/384)
-   [Deprecate RMI Activation for Removal](https://openjdk.java.net/jeps/385)
    -   Deprecate the RMI Activation mechanism for future removal. RMI Activation is an obsolete part of RMI that has been optional since Java 8. No other part of RMI will be deprecated.

## Java 16, 

[Java 16 features 17 JEPs](https://openjdk.java.net/projects/jdk/16/). 

- [Vector API (Incubator)](https://openjdk.java.net/jeps/338): provides vector computation (i.e. SIMD) supported by hardware instructions.
- [Enable C++14 Language Features](https://openjdk.java.net/jeps/347): allows the use of C++14 language features in JDK C++ source code, and give specific guidance about which of those features may be used in HotSpot code. means the code will be compiled with std=c++14 like options in all platforms. 
- [Migrate from Mercurial to Git](https://openjdk.java.net/jeps/357)
- [Migrate to GitHub](https://openjdk.java.net/jeps/369)
- [ZGC: Concurrent Thread-Stack Processing](https://openjdk.java.net/jeps/376): Remaining activities that is left in GC safe-points in ZGC will be moved to concurrent phase to eliminate effectively all pause and scalability issues.
- [Unix-Domain Socket Channels](https://openjdk.java.net/jeps/380): Support for UNIX-domain sockets (AF_UNIX) through java.net.UnixDomainSocketAddress.
- [Alpine Linux Port](https://openjdk.java.net/jeps/386): port of JDK to Linux distributions using musl as the standard library such as Alpine Linux, both on x64 and AArch64.
- [Elastic Metaspace](https://openjdk.java.net/jeps/387): improve the use of the memory holding the class-metadata (metaspace). It will reduce the footprint and improve the efficiency of returning unused metaspace to the operating system.
- [Windows/AArch64 Port](https://openjdk.java.net/jeps/388): This will make a port of JDK to Windows on AArch64.
- [Foreign Linker API (Incubator)](https://openjdk.java.net/jeps/389): This API will offer a pure-Java way to access native code, replacing JNI.
- [Warnings for Value-Based Classes](https://openjdk.java.net/jeps/390): Primitive wrapper classes, that are annotated as `@jdk.internal.ValueBased`, will have their constructors, which are already marked as deprecated in Java 9, will produce removal warnings. E.g. when using a Double as a synchronized object.
- [Packaging Tool](https://openjdk.java.net/jeps/392): provides a way to generate platform native packages such as deb and rpm in Linux, pkg and dmg in macOS and msi and exe in Windows.
- [Foreign-Memory Access API (Third Incubator)](https://openjdk.java.net/jeps/393): includes a simplified way to perform most common and/or simple tasks with MemoryAccess static class, without a need for VarHandle.
- [Pattern Matching for instanceof](https://openjdk.java.net/jeps/394): do `variable instanceof Type typedVar` checks—no more preview.
- [Records](https://openjdk.java.net/jeps/395): A record is a nominal tuple that has no need for encapsulation—no more preview.
- [Strongly Encapsulate JDK Internals by Default](https://openjdk.java.net/jeps/396): change the default value of `--illegal-access` option from permit to deny. This means JDK internal classes other than critical APIs such as `sun.misc.Unsafe` will not be open by default.
- [Sealed Classes (Second Preview)](https://openjdk.java.net/jeps/397):  A sealed class/interface can't be extended/implemented except by the types it lists—one more preview.

More details on https://javaalmanac.io/, collection of information about the history and future of Java.