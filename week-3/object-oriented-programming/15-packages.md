# Packages in Java

---

## What is a Package?

A package is a **namespace** that groups related classes and interfaces together. It is like a **folder** that organizes your code to avoid naming conflicts and make it easier to manage.

---

## Why Use Packages?

- **Avoid naming conflicts** — Two classes can have the same name if they are in different packages
- **Code organization** — Group related classes together
- **Access control** — Packages define the scope for access modifiers
- **Reusability** — Packages can be imported and reused across projects

---

## Types of Packages

### 1. Built-in Packages (Java API)
Provided by Java — you just import and use them.

```java
import java.util.ArrayList;
import java.io.File;
import java.sql.Connection;
```

### 2. User-defined Packages
Created by the programmer to organize their own code.

```java
package com.myapp.utils;

public class Helper {
    public static void greet() {
        System.out.println("Hello from Helper");
    }
}
```

---

## Creating a Package

Use the `package` keyword at the **top of the file** (before any imports or class definitions).

```java
package com.school.student;

public class Student {
    String name;

    public Student(String name) {
        this.name = name;
    }

    public void display() {
        System.out.println("Student: " + name);
    }
}
```

### Folder Structure:
```
com/
└── school/
    └── student/
        └── Student.java
```

---

## Importing Packages

Use the `import` keyword to use classes from other packages.

### Import Single Class
```java
import com.school.student.Student;
```

### Import All Classes (wildcard *)
```java
import com.school.student.*;
```

### Import Static Members
```java
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

public class Calc {
    public static void main(String[] args) {
        System.out.println(PI);        // 3.141592653589793
        System.out.println(sqrt(16));  // 4.0
    }
}
```

---

## Package Naming Convention

- All **lowercase**
- Reversed domain name as prefix

```
com.google.utils
org.apache.commons
in.edu.school
```

---

## Accessing Classes from Other Packages

```java
// File: com/school/Student.java
package com.school;

public class Student {
    public String name = "Alice";
}
```

```java
// File: Main.java
import com.school.Student;

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        System.out.println(s.name);
    }
}
```