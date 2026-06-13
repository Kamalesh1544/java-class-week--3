# Variables in Java

---

## What is a Variable?

A variable is a **named container** that stores a value in memory. Think of it like a labeled box where you can put something inside and refer to it by the label name.

In Java, every variable has:
- A **name** (identifier)
- A **data type** (what kind of value it holds)
- A **value** (the actual data stored)

---

## Syntax

```java
dataType variableName = value;
```

---

## Types of Variables

### 1. Local Variables
- Declared inside a method, constructor, or block
- No default value — must be initialized before use
- Destroyed when the method ends

```java
public void greet() {
    String name = "Alice";   // local variable
    int age = 25;            // local variable
    System.out.println("Hello " + name + ", age: " + age);
}
```

### 2. Instance Variables (Non-Static)
- Declared inside a class but outside any method
- Gets a **default value** (0, false, null, etc.)
- Belongs to each object (instance) of the class

```java
public class Student {
    String name;    // instance variable — default is null
    int rollNo;     // instance variable — default is 0

    public void display() {
        System.out.println(name + " " + rollNo);
    }
}
```

### 3. Static Variables (Class Variables)
- Declared with the `static` keyword
- Shared across all objects of the class
- Only one copy exists in memory

```java
public class Counter {
    static int count = 0;   // static variable

    public Counter() {
        count++;
    }

    public static void main(String[] args) {
        Counter c1 = new Counter();
        Counter c2 = new Counter();
        System.out.println(Counter.count);  // Output: 2
    }
}
```

### 4. Final Variables
- Declared with the `final` keyword
- Value **cannot be changed** once assigned

```java
final double PI = 3.14159;
// PI = 3.0;   // ERROR: cannot assign a value to final variable
```

---

## Naming Rules

| Rule | Example |
|---|---|
| Must start with a letter, $, or _ | `name`, `$price`, `_count` |
| Cannot use Java keywords | `int class = 5;` is INVALID |
| Case-sensitive | `age` and `Age` are different |
| No spaces | `first name` is INVALID |

---

## Example Program

```java
public class VariablesDemo {
    public static void main(String[] args) {
        // Local variables
        int num = 10;
        double price = 99.99;
        char grade = 'A';
        boolean passed = true;
        String city = "Chennai";

        System.out.println("Number: " + num);
        System.out.println("Price: " + price);
        System.out.println("Grade: " + grade);
        System.out.println("Passed: " + passed);
        System.out.println("City: " + city);
    }
}
```

### Output:
```
Number: 10
Price: 99.99
Grade: A
Passed: true
City: Chennai
```

---

## Key Points to Remember

- Always initialize variables before using them.
- Use meaningful names like `studentName` instead of `x`.
- Local variables require explicit initialization.
- Instance and static variables get default values automatically.
- Use `final` for constants.
