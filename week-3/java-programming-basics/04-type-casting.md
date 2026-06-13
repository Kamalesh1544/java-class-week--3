# Type Casting in Java

---

## What is Type Casting?

Type casting is the process of **converting one data type to another**. In Java, there are two types of casting:

1. **Widening Casting** (Implicit) — smaller to larger type (automatic)
2. **Narrowing Casting** (Explicit) — larger to smaller type (manual)

---

## Widening Casting (Implicit)

Smaller data type is automatically converted to a larger data type. No data loss occurs.

### Order of Widening:
```
byte → short → int → long → float → double
char → int → long → float → double
```

### Syntax
```java
smallType smallVar = value;
largeType largeVar = smallVar;   // automatic
```

### Example
```java
public class WideningDemo {
    public static void main(String[] args) {
        byte b = 100;
        int i = b;          // byte → int (automatic)
        long l = i;         // int → long (automatic)
        float f = l;        // long → float (automatic)
        double d = f;       // float → double (automatic)

        System.out.println("byte: " + b);
        System.out.println("int: " + i);
        System.out.println("long: " + l);
        System.out.println("float: " + f);
        System.out.println("double: " + d);
    }
}
```

### Output:
```
byte: 100
int: 100
long: 100
float: 100.0
double: 100.0
```

---

## Narrowing Casting (Explicit)

Larger data type is manually converted to a smaller data type. **Data loss may occur**. Must use parentheses.

### Order of Narrowing:
```
double → float → long → int → short → byte
```

### Syntax
```java
largeType largeVar = value;
smallType smallVar = (smallType) largeVar;   // manual cast
```

### Example
```java
public class NarrowingDemo {
    public static void main(String[] args) {
        double d = 99.99;
        int i = (int) d;          // double → int (decimal part lost)
        byte b = (byte) i;        // int → byte (may overflow)

        System.out.println("double: " + d);
        System.out.println("int: " + i);
        System.out.println("byte: " + b);
    }
}
```

### Output:
```
double: 99.99
int: 99
byte: 99
```

---

## What Happens When Overflow Occurs?

When a larger value is cast to a smaller type, it **wraps around**.

```java
public class OverflowDemo {
    public static void main(String[] args) {
        int big = 200;
        byte small = (byte) big;   // 200 exceeds byte range (-128 to 127)

        System.out.println("int: " + big);
        System.out.println("byte: " + small);   // Output: -56 (overflow)
    }
}
```

### Output:
```
int: 200
byte: -56
```

---

## Type Casting with Objects (Downcasting and Upcasting)

In inheritance, casting works between parent and child classes.

### Upcasting (Child → Parent) — Automatic
```java
class Animal {}
class Dog extends Animal {}

Animal a = new Dog();   // upcasting — always safe
```

### Downcasting (Parent → Child) — Explicit
```java
Animal a = new Dog();
Dog d = (Dog) a;        // downcasting — may throw ClassCastException
```

### Safe Downcasting with instanceof
```java
Animal a = new Dog();
if (a instanceof Dog) {
    Dog d = (Dog) a;
    System.out.println("Safe downcasting successful");
}
```

---

## Example Program — All Together

```java
public class TypeCastingDemo {
    public static void main(String[] args) {
        // Widening
        int x = 100;
        double y = x;       // int → double
        System.out.println("Widening: int " + x + " → double " + y);

        // Narrowing
        double a = 75.8;
        int b = (int) a;   // double → int
        System.out.println("Narrowing: double " + a + " → int " + b);

        // char to int
        char ch = 'A';
        int ascii = ch;     // char → int (widening)
        System.out.println("char '" + ch + "' → int " + ascii);

        // int to char
        char fromInt = (char) 65;  // int → char (narrowing)
        System.out.println("int 65 → char '" + fromInt + "'");
    }
}
```

### Output:
```
Widening: int 100 → double 100.0
Narrowing: double 75.8 → int 75
char 'A' → int 65
int 65 → char 'A'
```

---

## Key Points to Remember

- Widening is automatic and safe (no data loss).
- Narrowing requires explicit casting and may cause data loss.
- Always check for overflow when narrowing.
- Use `instanceof` before downcasting objects to avoid `ClassCastException`.
- `char` can be widened to `int` (gives Unicode value).
