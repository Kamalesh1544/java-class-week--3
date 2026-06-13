# Inheritance in Java

---

## What is Inheritance?

Inheritance is a mechanism where a **child class (subclass)** acquires the **fields and methods** of a **parent class (superclass)**. It promotes code reusability.

Think of it like a child inheriting properties from parents — the child gets what the parents already have, plus can have its own unique features.

---

## Syntax

```java
class Parent {
    // parent class members
}

class Child extends Parent {
    // child class members (plus inherited ones)
}
```

---

## Basic Example

```java
class Animal {
    String name;

    void eat() {
        System.out.println(name + " is eating");
    }
}

class Dog extends Animal {
    void bark() {
        System.out.println(name + " is barking");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog d = new Dog();
        d.name = "Buddy";
        d.eat();     // inherited method
        d.bark();    // own method
    }
}
```

### Output:
```
Buddy is eating
Buddy is barking
```

---

## Types of Inheritance in Java

### 1. Single Inheritance
```
    Animal
       ↑
      Dog
```
```java
class Animal { void eat() {} }
class Dog extends Animal { void bark() {} }
```

### 2. Multilevel Inheritance
```
    Animal
       ↑
      Dog
       ↑
  Puppy
```
```java
class Animal { void eat() {} }
class Dog extends Animal { void bark() {} }
class Puppy extends Dog { void cry() {} }
```

### 3. Hierarchical Inheritance
```
      Animal
       ↑ ↑
     Dog  Cat
```
```java
class Animal { void eat() {} }
class Dog extends Animal { void bark() {} }
class Cat extends Animal { void meow() {} }
```

### 4. Multiple Inheritance — NOT supported with classes
Java does **not allow** a class to extend more than one class.

```java
// INVALID — Java does not support this
class A {}
class B {}
class C extends A, B {}   // ERROR
```

> Multiple inheritance is supported through **Interfaces** (covered later).
