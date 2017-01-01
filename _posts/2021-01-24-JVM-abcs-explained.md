---
layout: post
title: JVM ABCs Explained
tags: java jvm
---

*Write once, run anywhere* was a 1995 slogan created by Sun Microsystems to illustrate the Java language's cross-platform benefits. Java was introduced as a way of developing operating system-independent "applets" for the desktop. Then the focus moved to the server-side, and later, Java has become a key technology for the Web. Java Virtual Machine (JVM) is the engine that provides a runtime environment to drive the Java applications. This article presents the JVM, its components, and how it works.

Before this, let's learn more about Runtime and VM concepts.

## What Is a Runtime?

A runtime provides an environment to translate the code written in a high-level language like Java to machine code and understandable by the CPU. 

We can distinguish these types of translators:

- Assemblers: directly translate assembly codes to binaries. The execution is fast.
- Compilers: translate code into assembly code, then use assemblers to translate the resulting code into binary. This technique is slow but provides fast execution.
- Interpreters: translate the code while executing it. The execution may be slow.

## What is a Virtual Machine?

A VM is the representation of a physical computer but in a virtual manner. The physical computer is the host machine and runs the guest machine called the VM.

A physical machine can run multiple VMs, each with its own OS and applications. These VMs are isolated from each other.

## What is the Java Virtual Machine?

Similar to VMs, the JVM creates an isolated space on a host machine. It is used to execute Java programs irrespective of the machine's platform or operating system.

The JVM is used to run Java desktop, server, and web applications.

Java code `.java` is compiled inside the JVM to an intermediary format called Java bytecode `.class`. Then, the JVM parses the resulting Java bytecode and translates it to binaries.

The JVM uses the Just-In-Time (JIT) Compiler during the runtime.

> The JVM is a stack-based VM where all the arithmetic and logic operations are carried out via push and pop operands, and results are stored on the stack. The stack is also the data structure to store methods. Stack-based VM bytecode is very compact because the location of operands is implicitly on the operand stack. Register-based VM bytecode requires all the implicit operands to be part of an instruction. That indicates that the Register-based code size will usually be much larger than Stack-based bytecode. On the other hand, register-based VM's can express computations using fewer VM instructions than a corresponding stack-based VM. Dispatching a VM instruction is costly, so reducing executed VM instructions is likely to improve the Register-based VM's speed significantly.

## Java Virtual Machine Components

The JVM consists of three distinct components:

1. ClassLoader
2. Runtime Memory/Data Area
3. Execution Engine
4. Native Method Interface JNI
5. Native Method Library

### Class Loader

When we compile a `.java` file, it is converted into byte code inside the `.class` file. The class loader loads the code into the main memory when we try to use this class in a program. The first loaded class into memory is usually the class that contains the `main()` method.

There are three steps in the class loading phase: loading, linking, and initialization.

**Loading:** involves considering the binary representation (bytecode) of a class with a particular name and generating the original class from that. There are three built-in class loaders in Java:

- **Bootstrap ClassLoader** **-** the root classloader. It loads the standard Java packages like `java.lang` found inside the `rt.jar` file and under the `$JAVA_HOME/JRE/lib` directory.
- **Extension ClassLoader -** the subclass of the Bootstrap ClassLoader. It loads the extensions of standard Java libraries present in the `$JAVA_HOME/jre/lib/ext` directory.
- **Application ClassLoader -** the subclass of Extension ClassLoader. It loads the files present on the `classpath` or the current directory of the application.

The JVM calls the` ClassLoader.loadClass()` method to load the class into memory based on a fully qualified name and the different Loader classes. If the last child class loader cannot load the class, it throws `NoClassDefFoundError` or `ClassNotFoundException`*.*

**Linking**: Linking process involves combining the different classes and dependencies of the program. Linking includes the following steps:

- **Verification:** checks the `.class` structural correctness against a set of constraints or rules. If the verification fails for some reason, we get a `VerifyException` (Run Java 11 in 8).

**Preparation:** the JVM allocates memory for a class's static fields and initializes them with default values.
- **Resolution:** symbolic references are replaced with real values present in the runtime constant pool.

**Initialization**: involves the execution of the initialization method of the class (known as `<clinit>`) [class's constructor, the static block, and assign values to all the static variables].

### Runtime Data Area

The JVM contains five principal components inside the runtime data area:

- Method Area: All the class level parts, such as the fields, the code of methods, and constructors, are stored in the method area. The JVM throws an `OutOfMemoryError` if the memory is not sufficient. 
- Heap Area: the variables and instances are stored here. This is the runtime data area from which memory for all class instances and arrays is allocated.
- Stack Area: a separate runtime stack is created when the JVM creates a new thread. All the local variables, method calls, and partial results are stored in the stack area. The JVM can throw a `StackOverflowError` a thread requires a larger stack size than what's available.   The Stack Frame is destroyed when the method call is complete. The Stack Frame is composed of three sub-parts:
  - **Local Variables –** Each frame of call contains an array of local variables, and their values are stored here (method parameters and variables).
  - **Operand Stack –** Each frame of call contains a LIFO stack is known as its *operand stack* to perform any intermediate operations.
  - **Frame Data –** All symbols of the method are stored frame data. It also stores the catch block information in case of exceptions.
- Program Counter (PC) Register: carries the address of the currently executing JVM instructions of each thread. Once the instruction is executed, the PC register is updated with the next instruction.
- Native Method Stacks: support native methods written in a language other than Java, such as C and C++. For every new thread, a separate native method stack is also allocated.

It is created on the start-up, and there is only one area per JVM.

### Execution Engine

The execution engine is used for performing functions such as garbage collection and compilation to machine code after loading the bytecode into the main memory and the data about the code in the runtime data area.

However, before executing the program, the bytecode is converted into machine language instructions. The Execution Engine uses the interpreter to execute the byte code at first, but when it finds some duplication, it uses the JIT compiler.

- Interpreter: executes the bytecode instructions as it reads line by line.

- JIT compiler: compiles the entire bytecode and changes it to native machine code. This code is used directly for repeated method calls.

The JIT Compiler has the following components:

1. **Intermediate Code Generator -** generates intermediate code
2. **Code Optimizer -** optimizes the intermediate code for better performance
3. **Target Code Generator -** converts intermediate code to native machine code
4. **Profiler -** finds the hotspots (code that is executed repeatedly)

The Garbage collection makes Java memory-efficient because it removes the unreferenced objects from heap memory and makes free space for new objects. It involves two phases:

1. **Mark -** in this step, the GC identifies the unused objects in the memory
2. **Sweep -** in this step, the GC removes the objects identified during the previous phase

Garbage Collections are done automatically by the JVM at regular intervals and do not need to be handled separately. It can also be triggered by calling `System.gc()`.

### Java Native Interface (JNI)

Java promotes the execution of native code via the Java Native Interface (JNI) to interact with hardware or overcome memory management and performance constraints in Java.

JNI behaves as a bridge for permitting the supporting packages for other programming languages such as C, C++, etc. 

### Native Method Libraries

Native Method Libraries component is a set of binaries in other programming languages, such as C, C++, and assembly. These libraries are loaded through JNI.

## Conclusion

In this article, we discussed the Java Virtual Machine's components. Often we omit how the JVM  works while our code is working. This can be useful when tweaking the JVM or fix a memory leak or understand its internal mechanics.