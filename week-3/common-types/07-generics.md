# Generics in Java

---

## What are Generics?

Generics enable methods, classes, and interfaces to work with **any data type** in a consistent way. Instead of writing separate code for each type, you write one generic code that works for all types.

They provide **type safety**, **eliminate casting**, and make code **reusable**.

---

## Why Use Generics?

- **Stronger type checks** at compile time
- **No need to cast** objects manually
- **Generic algorithms** that work with any type
- **Code reusability** — write once, use with any type

---

## Without Generics (Problem)

```java
import java.util.ArrayList;
import java.util.List;

public class NoGenericsDemo {
    public static void main(String[] args) {
        List list = new ArrayList();

        list.add("Hello");
        list.add(123);         // no compile error — list accepts anything
        list.add(true);

        // Must cast to use — error prone
        String s = (String) list.get(0);
        // Integer i = (Integer) list.get(1);  // works
        // String x = (String) list.get(1);    // ClassCastException at runtime!
    }
}
```

### Problem:
- No type safety
- Must manually cast every element
- Runtime errors if wrong cast

---

## With Generics (Solution)

```java
import java.util.ArrayList;
import java.util.List;

public class GenericsDemo {
    public static void main(String[] args) {
        List<String> strings = new ArrayList<>();

        strings.add("Hello");
        // strings.add(123);   // COMPILE ERROR — only String allowed

        String s = strings.get(0);   // no casting needed
        System.out.println(s.toUpperCase());
    }
}
```

### Output:
```
HELLO
```

---

## Generic Class

A class that can work with any data type.

```java
public class Box<T> {
    private T content;

    public void set(T content) {
        this.content = content;
    }

    public T get() {
        return content;
    }

    public static void main(String[] args) {
        Box<String> stringBox = new Box<>();
        stringBox.set("Hello");
        System.out.println(stringBox.get());

        Box<Integer> intBox = new Box<>();
        intBox.set(100);
        System.out.println(intBox.get());
    }
}
```

### Output:
```
Hello
100
```

### Multiple Type Parameters

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() { return key; }
    public V getValue() { return value; }

    public static void main(String[] args) {
        Pair<String, Integer> student = new Pair<>("Alice", 20);
        System.out.println(student.getKey() + " : " + student.getValue());
    }
}
```

### Output:
```
Alice : 20
```

---

## Generic Method

A method that can accept parameters of any type.

```java
public class GenericMethodDemo {

    // Generic method
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.print(element + " ");
        }
        System.out.println();
    }

    public static void main(String[] args) {
        Integer[] nums = {1, 2, 3, 4, 5};
        String[] names = {"Alice", "Bob", "Charlie"};

        printArray(nums);     // 1 2 3 4 5
        printArray(names);    // Alice Bob Charlie
    }
}
```

### Output:
```
1 2 3 4 5
Alice Bob Charlie
```

---

## Bounded Type Parameters

Restrict the types that can be used with generics.

### Upper Bound (extends)
```java
// Only Number or its subclasses (Integer, Double, etc.)
public static <T extends Number> double sum(List<T> list) {
    double total = 0;
    for (T num : list) {
        total += num.doubleValue();
    }
    return total;
}

public static void main(String[] args) {
    List<Integer> nums = List.of(1, 2, 3, 4, 5);
    System.out.println("Sum: " + sum(nums));
}
```

### Output:
```
Sum: 15.0
```

### Multiple Bounds
```java
// T must implement both Comparable and Serializable
public class MaxFinder<T extends Comparable<T> & java.io.Serializable> {
    private T value;

    public MaxFinder(T value) {
        this.value = value;
    }

    public T getMax(T other) {
        return value.compareTo(other) >= 0 ? value : other;
    }
}
```

---

## Wildcards

### ? (Unbounded Wildcard)
```java
public static void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

### ? extends (Upper Bounded)
```java
// Accepts Number and its subclasses
public static double sumList(List<? extends Number> list) {
    double total = 0;
    for (Number num : list) {
        total += num.doubleValue();
    }
    return total;
}
```

### ? super (Lower Bounded)
```java
// Accepts Integer or its superclasses (Number, Object)
public static void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
    list.add(3);
}
```

---

## Generic Interface

```java
interface Repository<T> {
    void save(T item);
    T findById(int id);
}

class UserRepository implements Repository<String> {
    @Override
    public void save(String item) {
        System.out.println("Saving: " + item);
    }

    @Override
    public String findById(int id) {
        return "User_" + id;
    }
}

public class Main {
    public static void main(String[] args) {
        UserRepository repo = new UserRepository();
        repo.save("Alice");
        System.out.println("Found: " + repo.findById(1));
    }
}
```

### Output:
```
Saving: Alice
Found: User_1
```

---

## Real-World Example — Generic Stack

```java
import java.util.ArrayList;
import java.util.List;

public class Stack<T> {
    private List<T> elements = new ArrayList<>();

    public void push(T item) {
        elements.add(item);
    }

    public T pop() {
        if (elements.isEmpty()) {
            throw new RuntimeException("Stack is empty");
        }
        return elements.remove(elements.size() - 1);
    }

    public T peek() {
        return elements.get(elements.size() - 1);
    }

    public boolean isEmpty() {
        return elements.isEmpty();
    }

    public static void main(String[] args) {
        Stack<Integer> stack = new Stack<>();
        stack.push(10);
        stack.push(20);
        stack.push(30);

        System.out.println("Top: " + stack.peek());   // 30
        System.out.println("Popped: " + stack.pop()); // 30
        System.out.println("Popped: " + stack.pop()); // 20
    }
}
```

### Output:
```
Top: 30
Popped: 30
Popped: 20
```

---

## Quick Reference

| Syntax | Meaning |
|---|---|
| `<T>` | Generic type parameter |
| `<T, V>` | Multiple type parameters |
| `<T extends Number>` | Upper bounded (Number or subclass) |
| `<T extends A & B>` | Multiple bounds |
| `<?>` | Unbounded wildcard |
| `<? extends T>` | Upper bounded wildcard |
| `<? super T>` | Lower bounded wildcard |

---

## Key Points to Remember

- Generics provide **compile-time type safety**.
- Use `<T>` for single type, `<T, V>` for multiple types.
- Use `extends` to restrict types (upper bound).
- Use `super` to restrict from below (lower bound).
- Wildcards (`?`) make APIs more flexible.
- Generics are erased at runtime (type erasure).
- Always use generics with collections for cleaner, safer code.
