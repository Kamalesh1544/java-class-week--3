# Working with Objects in Java

---

## Creating Objects

Objects are created using the `new` keyword along with a constructor.

### Syntax
```java
ClassName obj = new ClassName();
```

### With Parameters
```java
ClassName obj = new ClassName(arg1, arg2);
```

---

## Accessing Fields

Use the **dot operator** (`.`) to access and modify fields.

```java
public class Book {
    String title;
    double price;

    public static void main(String[] args) {
        Book b = new Book();
        b.title = "Java Programming";   // set field
        b.price = 499.99;               // set field

        System.out.println(b.title);    // get field
        System.out.println(b.price);    // get field
    }
}
```

### Output:
```
Java Programming
499.99
```

---

## Calling Methods

Use the dot operator to call methods on an object.

```java
public class Calculator {
    int result;

    public void add(int a, int b) {
        result = a + b;
    }

    public void multiply(int a, int b) {
        result = a * b;
    }

    public void show() {
        System.out.println("Result: " + result);
    }

    public static void main(String[] args) {
        Calculator calc = new Calculator();

        calc.add(10, 20);
        calc.show();         // Result: 30

        calc.multiply(5, 6);
        calc.show();         // Result: 30
    }
}
```

### Output:
```
Result: 30
Result: 30
```

---

## Passing Objects as Parameters

Objects can be passed to methods just like primitives.

```java
public class Point {
    int x, y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public void display() {
        System.out.println("(" + x + ", " + y + ")");
    }

    public double distanceTo(Point other) {
        int dx = this.x - other.x;
        int dy = this.y - other.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    public static void main(String[] args) {
        Point p1 = new Point(0, 0);
        Point p2 = new Point(3, 4);

        p1.display();                          // (0, 0)
        p2.display();                          // (3, 4)
        System.out.println("Distance: " + p1.distanceTo(p2));  // 5.0
    }
}
```

### Output:
```
(0, 0)
(3, 4)
Distance: 5.0
```

---

## Returning Objects from Methods

Methods can return objects.

```java
public class Student {
    String name;
    int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void display() {
        System.out.println(name + " (age " + age + ")");
    }

    public static Student createStudent(String name, int age) {
        return new Student(name, age);   // returning a new object
    }

    public static void main(String[] args) {
        Student s1 = createStudent("Alice", 20);
        Student s2 = createStudent("Bob", 22);

        s1.display();   // Alice (age 20)
        s2.display();   // Bob (age 22)
    }
}
```

### Output:
```
Alice (age 20)
Bob (age 22)
```

---

## Comparing Objects

### Using == (Reference Comparison)
```java
Student s1 = new Student("Alice", 20);
Student s2 = new Student("Alice", 20);
Student s3 = s1;

System.out.println(s1 == s2);   // false (different objects)
System.out.println(s1 == s3);   // true (same object)
```

### Using equals() (Value Comparison)
Override `equals()` to compare by values.

```java
public class Student {
    String name;
    int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (obj == null || getClass() != obj.getClass()) return false;
        Student other = (Student) obj;
        return age == other.age && name.equals(other.name);
    }

    public static void main(String[] args) {
        Student s1 = new Student("Alice", 20);
        Student s2 = new Student("Alice", 20);
        Student s3 = new Student("Bob", 22);

        System.out.println(s1.equals(s2));   // true
        System.out.println(s1.equals(s3));   // false
    }
}
```

---

## Array of Objects

```java
public class Player {
    String name;
    int score;

    public Player(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public void display() {
        System.out.println(name + ": " + score);
    }

    public static void main(String[] args) {
        Player[] team = {
            new Player("Alice", 100),
            new Player("Bob", 85),
            new Player("Charlie", 92)
        };

        System.out.println("--- Team ---");
        for (Player p : team) {
            p.display();
        }
    }
}
```

### Output:
```
--- Team ---
Alice: 100
Bob: 85
Charlie: 92
```

---

## Key Points to Remember

- Use `.` to access fields and call methods.
- Objects can be passed to and returned from methods.
- `==` compares references; `equals()` compares values (override it).
- Arrays can hold multiple objects.
- Objects are reference types stored on the Heap.
