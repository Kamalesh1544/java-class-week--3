# Abstract Classes in Java

---

## What is an Abstract Class?

An abstract class is a class that **cannot be instantiated** (you cannot create objects from it). It serves as a **base class** for other classes to extend. It can have both **abstract methods** (no body) and **concrete methods** (with body).

Use abstract classes when you want to provide a **common template** but some methods should be implemented by each subclass differently.

---

## Syntax

```java
abstract class ClassName {
    // abstract method (no body)
    abstract returnType methodName();

    // concrete method (with body)
    returnType methodName2() {
        // implementation
    }
}
```

---

## Basic Example

```java
abstract class Shape {
    String name;

    Shape(String name) {
        this.name = name;
    }

    abstract double area();    // abstract — no body
    abstract double perimeter(); // abstract — no body

    void display() {           // concrete — has body
        System.out.println("Shape: " + name);
    }
}

class Circle extends Shape {
    double radius;

    Circle(double radius) {
        super("Circle");
        this.radius = radius;
    }

    double area() {
        return Math.PI * radius * radius;
    }

    double perimeter() {
        return 2 * Math.PI * radius;
    }
}

class Rectangle extends Shape {
    double width, height;

    Rectangle(double width, double height) {
        super("Rectangle");
        this.width = width;
        this.height = height;
    }

    
    double area() {
        return width * height;
    }

    
    double perimeter() {
        return 2 * (width + height);
    }
}

public class Main {
    public static void main(String[] args) {
        Shape s1 = new Circle(5);
        Shape s2 = new Rectangle(4, 6);

        s1.display();
        System.out.println("Area: " + String.format("%.2f", s1.area()));
        System.out.println("Perimeter: " + String.format("%.2f", s1.perimeter()));

        System.out.println();

        s2.display();
        System.out.println("Area: " + s2.area());
        System.out.println("Perimeter: " + s2.perimeter());
    }
}
```

### Output:
```
Shape: Circle
Area: 78.54
Perimeter: 31.42

Shape: Rectangle
Area: 24.0
Perimeter: 20.0
```

--