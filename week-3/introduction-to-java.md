# Introduction to Java

---

## What is Java?

Java is a high-level, object-oriented, platform-independent programming language developed by **James Gosling** at **Sun Microsystems** in **1995**. It was later acquired by **Oracle Corporation** in 2010.

Java follows the principle of **"Write Once, Run Anywhere" (WORA)**, meaning compiled Java code can run on any device that supports the Java Virtual Machine (JVM) without recompilation.

Java is widely used for building enterprise applications, mobile applications (Android), web applications, desktop applications, embedded systems, and large-scale distributed systems.

---

## Why Java?

- **Platform Independent** – Java programs can run on any operating system that has a JVM installed.
- **Object-Oriented** – Java follows OOP principles like encapsulation, inheritance, polymorphism, and abstraction.
- **Secure** – Java provides built-in security features like bytecode verification and sandbox execution.
- **Robust** – Strong memory management, exception handling, and type-checking mechanism.
- **Multithreaded** – Java supports concurrent execution of two or more threads for maximum CPU utilization.
- **Simple and Familiar** – Java syntax is based on C/C++, making it easy to learn and use.
- **High Performance** – Just-In-Time (JIT) compiler improves performance by converting bytecode to native machine code at runtime.
- **Distributed** – Java supports distributed computing through RMI and EJB.

---

## Features of Java

| Feature | Description |
|---|---|
| Platform Independent | Code runs on any platform with JVM |
| Object-Oriented | Based on classes, objects, inheritance, and polymorphism |
| Secure | Bytecode verification, sandbox execution, no pointer arithmetic |
| Robust | Strong memory management and exception handling |
| Multithreading | Supports concurrent thread execution |
| Portable | Same code can be moved between different platforms |
| Architecture Neutral | Bytecode is not tied to any specific processor architecture |
| Interpreted | Bytecode is interpreted by the JVM at runtime |
| High Performance | JIT compiler provides near-native performance |
| Dynamic | Supports dynamic loading of classes and libraries |

---

## Where is Java Used?

- **Mobile Applications** – Android app development
- **Enterprise Applications** – Banking, healthcare, insurance systems
- **Web Applications** – Backend services using Spring, Jakarta EE
- **Desktop Applications** – GUI-based software using JavaFX, Swing
- **Embedded Systems** – SIM cards, Blu-ray players, TV set-top boxes
- **Big Data Technologies** – Hadoop, Spark, Kafka
- **Cloud Applications** – Microservices and serverless computing
- **Scientific Applications** – Numerical analysis and scientific research tools

---

## JVM (Java Virtual Machine)

JVM is the runtime engine that executes Java bytecode. It is a part of the **JRE (Java Runtime Environment)**.

- JVM converts **bytecode** (.class files) into **machine-specific code**.
- It provides platform independence by acting as an abstract layer between the compiled code and the operating system.
- JVM performs **memory management**, **garbage collection**, and **security verification**.

### How JVM Works:

1. **Class Loader** – Loads the .class file into memory.
2. **Bytecode Verifier** – Checks the bytecode for security violations.
3. **Execution Engine** – Interprets or compiles bytecode to native code using the JIT compiler.

---

## JDK (Java Development Kit)

JDK is a software development kit used to develop Java applications. It includes:

- **JRE (Java Runtime Environment)**
- **Compiler (javac)**
- **Debugger**
- **Java Documentation (javadoc)**
- **Jar (Java Archive tool)**

JDK is needed for **developing** Java programs. It is available in multiple editions:

- **Java SE (Standard Edition)** – Core Java development
- **Java EE (Enterprise Edition)** – Enterprise and web application development
- **Java ME (Micro Edition)** – Mobile and embedded device development

---

## JRE (Java Runtime Environment)

JRE is a subset of JDK that is required to **run** Java applications.

- It contains the **JVM**, **core libraries**, and other supporting files.
- It does **not** contain the compiler or other development tools.
- JRE is sufficient for users who only need to run Java programs without developing them.

---

## Difference Between JVM, JDK, and JRE

| Component | Purpose |
|---|---|
| JVM | Executes Java bytecode at runtime |
| JRE | Provides JVM + libraries to run Java programs |
| JDK | Provides JRE + development tools to build Java programs |

---

## Summary

Java is one of the most popular and widely used programming languages in the world. Its platform independence, security, and robustness make it a preferred choice for developers across industries. Understanding JVM, JDK, and JRE is essential for getting started with Java development.
