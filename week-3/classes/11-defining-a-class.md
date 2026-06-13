# Defining a Class in Java

---

## What is a Class?

A class is a **user-defined data type** that groups variables (fields) and methods (functions) into a single unit. It serves as a blueprint for creating objects.

---

## Basic Syntax

```java
class ClassName {
    // Fields (instance variables)
    dataType fieldName;

    // Constructor
    public ClassName(parameters) {
        // initialization code
    }

    // Methods
    public returnType methodName(parameters) {
        // method body
    }
}
```

---

## Parts of a Class

### 1. Fields (Instance Variables)
Variables declared inside a class but outside any method.

```java
public class Person {
    String name;      // field
    int age;          // field
    double height;    // field
}
```

### 2. Constructor
A special method that runs when an object is created. Used to initialize fields.

```java
public class Person {
    String name;
    int age;

    // Constructor
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 3. Methods
Functions defined inside a class that describe the object's behavior.

```java
public class Person {
    String name;
    int age;

    public void introduce() {
        System.out.println("Hi, I'm " + name + ", age " + age);
    }

    public boolean isAdult() {
        return age >= 18;
    }
}
```

### 4. The `this` Keyword
Refers to the **current object**. Used to distinguish between fields and parameters with the same name.

```java
public class Person {
    String name;

    public Person(String name) {
        this.name = name;   // this.name = field, name = parameter
    }
}
```

---

## Types of Constructors

### Default Constructor (No parameters)
```java
public class Student {
    String name;

    public Student() {
        name = "Unknown";
    }
}
```

### Parameterized Constructor
```java
public class Student {
    String name;

    public Student(String name) {
        this.name = name;
    }
}
```

### Constructor Overloading (Multiple constructors)
```java
public class Student {
    String name;
    int age;

    public Student() {
        this.name = "Unknown";
        this.age = 0;
    }

    public Student(String name) {
        this.name = name;
        this.age = 0;
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

---

## Methods in Detail

### Method with No Parameters and No Return
```java
public void greet() {
    System.out.println("Hello!");
}
```

### Method with Parameters
```java
public void greet(String name) {
    System.out.println("Hello, " + name + "!");
}
```

### Method with Return Value
```java
public int add(int a, int b) {
    return a + b;
}
```

---

## Complete Class Example

```java
public class BankAccount {
    // Fields
    String owner;
    double balance;

    // Constructor
    public BankAccount(String owner, double balance) {
        this.owner = owner;
        this.balance = balance;
    }

    // Methods
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println("Rs." + amount + " deposited. Balance: Rs." + balance);
        } else {
            System.out.println("Invalid amount");
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Rs." + amount + " withdrawn. Balance: Rs." + balance);
        } else {
            System.out.println("Insufficient funds");
        }
    }

    public void display() {
        System.out.println("Owner: " + owner + " | Balance: Rs." + balance);
    }

    // Main method to test
    public static void main(String[] args) {
        BankAccount acc = new BankAccount("Alice", 10000);
        acc.display();

        acc.deposit(5000);
        acc.withdraw(3000);
        acc.display();
    }
}
```

### Output:
```
Owner: Alice | Balance: Rs.10000.0
Rs.5000.0 deposited. Balance: Rs.15000.0
Rs.3000.0 withdrawn. Balance: Rs.12000.0
Owner: Alice | Balance: Rs.12000.0
```

---

## Access Modifiers in Classes

| Modifier | Class | Subclass | Package | Everywhere |
|---|---|---|---|---|
| `public` | Yes | Yes | Yes | Yes |
| `protected` | Yes | Yes | Yes | No |
| `default` | Yes | Yes | No | No |
| `private` | Yes | No | No | No |

```java
public class Example {
    public int a;       // accessible everywhere
    protected int b;    // accessible in package + subclasses
    int c;              // accessible only in package
    private int d;      // accessible only in this class
}
```

---

## Key Points to Remember

- A class is a blueprint; it does not occupy memory until an object is created.
- Fields define **what data** the object holds.
- Methods define **what the object can do**.
- Constructors initialize objects when they are created.
- Use `this` to refer to the current object's fields.
- Use access modifiers to control visibility.
