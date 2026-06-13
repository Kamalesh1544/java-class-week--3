# Enums in Java

---

## What is an Enum?

An enum (enumeration) is a **special data type** that represents a **fixed set of constants**. Once defined, the values cannot change.

Think of it like a list of named values — `RED`, `GREEN`, `BLUE` for colors, or `MONDAY`, `TUESDAY` for days.

---

## Basic Syntax

```java
enum EnumName {
    VALUE1, VALUE2, VALUE3
}
```

---

## Simple Example

```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public class Main {
    public static void main(String[] args) {
        Day today = Day.MONDAY;
        System.out.println("Today is: " + today);
    }
}
```

### Output:
```
Today is: MONDAY
```

---

## Enums in switch Statements

```java
enum Season {
    SPRING, SUMMER, MONSOON, WINTER
}

public class Main {
    public static void main(String[] args) {
        Season s = Season.MONSOON;

        switch (s) {
            case SPRING:
                System.out.println("Flowers bloom");
                break;
            case SUMMER:
                System.out.println("It's hot");
                break;
            case MONSOON:
                System.out.println("It's raining");
                break;
            case WINTER:
                System.out.println("It's cold");
                break;
        }
    }
}
```

### Output:
```
It's raining
```

---


---

## Enum Methods

| Method | Description |
|---|---|
| `name()` | Returns the name as a String |
| `ordinal()` | Returns the position (starting from 0) |
| `values()` | Returns an array of all enum values |
| `valueOf(String)` | Returns the enum constant with the specified name |

```java
enum Fruit {
    APPLE, BANANA, CHERRY
}

public class Main {
    public static void main(String[] args) {
        // name() and ordinal()
        Fruit f = Fruit.BANANA;
        System.out.println("Name: " + f.name());       // BANANA
        System.out.println("Ordinal: " + f.ordinal());  // 1

        // values()
        System.out.println("\nAll fruits:");
        for (Fruit fruit : Fruit.values()) {
            System.out.println(fruit.ordinal() + ": " + fruit.name());
        }

        // valueOf()
        Fruit f2 = Fruit.valueOf("CHERRY");
        System.out.println("\nvalueOf: " + f2);         // CHERRY
    }
}
```

### Output:
```
Name: BANANA
Ordinal: 1

All fruits:
0: APPLE
1: BANANA
2: CHERRY

valueOf: CHERRY
```
---

## Enum as Type Safety

Enums prevent invalid values — unlike plain strings or integers.

```java
// Without enums — error prone
String day = "MONDAY";    // could be "monday", "MONDAY", "invalid"
if (day == "MONDAY") {}   // string comparison issues

// With enums — type safe
enum Day { MONDAY, TUESDAY }
Day d = Day.MONDAY;       // only valid values allowed
```

