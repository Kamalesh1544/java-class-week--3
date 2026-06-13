# Interfaces in Java

---

## What is an Interface?

An interface is a **contract** that defines what a class must do, without specifying how it does it. It contains **abstract methods** (methods without body) that implementing classes must override.

An interface achieves **100% abstraction** and supports **multiple inheritance** (which classes don't support).

---

## Syntax

```java
interface InterfaceName {
    // abstract method (public and abstract by default)
    returnType methodName();

}
```

### Implementing an Interface:
```java
class ClassName implements InterfaceName {
    
    public returnType methodName() {
        // implementation
    }
}
```

---

## Basic Example

```java
interface Drawable {
    void draw();   // abstract by default
}

class Circle implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing circle");
    }
}

class Square implements Drawable {
    @Override
    public void draw() {
        System.out.println("Drawing square");
    }
}

public class Main {
    public static void main(String[] args) {
        Circle c = new Circle();
        Square s = new Square();

        c.draw();   // Drawing circle
        s.draw();   // Drawing square
    }
}
```

### Output:
```
Drawing circle
Drawing square
```

---

## Multiple Interfaces (Multiple Inheritance)

A class can implement **multiple interfaces**.

```java
interface Flyable {
    void fly();
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}

public class Main {
    public static void main(String[] args) {
        Duck d = new Duck();
        d.fly();
        d.swim();
    }
}
```

### Output:
```
Duck is flying
Duck is swimming
```

