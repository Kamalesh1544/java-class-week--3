# Data Types in Java

---

## What is a Data Type?

A data type defines **what kind of value** a variable can hold. Every variable in Java must have a declared data type.

Java has **two categories** of data types:

1. **Primitive Data Types** — Built-in, stores actual values
2. **Non-Primitive (Reference) Data Types** — Objects, stores reference/memory address

---

## Primitive Data Types

Java has **8 primitive data types**:

| Data Type | Size | Default | Range |
|---|---|---|---|
| `byte` | 1 byte | 0 | -128 to 127 |
| `short` | 2 bytes | 0 | -32,768 to 32,767 |
| `int` | 4 bytes | 0 | -2^31 to 2^31 - 1 |
| `long` | 8 bytes | 0L | -2^63 to 2^63 - 1 |
| `float` | 4 bytes | 0.0f | ±3.4e+38 (6-7 decimal digits) |
| `double` | 8 bytes | 0.0d | ±1.7e+308 (15-16 decimal digits) |
| `char` | 2 bytes | '\u0000' | 0 to 65,535 (Unicode) |
| `boolean` | 1 bit | false | true or false |

---

## Syntax and Examples

### byte
```java
byte b = 100;
```

### short
```java
short s = 30000;
```

### int (most commonly used for whole numbers)
```java
int age = 25;
int salary = 50000;
```

### long (for large numbers, use 'L' suffix)
```java
long population = 8000000000L;
```

### float (use 'f' suffix)
```java
float weight = 65.5f;
```

### double (default for decimal numbers)
```java
double price = 99.99;
double pi = 3.14159265359;
```

### char (single character, use single quotes)
```java
char grade = 'A';
char symbol = '@';
```

### boolean (true or false)
```java
boolean isActive = true;
boolean hasPermission = false;
```

---

## Non-Primitive (Reference) Data Types

| Data Type | Description |
|---|---|
| `String` | Sequence of characters |
| `Array` | Collection of similar elements |
| `Class` | Blueprint for creating objects |
| `Interface` | Contract for class behavior |

```java
String name = "Java Programming";
int[] numbers = {1, 2, 3, 4, 5};
```

---

## Type Conversion (Default Values)

| Data Type | Default Value |
|---|---|
| byte, short, int, long | 0 |
| float, double | 0.0 |
| char | '\u0000' (null character) |
| boolean | false |
| String / Objects | null |

---

## Example Program

```java
public class DataTypesDemo {
    public static void main(String[] args) {
        byte temperature = 40;
        short year = 2026;
        int population = 1400000000;
        long distance = 15000000000L;
        float height = 5.9f;
        double gpa = 3.85;
        char grade = 'A';
        boolean passed = true;
        String college = "IIT Madras";

        System.out.println("Temperature: " + temperature);
        System.out.println("Year: " + year);
        System.out.println("Population: " + population);
        System.out.println("Distance: " + distance);
        System.out.println("Height: " + height);
        System.out.println("GPA: " + gpa);
        System.out.println("Grade: " + grade);
        System.out.println("Passed: " + passed);
        System.out.println("College: " + college);
    }
}
```

### Output:
```
Temperature: 40
Year: 2026
Population: 1400000000
Distance: 15000000000
Height: 5.9
GPA: 3.85
Grade: A
Passed: true
College: IIT Madras
```

---

## Key Points to Remember

- Use `int` for whole numbers and `double` for decimals by default.
- Use `long` when `int` is too small (add `L` suffix).
- Use `float` when memory is a concern (add `f` suffix).
- `char` uses single quotes `'A'`, `String` uses double quotes `"Hello"`.
- `boolean` can only be `true` or `false`.
