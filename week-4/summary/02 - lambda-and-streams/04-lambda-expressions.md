# Lambda Expressions in Java

---

## What is a Lambda?

A Lambda is a **short block of code** that takes parameters and returns a value. It is like an anonymous function — a function without a name. Lambdas make your code shorter and more readable, especially when working with collections and functional interfaces.

Think of it as a quick, one-line function you write inline instead of creating a full class or method.

---

## Before Lambda (Anonymous Class)

```java
// Old way — verbose and repetitive
Runnable r = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello");
    }
};
```

## With Lambda (Clean and Short)

```java
// New way — concise
Runnable r = () -> System.out.println("Hello");
```

---

## Lambda Syntax

```
(parameters) -> expression
```

or

```
(parameters) -> { statements; }
```

### Examples:

```java
// No parameters
() -> System.out.println("Hello")

// One parameter (parentheses optional)
x -> x * x
(x) -> x * x

// Two parameters
(a, b) -> a + b

// Multiple statements
(x, y) -> {
    int sum = x + y;
    System.out.println("Sum: " + sum);
    return sum;
}
```

---

## Lambda with Functional Interfaces

A lambda can only be used with a **functional interface** — an interface with exactly **one abstract method**.

### Examples of built-in functional interfaces:

| Interface | Method | Description |
|---|---|---|
| `Runnable` | `run()` | Takes nothing, returns nothing |
| `Callable<V>` | `call()` | Takes nothing, returns V |
| `Comparator<T>` | `compare(T, T)` | Takes two T, returns int |
| `Function<T, R>` | `apply(T)` | Takes T, returns R |
| `Predicate<T>` | `test(T)` | Takes T, returns boolean |
| `Consumer<T>` | `accept(T)` | Takes T, returns nothing |
| `Supplier<T>` | `get()` | Takes nothing, returns T |

### Creating a custom functional interface:

```java
@FunctionalInterface   // optional but recommended
interface MathOperation {
    int operate(int a, int b);
}

public class LambdaDemo {
    public static void main(String[] args) {
        MathOperation add = (a, b) -> a + b;
        MathOperation multiply = (a, b) -> a * b;

        System.out.println(add.operate(5, 3));        // 8
        System.out.println(multiply.operate(5, 3));    // 15
    }
}
```

### Output:
```
8
15
```

---

## Lambda Examples

### Runnable
```java
Runnable task = () -> System.out.println("Running...");
new Thread(task).start();
```

### Comparator
```java
import java.util.Arrays;
import java.util.Comparator;

String[] names = {"Charlie", "Alice", "Bob"};

// Sort by length
Arrays.sort(names, (a, b) -> a.length() - b.length());
System.out.println(Arrays.toString(names));   // [Bob, Alice, Charlie]
```

### Predicate (returns boolean)
```java
import java.util.function.Predicate;

Predicate<Integer> isEven = n -> n % 2 == 0;

System.out.println(isEven.test(4));   // true
System.out.println(isEven.test(7));   // false
```

### Function (transforms one type to another)
```java
import java.util.function.Function;

Function<String, Integer> toLength = s -> s.length();

System.out.println(toLength.apply("Hello"));   // 5
System.out.println(toLength.apply("Java"));    // 4
```

### Consumer (takes, no return)
```java
import java.util.function.Consumer;

Consumer<String> print = s -> System.out.println(s.toUpperCase());

print.accept("hello");   // HELLO
print.accept("world");   // WORLD
```

### Supplier (no params, returns value)
```java
import java.util.function.Supplier;

Supplier<Double> randomValue = () -> Math.random();

System.out.println(randomValue.get());
System.out.println(randomValue.get());
```

---

## Lambda with Collections

### forEach
```java
import java.util.List;

List<String> names = List.of("Alice", "Bob", "Charlie");

names.forEach(name -> System.out.println(name));
```

### Output:
```
Alice
Bob
Charlie
```

### sort
```java
import java.util.ArrayList;
import java.util.List;

List<String> names = new ArrayList<>(List.of("Charlie", "Alice", "Bob"));

names.sort((a, b) -> a.compareTo(b));
System.out.println(names);   // [Alice, Bob, Charlie]
```

### removeIf
```java
import java.util.ArrayList;
import java.util.List;

List<Integer> numbers = new ArrayList<>(List.of(1, 2, 3, 4, 5, 6));

numbers.removeIf(n -> n % 2 == 0);   // remove even numbers
System.out.println(numbers);          // [1, 3, 5]
```

---

## Method References (Shorthand for Lambda)

When a lambda simply calls an existing method, you can use a method reference instead.

| Type | Lambda | Method Reference |
|---|---|---|
| Static method | `x -> Math.sqrt(x)` | `Math::sqrt` |
| Instance method | `s -> System.out.println(s)` | `System.out::println` |
| Specific object | `n -> obj.compare(n)` | `obj::compare` |
| Constructor | `() -> new ArrayList()` | `ArrayList::new` |

### Examples:

```java
import java.util.Arrays;
import java.util.List;

List<String> names = List.of("Charlie", "Alice", "Bob");

// Lambda
names.forEach(name -> System.out.println(name));

// Method reference (shorthand)
names.forEach(System.out::println);
```

```java
import java.util.Arrays;
import java.util.List;

List<String> names = List.of("Charlie", "Alice", "Bob");

// Sort using method reference
List<String> sorted = names.stream()
    .sorted(String::compareToIgnoreCase)
    .toList();

System.out.println(sorted);   // [Alice, Bob, Charlie]
```

---

## Variable Capture (Final Effectively)

Lambdas can access **local variables** from the enclosing scope, but they must be **final or effectively final** (not reassigned).

```java
int x = 10;   // effectively final

Runnable r = () -> System.out.println(x);
r.run();   // 10

// x = 20;   // ERROR — cannot reassign if used in lambda
```

---

## Complete Example — Real World

```java
import java.util.*;
import java.util.function.*;
import java.util.stream.*;

public class LambdaDemo {
    public static void main(String[] args) {
        List<String> students = List.of(
            "Alice", "Bob", "Charlie", "David", "Eve"
        );

        // forEach
        System.out.println("--- All Students ---");
        students.forEach(s -> System.out.println(s));

        // filter + collect
        System.out.println("\n--- Names with length > 3 ---");
        List<String> longNames = students.stream()
            .filter(s -> s.length() > 3)
            .collect(Collectors.toList());
        longNames.forEach(s -> System.out.println(s));

        // map + collect
        System.out.println("\n--- Uppercase names ---");
        List<String> upper = students.stream()
            .map(String::toUpperCase)
            .collect(Collectors.toList());
        upper.forEach(s -> System.out.println(s));

        // sort
        System.out.println("\n--- Sorted ---");
        List<String> sorted = students.stream()
            .sorted()
            .toList();
        sorted.forEach(s -> System.out.println(s));
    }
}
```

### Output:
```
--- All Students ---
Alice
Bob
Charlie
David
Eve

--- Names with length > 3 ---
Alice
Charlie
David

--- Uppercase names ---
ALICE
BOB
CHARLIE
DAVID
EVE

--- Sorted ---
Alice
Bob
Charlie
David
Eve
```

---

## Key Points to Remember

- Lambda is a short anonymous function: `(params) -> expression`
- Lambdas work only with **functional interfaces** (one abstract method)
- Use `@FunctionalInterface` annotation to mark functional interfaces
- Built-in interfaces: `Predicate`, `Function`, `Consumer`, `Supplier`, `Comparator`
- Lambdas can access final or effectively final variables
- Use **method references** (`::`) as shorthand for simple lambdas
- Lambdas make code shorter and enable functional-style programming
