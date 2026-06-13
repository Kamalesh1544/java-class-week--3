# Classes vs Objects in Java

---

## Definition

| Term | Definition |
|---|---|
| **Class** | A blueprint or template that defines the structure and behavior of objects |
| **Object** | A real-world instance created from the class blueprint |

A **class** is like an **architectural plan** for a house. An **object** is the **actual house** built from that plan.

---

## Class — The Blueprint

A class defines:
- **Fields** (attributes/properties) — what data an object will have
- **Methods** (behaviors) — what actions an object can perform
- **Constructor** — how to initialize the object

```java
public class Car {
    // Fields (attributes)
    String brand;
    int speed;

    // Constructor
    public Car(String brand, int speed) {
        this.brand = brand;
        this.speed = speed;
    }

    // Methods (behaviors)
    public void drive() {
        System.out.println(brand + " is driving at " + speed + " km/h");
    }
}
```

No memory is allocated when a class is defined. Memory is allocated only when an object is created.

---

## Object — The Instance

An object is a **live entity** in memory with actual values.

```java
Car car1 = new Car("Toyota", 120);
Car car2 = new Car("BMW", 180);

car1.drive();   // Toyota is driving at 120 km/h
car2.drive();   // BMW is driving at 180 km/h
```

---

## Side-by-Side Comparison

| Feature | Class | Object |
|---|---|---|
| What it is | Blueprint / Template | Instance of a class |
| Memory | No memory allocated | Memory allocated (Heap) |
| Created with | `class` keyword | `new` keyword |
| Number | Defined once | Multiple objects from one class |
| Contains | Fields + Methods + Constructor | Actual values + method execution |
| Example | `Car` (the design) | `new Car("Toyota", 120)` (real car) |
| Also called | Data type | Instance / Reference type |

---

## Real-World Analogy

```
Class: Animal
├── Fields: name, type, sound
├── Methods: eat(), sleep(), makeSound()
└── Constructor: Animal(name, type, sound)

Objects:
├── animal1: name="Lion", type="Wild", sound="Roar"
├── animal2: name="Dog", type="Pet", sound="Bark"
└── animal3: name="Cat", type="Pet", sound="Meow"
```

---

## Example — Class and Multiple Objects

```java
public class Employee {
    String name;
    double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void display() {
        System.out.println("Name: " + name + ", Salary: Rs." + salary);
    }

    public void giveRaise(double amount) {
        salary += amount;
        System.out.println(name + " got Rs." + amount + " raise");
    }

    public static void main(String[] args) {
        Employee e1 = new Employee("Alice", 50000);
        Employee e2 = new Employee("Bob", 60000);

        e1.display();   // Name: Alice, Salary: Rs.50000.0
        e2.display();   // Name: Bob, Salary: Rs.60000.0

        e1.giveRaise(5000);   // Alice got Rs.5000.0 raise
        e1.display();          // Name: Alice, Salary: Rs.55000.0
    }
}
```

### Output:
```
Name: Alice, Salary: Rs.50000.0
Name: Bob, Salary: Rs.60000.0
Alice got Rs.5000.0 raise
Name: Alice, Salary: Rs.55000.0
```

---

## Key Points to Remember

- A class is a template; an object is a real instance.
- One class can create unlimited objects.
- Each object has its own copy of instance variables.
- Classes define behavior; objects execute behavior.
- Memory is allocated only when an object is created with `new`.
