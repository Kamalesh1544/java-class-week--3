# Access Modifiers in Java

---

## What is an Access Modifier?

Access modifiers control the **visibility and accessibility** of classes, variables, methods, and constructors. They determine **who can access** what part of your code.

Java has **4 access modifiers**:

| Modifier | Same Class | Same Package | Subclass (different package) | Everywhere |
|---|---|---|---|---|
| `public` | Yes | Yes | Yes | Yes |
| `protected` | Yes | Yes | Yes | No |
| `default` (no keyword) | Yes | Yes | No | No |
| `private` | Yes | No | No | No |

---

## 1. public

Accessible from **everywhere** — any class, any package.

### Syntax
```java
public class MyClass {
    public int count = 10;

    public void display() {
        System.out.println("Public method");
    }
}
```

### Example
```java
// File: com/app/MyClass.java
package com.app;

public class MyClass {
    public String name = "Java";
}

// File: Main.java
import com.app.MyClass;

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        System.out.println(obj.name);   // accessible
    }
}
```

---

## 2. protected

Accessible within the **same package** and by **subclasses** in other packages.

### Syntax
```java
public class Animal {
    protected String type = "Animal";

    protected void breathe() {
        System.out.println("Breathing...");
    }
}
```

### Example
```java
// File: com/animals/Animal.java
package com.animals;

public class Animal {
    protected String sound = "Generic sound";
}

// File: com/dogs/Dog.java
package com.dogs;
import com.animals.Animal;

public class Dog extends Animal {
    public void makeSound() {
        System.out.println(sound);   // accessible — subclass
    }
}

// File: Main.java
import com.animals.Animal;

public class Main {
    public static void main(String[] args) {
        Animal a = new Animal();
        // System.out.println(a.sound);   // ERROR — different package, not a subclass
    }
}
```

---

## 3. default (No Modifier)

When no access modifier is specified, it is called **package-private** or **default**. Accessible only within the **same package**.

### Syntax
```java
class MyClass {
    int count = 10;       // default access

    void display() {      // default access
        System.out.println("Default method");
    }
}
```

### Example
```java
// File: com/utils/Calculator.java
package com.utils;

class Calculator {            // default — cannot be accessed outside package
    int add(int a, int b) {   // default method
        return a + b;
    }
}

// File: Main.java
import com.utils.Calculator;

public class Main {
    public static void main(String[] args) {
        // Calculator c = new Calculator();   // ERROR — default class not accessible
    }
}
```

---

## 4. private

Accessible only within the **same class**. Not visible to any other class.

### Syntax
```java
public class BankAccount {
    private double balance = 1000.00;

    private void deposit(double amount) {
        balance += amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

### Example
```java
public class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Alice");
        // System.out.println(p.name);    // ERROR — private not accessible
        System.out.println(p.getName());   // OK — public getter method
    }
}
```

---

## Use Cases

| Modifier | When to Use |
|---|---|
| `public` | API methods, utility classes, constants |
| `protected` | Base class methods that subclasses may override |
| `default` | Helper classes and methods used only within a package |
| `private` | Internal implementation details, sensitive data |

---

## Encapsulation Example (Using private + getters/setters)

```java
public class Student {
    private String name;
    private int age;

    // Getter for name
    public String getName() {
        return name;
    }

    // Setter for name
    public void setName(String name) {
        this.name = name;
    }

    // Getter for age
    public int getAge() {
        return age;
    }

    // Setter for age with validation
    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        } else {
            System.out.println("Invalid age");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        s.setName("Bob");
        s.setAge(20);

        System.out.println("Name: " + s.getName());
        System.out.println("Age: " + s.getAge());
    }
}
```

### Output:
```
Name: Bob
Age: 20
```

---

## Key Points to Remember

- Use `private` for fields and helper methods.
- Use `public` for methods that form the class's API.
- Use `protected` for methods subclasses need to override.
- Use `default` for package-level utility classes.
- Encapsulation: make fields `private` and provide `public` getters/setters.
