# Introduction to Java Application Deployment

---

## What is Deployment?

Deployment is the process of **making your Java application available for end users to use**. After you write and test your code, it needs to be packaged and delivered to the environment where it will actually run — whether that is a user's computer, a server, or the cloud.

Think of it like this: you built a car in a factory (development), now you need to deliver it to the showroom so customers can drive it (deployment).

---

## Development vs Production Environment

These are two completely separate setups:

| Feature | Development Environment | Production Environment |
|---|---|---|
| Purpose | Writing and testing code | Real users use the application |
| Who uses it | Developers | End users |
| Code changes | Frequent changes | Stable, no frequent changes |
| Data | Test/dummy data | Real, live data |
| Debugging | Enabled (can step through code) | Disabled (for performance) |
| Error messages | Detailed (helps developer fix bugs) | Generic (hides internal details from users) |
| Performance | Not optimized (focus is on functionality) | Optimized for speed and reliability |
| Server | Your local machine | Dedicated server or cloud |

In short: **Development** is where you build and test. **Production** is where real users interact with your finished application.

---

## JAR Files (Java ARchive)

A JAR file is a **compressed file** (like a ZIP) that bundles together:
- Compiled `.class` files
- Resources (images, config files, etc.)
- Metadata (manifest file)

JAR files make it easy to distribute and share Java programs. Instead of sending hundreds of individual files, you send one `.jar` file.

### How a JAR is structured:
```
myapp.jar
├── META-INF/
│   └── MANIFEST.MF      ← metadata (main class, version, etc.)
├── com/
│   └── myapp/
│       ├── Main.class
│       ├── Utils.class
│       └── Data.class
└── resources/
    └── config.properties
```

### Creating a JAR file:
```bash
# Step 1: Compile your Java files
javac Main.java Utils.java

# Step 2: Create the JAR
jar cf myapp.jar Main.class Utils.class

# Step 3: Run the JAR
java -jar myapp.jar
```

---

## Executable JARs

A regular JAR file cannot be run directly with `java -jar` unless it is configured as an **executable JAR**. An executable JAR contains a special entry in its manifest file that tells Java which class has the `main()` method.

### Manifest file (MANIFEST.MF):
```
Main-Class: com.myapp.Main
Class-Path: .
```

### How to create an executable JAR:
```bash
# Create JAR with manifest
jar cfe myapp.jar com.myapp.Main *.class

# Run it
java -jar myapp.jar
```

The `e` flag in `jar cfe` specifies the entry point (main class).

---

## Java Runtime Environment (JRE)

The JRE is the software required to **run** Java applications. It contains:

| Component | Purpose |
|---|---|
| JVM (Java Virtual Machine) | Executes Java bytecode |
| Core Libraries | Basic classes (String, Math, collections, etc.) |
| Other Files | Supporting files needed at runtime |

If a user only wants to **run** a Java program (not develop one), they need the JRE. It does not include any development tools like the compiler.

### What you can do with JRE:
- Run Java applications
- Run JAR files

### What you cannot do with JRE:
- Compile Java source code (`.java` files)
- Use `javac` command

---

## Java Development Kit (JDK)

The JDK is the full software kit required to **develop** Java applications. It contains:

| Component | Purpose |
|---|---|
| JRE | Everything needed to run Java programs |
| Compiler (javac) | Compiles `.java` source files to `.class` bytecode |
| Debugger (jdb) | Helps find and fix bugs |
| Javadoc | Generates documentation from code comments |
| JAR tool | Creates and manages JAR files |
| Other tools | Profiler, monitoring tools, etc. |

### What you can do with JDK:
- Compile Java source code
- Run Java applications
- Create JAR files
- Debug applications
- Generate documentation

### What you need JDK for:
- Writing and building Java programs
- Creating executable JARs for distribution

---

## JRE vs JDK

| Feature | JRE | JDK |
|---|---|---|
| Purpose | Run Java programs | Develop and run Java programs |
| Compiler (javac) | No | Yes |
| JVM | Yes | Yes |
| Core Libraries | Yes | Yes |
| Debugger | No | Yes |
| Who needs it | End users | Developers |

> **Simple rule:** If you are developing, install JDK. If you only need to run programs, install JRE.

---

## Deployment Lifecycle

The deployment lifecycle is the step-by-step process of getting your application from development to production.

### Step 1: Development
- Write Java source code (`.java` files)
- Use IDE (IntelliJ, Eclipse) or text editor
- Test locally on your machine

### Step 2: Build
- Compile source code using `javac`
- Run tests to make sure everything works
- Package into a JAR file

```bash
# Build example
javac -d out src/com/myapp/*.java
jar cfe myapp.jar com.myapp.Main -C out .
```

### Step 3: Testing / Staging
- Deploy to a test environment (not production yet)
- Perform quality assurance (QA) testing
- Check for bugs, performance issues, security vulnerabilities
- Get approval from stakeholders

### Step 4: Production Deployment
- Deploy the JAR to the production server
- This could be:
  - A physical server in your office
  - A cloud server (AWS, Azure, GCP)
  - A container (Docker)
- Configure the environment (database connections, ports, etc.)

### Step 5: Monitoring and Maintenance
- Monitor the application for errors and performance
- Collect logs for debugging
- Release updates and patches as needed
- Scale up if user traffic increases

---

## Deployment Methods

| Method | Description |
|---|---|
| Standalone JAR | User downloads and runs the JAR directly |
| Web Application | Deployed on a web server (Tomcat, Jetty) using WAR file |
| Container | Packaged in Docker container and deployed to cloud |
| Cloud Service | Deployed directly to cloud platforms (AWS, Heroku, etc.) |

---

## Example — Complete Deployment Workflow

Here is how a simple application goes from code to deployment:

```java
// File: Main.java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, deployed world!");
    }
}
```

```bash
# 1. Compile
javac Main.java

# 2. Create manifest file
echo "Main-Class: Main" > MANIFEST.MF

# 3. Package into JAR
jar cfm myapp.jar MANIFEST.MF Main.class

# 4. Test locally
java -jar myapp.jar
# Output: Hello, deployed world!

# 5. Distribute myapp.jar to users
# Users just need JRE to run it
```

---

## Key Points to Remember

- **Deployment** = making your app available for users to use.
- **Development** is for building; **Production** is for real users.
- **JAR files** bundle your code into a single distributable file.
- **Executable JARs** can be run directly with `java -jar`.
- **JRE** is for running Java apps (no compiler).
- **JDK** is for developing Java apps (includes compiler).
- The deployment lifecycle: Develop → Build → Test → Deploy → Monitor.
