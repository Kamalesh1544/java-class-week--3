# Polymorphism in Java

---

## What is Polymorphism?

Polymorphism means **"many forms"**. It allows objects of different classes to be treated as objects of a common parent class. The same method call can behave differently depending on the actual object type.

---

## Two Types of Polymorphism

| Type | Also Called | When |
|---|---|---|
| Compile-time | Static / Method Overloading | Same method name, different parameters (resolved at compile time) |
| Runtime | Dynamic / Method Overriding | Same method name, same parameters in parent-child (resolved at runtime) |

---

## 1. Compile-Time Polymorphism (Method Overloading)

Multiple methods with the **same name** but **different parameter lists** in the same class.

```java
class Calculator {
    // add with 2 ints
    int add(int a, int b) {
        return a + b;
    }

    // add with 3 ints (overloaded)
    int add(int a, int b, int c) {
        return a + b + c;
    }

    // add with doubles (overloaded)
    double add(double a, double b) {
        return a + b;
    }

    public static void main(String[] args) {
        Calculator calc = new Calculator();

        System.out.println(calc.add(10, 20));          // 30
        System.out.println(calc.add(10, 20, 30));      // 60
        System.out.println(calc.add(10.5, 20.5));      // 31.0
    }
}
```

### Output:
```
30
60
31.0
```

### Rules for Overloading:
- Method name must be the **same**
- Parameter list must be **different** (number, type, or order)
- Return type **can** be different (but alone is not enough to overload)

---

## 2. Runtime Polymorphism (Method Overriding)

Child class provides its **own implementation** of a method from the parent class. The JVM decides which method to call at **runtime** based on the actual object type.

```java
class Animal {
    void sound() {
        System.out.println("Some generic sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}

class Cat extends Animal {
    @Override
    void sound() {
        System.out.println("Meow");
    }
}

class Cow extends Animal {
    @Override
    void sound() {
        System.out.println("Moo");
    }
}

public class Main {
    public static void main(String[] args) {
        Animal a1 = new Dog();
        Animal a2 = new Cat();
        Animal a3 = new Cow();

        a1.sound();   // Bark
        a2.sound();   // Meow
        a3.sound();   // Moo
    }
}
```

### Output:
```
Bark
Meow
Moo
```

---

## Overloading vs Overriding

| Feature | Overloading | Overriding |
|---|---|---|
| Also called | Compile-time polymorphism | Runtime polymorphism |
| Class | Same class (or parent-child) | Parent-child relationship |
| Method name | Same | Same |
| Parameters | Must be different | Must be same |
| Return type | Can be different | Must be same or covariant |
| `@Override` | Not used | Used (recommended) |
| Binding | Static (compile time) | Dynamic (runtime) |

---

## Example — Real World

```java
class Payment {
    void pay(double amount) {
        System.out.println("Paying Rs." + amount + " (generic)");
    }
}

class CreditCard extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paying Rs." + amount + " via Credit Card");
    }
}

class UPI extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paying Rs." + amount + " via UPI");
    }
}

class Cash extends Payment {
    @Override
    void pay(double amount) {
        System.out.println("Paying Rs." + amount + " via Cash");
    }
}

public class Main {
    public static void main(String[] args) {
        Payment[] payments = {new CreditCard(), new UPI(), new Cash()};

        for (Payment p : payments) {
            p.pay(500);
        }
    }
}
```

### Output:
```
Paying Rs.500.0 via Credit Card
Paying Rs.500.0 via UPI
Paying Rs.500.0 via Cash
```

---

## Key Points to Remember

- **Overloading** = same method name, different parameters (compile-time).
- **Overriding** = same method, same parameters in parent-child (runtime).
- Runtime polymorphism enables **flexible, extensible code**.
- Use `@Override` annotation when overriding methods.
- Use `instanceof` before downcasting to avoid `ClassCastException`.
- Polymorphism is the foundation of **design patterns** and ** frameworks**.
