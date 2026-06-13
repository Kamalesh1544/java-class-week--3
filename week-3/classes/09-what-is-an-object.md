# What is an Object in Java?

---

## Definition

An object is a **runtime instance** of a class. It is a real-world entity that has:
- **State** (data/attributes) — stored in fields/variables
- **Behavior** (actions) — stored in methods

Think of a class as a **blueprint** and an object as the **actual house** built from that blueprint.

---

## How Objects are Created

Objects are created using the `new` keyword. This calls the **constructor** of the class and allocates memory on the **Heap**.

### Syntax
```java
ClassName objectName = new ClassName();
```

### Example
```java
public class Car {
    String brand;
    int speed;

    public void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h");
    }

    public static void main(String[] args) {
        Car myCar = new Car();    // myCar is the object
        myCar.brand = "Toyota";
        myCar.speed = 120;
        myCar.drive();
    }
}
```

### Output:
```
Toyota is driving at 120 km/h
```

---

## Parts of an Object

```
Car myCar = new Car();
│    │         │
│    │         └── Constructor (creates the object)
│    └──────────── Reference variable (holds the address)
└───────────────── Type (class name)
```

| Part | Description |
|---|---|
| Class Name | The type of object |
| Reference | A variable that points to the object in memory |
| Constructor | Initializes the object |
| Heap Memory | Where the object actually lives |

---

## Object State, Behavior, and Identity

| Concept | Description | Example |
|---|---|---|
| State | Data stored in the object's fields | `brand = "Toyota"` |
| Behavior | What the object can do (methods) | `drive()`, `stop()` |
| Identity | Unique reference (memory address) | Each `new` creates a unique object |

---

## Multiple Objects from Same Class

```java
public class Student {
    String name;
    int age;

    public void study() {
        System.out.println(name + " is studying");
    }

    public static void main(String[] args) {
        Student s1 = new Student();
        s1.name = "Alice";
        s1.age = 20;

        Student s2 = new Student();
        s2.name = "Bob";
        s2.age = 22;

        s1.study();   // Alice is studying
        s2.study();   // Bob is studying
    }
}
```

### Output:
```
Alice is studying
Bob is studying
```

---

## Object Reference vs Object

```java
Student s1 = new Student();
Student s2 = s1;        // s2 points to the SAME object as s1

s1.name = "Alice";
System.out.println(s2.name);   // Alice (both reference same object)

s2.name = "Bob";
System.out.println(s1.name);   // Bob (change reflected in s1 too)
```

### Visual:
```
s1 ──────┐
         ├──→ Object in Heap (name = "Bob")
s2 ──────┘
```

---

## Comparing Objects

### Using == (compares references, not values)
```java
Student s1 = new Student();
Student s2 = new Student();

System.out.println(s1 == s2);   // false (different objects)
```

### Using equals() (compares values if overridden)
```java
String a = new String("Hello");
String b = new String("Hello");

System.out.println(a == b);        // false (different references)
System.out.println(a.equals(b));   // true (same content)
```

---

## Example Program — Real World

```java
public class Phone {
    String brand;
    double price;

    public void call(String number) {
        System.out.println("Calling " + number + "...");
    }

    public void info() {
        System.out.println(brand + " costs Rs." + price);
    }

    public static void main(String[] args) {
        Phone phone1 = new Phone();
        phone1.brand = "Samsung";
        phone1.price = 25000;

        Phone phone2 = new Phone();
        phone2.brand = "Apple";
        phone2.price = 80000;

        phone1.info();           // Samsung costs Rs.25000.0
        phone2.info();           // Apple costs Rs.80000.0
        phone1.call("9876543210"); // Calling 9876543210...
    }
}
```

### Output:
```
Samsung costs Rs.25000.0
Apple costs Rs.80000.0
Calling 9876543210...
```

---

## Key Points to Remember

- An object is an instance of a class created with `new`.
- Each object has its own copy of instance variables.
- Objects are stored on the Heap; references on the Stack.
- Two references can point to the same object.
- Use `==` to compare references, `equals()` to compare content.
- Objects are garbage collected when no references point to them.
