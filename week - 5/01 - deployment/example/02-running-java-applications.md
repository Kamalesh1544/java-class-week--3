# Running Java Applications

---

## Compiling Java Programs

When you write Java code, you write it in a `.java` file (source code). But the computer cannot understand `.java` files directly. You need to **compile** them into `.class` files (bytecode) that the JVM can understand.

### The Compilation Process:

```
Main.java  →  [javac compiler]  →  Main.class  →  [JVM]  →  Output
(source code)                      (bytecode)               (result)
```

### How to compile:
```bash
javac Main.java
```

This creates a `Main.class` file in the same directory.

### What happens during compilation:
1. `javac` reads your `.java` file
2. Checks for syntax errors (wrong types, missing semicolons, etc.)
3. Converts valid code into **bytecode** (`.class` file)
4. Bytecode is platform-independent — it runs on any device with a JVM

### If there are errors:
```bash
javac Main.java
# Output:
# Main.java:5: error: cannot find symbol
#     System.out.println(x);
#                         ^
#   symbol:   variable x
# 1 error
```

The compiler tells you exactly what is wrong and on which line. Fix the error and recompile.

---

## Running Java Programs from Command Line

After compilation, you use the `java` command to run the `.class` file.

### Basic syntax:
```bash
java ClassName
```

### Example:
```bash
# Compile
javac Main.java

# Run
java Main
```

### Important notes:
- Use the **class name** without `.class` extension
- The class must have a `main()` method
- You must be in the same directory (or specify classpath)

### Running a JAR file:
```bash
java -jar myapp.jar
```

---

## What Happens When You Run a Java Program

```
java Main
│
├── 1. JVM starts
├── 2. Class Loader loads Main.class into memory
├── 3. Bytecode Verifier checks for security issues
├── 4. JVM looks for main() method
├── 5. main() method executes
├── 6. Program runs until main() finishes
└── 7. JVM shuts down
```

---

## Classpath

The classpath is a **setting that tells the JVM where to look for `.class` files and JAR files**. When your program uses other classes (from your project or libraries), the JVM needs to know where to find them.

### Default classpath:
- Current directory (`.`)
- JVM looks for classes in the folder you are running the command from

### Setting classpath:

#### Temporary (for current session):
```bash
# Windows
set CLASSPATH=C:\mylibs\library.jar;.;

# Linux/Mac
export CLASSPATH=/mylibs/library.jar:.
```

#### Command-line flag:
```bash
java -cp C:\mylibs\library.jar:. Main
```

The `-cp` flag sets the classpath for that specific run.

### Classpath syntax:
```
Windows:    C:\path\to\lib\jar1.jar;C:\path\to\lib\jar2.jar;.
Linux/Mac:  /path/to/lib/jar1.jar:/path/to/lib/jar2.jar:.
```

The `.` at the end means "also look in the current directory."

---

## Environment Variables

Environment variables are **system-wide settings** that tell your computer where to find Java tools and libraries.

### JAVA_HOME
Points to the directory where JDK is installed.

#### Setting JAVA_HOME (Windows):
```
1. Right-click "This PC" → Properties
2. Advanced System Settings → Environment Variables
3. Click "New" under System Variables
4. Variable name: JAVA_HOME
5. Variable value: C:\Program Files\Java\jdk-17
6. Click OK
```

#### Setting JAVA_HOME (Linux/Mac):
```bash
# Add to ~/.bashrc or ~/.zshrc
export JAVA_HOME=/usr/lib/jvm/java-17
export PATH=$JAVA_HOME/bin:$PATH
```

### PATH
Tells the system where to find executable programs. When you type `java` or `javac`, the system searches through PATH directories to find them.

#### Adding Java to PATH (Windows):
```
1. Environment Variables → Path → Edit
2. Add: C:\Program Files\Java\jdk-17\bin
3. Click OK
```

### CLASSPATH
Tells the JVM where to find class files and libraries.

### Why environment variables matter:

| Variable | Purpose |
|---|---|
| `JAVA_HOME` | Tells tools where JDK is installed |
| `PATH` | Lets you run `java`, `javac` from any directory |
| `CLASSPATH` | Tells JVM where to find your classes and libraries |

---

## JVM Basics (How It Works)

The JVM is the engine that runs your Java program. Here is what happens step by step:

### Step 1: Class Loading
```
Class Loader
│
├── Reads Main.class file
├── Checks if the class is valid
├── Loads it into memory (Method Area)
└── Creates a Class object for it
```

### Step 2: Bytecode Verification
```
Bytecode Verifier
│
├── Checks if the bytecode is valid
├── Ensures no security violations
├── Verifies stack is not corrupted
└── Confirms memory access is safe
```

### Step 3: Execution Engine
```
Execution Engine
│
├── Interpreter — reads and executes bytecode line by line
├── JIT Compiler — converts frequently used bytecode to native machine code
│   └── This makes Java faster after initial runs
└── Garbage Collector — automatically frees unused memory
```

### JVM Memory Layout:
```
JVM Memory
│
├── Method Area — stores class information, static variables
├── Heap — stores objects and instance variables
├── Stack — stores local variables and method calls (per thread)
└── PC Register — stores address of current instruction (per thread)
```

---

## Main Method Execution

The `main()` method is the **entry point** of every Java application. When you run a Java program, the JVM looks for this specific method:

```java
public static void main(String[] args)
```

### Breaking down each part:

| Part | Meaning |
|---|---|
| `public` | Access modifier — JVM must be able to access it from outside |
| `static` | JVM calls it without creating an object of the class |
| `void` | The main method does not return any value |
| `main` | The method name — JVM specifically looks for this name |
| `String[] args` | Array of strings — command-line arguments passed to the program |

### What happens when you run `java Main`:

```
1. JVM looks for Main.class
2. JVM searches for: public static void main(String[] args)
3. If found → execution begins
4. If not found → error: "Main method not found in class Main"
```

### Command-line arguments:

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Arguments count: " + args.length);

        for (int i = 0; i < args.length; i++) {
            System.out.println("args[" + i + "] = " + args[i]);
        }
    }
}
```

```bash
# Run with arguments
java Main hello world 123
```

### Output:
```
Arguments count: 3
args[0] = hello
args[1] = world
args[2] = 123
```

### Multiple main methods:

If you have multiple classes with `main()` methods, you specify which one to run:

```bash
# Run Main class
java Main

# Run Test class
java Test
```

The JVM runs the `main()` method of the class you specify.

---

## Common Errors When Running

| Error | Cause | Solution |
|---|---|---|
| `ClassNotFoundException` | JVM cannot find the .class file | Check classpath |
| `NoClassDefFoundError` | Class was available during compilation but not at runtime | Include all required classes/JARs |
| `Main method not found` | The class does not have a valid main method | Check method signature |
| `Could not find or load main class` | Wrong class name or classpath issue | Verify class name and path |

---

## Key Points to Remember

- Use `javac` to compile `.java` → `.class`
- Use `java ClassName` to run (without `.class` extension)
- Classpath tells JVM where to find classes
- `JAVA_HOME` points to JDK installation directory
- `PATH` lets you run Java commands from anywhere
- JVM loads, verifies, and executes bytecode
- The `main()` method is the entry point of every Java application
- `String[] args` receives command-line arguments
