# Representing Data

In Java you represent data using classes and objects. Think of a class like a blueprint and an object like the actual thing built from that blueprint. So if you want to represent a student, you create a Student class with fields like name, age, grade and then you create objects from it for each actual student. The class defines what data is there and the objects hold the real values. This is basically how you model real world things in code — you look at what properties something has, put those as fields in a class, and then create objects whenever you need an instance of it. You can also use enums when you have a fixed set of values like days of the week or blood types instead of using random strings. And when you need to store a bunch of similar objects together you use collections like ArrayList or HashMap. The whole idea is to structure your data in a way that makes sense for what you are building so that later when you need to access, update or pass around that data it is organized and easy to work with.

```java
class Student {
    String name;
    int age;
    String grade;

    Student(String name, int age, String grade) {
        this.name = name;
        this.age = age;
        this.grade = grade;
    }

    void display() {
        System.out.println(name + " | Age: " + age + " | Grade: " + grade);
    }
}

// Creating objects
Student s1 = new Student("Alice", 20, "A");
Student s2 = new Student("Bob", 22, "B+");

s1.display();   // Alice | Age: 20 | Grade: A
s2.display();   // Bob | Age: 22 | Grade: B+
```
