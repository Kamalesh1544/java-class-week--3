# Primitives vs Objects in Java

---

## What is the Difference?

In Java, data is stored in two ways:

| Feature | Primitives | Objects (Reference Types) |
|---|---|---|
| Stores | Actual value | Reference (memory address) |
| Memory | Stack memory | Heap memory |
| Speed | Faster | Slower |
| Default Value | 0, false, 0.0, '\u0000' | null |
| Methods | No methods | Has methods and properties |
| Examples | int, char, boolean | String, Integer, Array |

---

## Primitives

Primitives are **basic data types** that store the actual value directly.

```java
int num = 10;        // stores 10 directly
double price = 99.9; // stores 99.9 directly
char grade = 'A';    // stores 'A' directly
boolean flag = true; // stores true directly
```

- No methods available
- Stored on the **Stack**
- Faster access

---

## Objects (Reference Types)

Objects are instances of classes. They store a **reference** (address) to the actual data in the heap.

```java
String name = "Hello";      // reference to String object in heap
int[] arr = {1, 2, 3};      // reference to array object in heap
Integer num = 10;            // wrapper class object
```

- Has methods and properties
- Stored on the **Heap**
- Reference is on the Stack

---

## Wrapper Classes (Auto Boxing)

Java provides **wrapper classes** for each primitive type. This allows primitives to be used as objects.

| Primitive | Wrapper Class |
|---|---|
| byte | Byte |
| short | Short |
| int | Integer |
| long | Long |
| float | Float |
| double | Double |
| char | Character |
| boolean | Boolean |

### Auto Boxing (Primitive → Object)
```java
int a = 10;
Integer obj = a;    // automatic conversion
```

### Unboxing (Object → Primitive)
```java
Integer obj = 20;
int a = obj;        // automatic conversion
```

---

## String — Special Case

`String` in Java is a **class**, not a primitive. But it behaves like a primitive because:
- Values are immutable (cannot be changed after creation)
- Can be assigned with `=` directly

```java
String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1 == s2);  // true (same reference due to string pool)
```

---

## Example Program

```java
public class PrimitivesVsObjects {
    public static void main(String[] args) {
        // Primitives
        int primitiveInt = 42;
        double primitiveDouble = 3.14;

        // Objects (Wrapper classes)
        Integer objectInt = 42;          // Auto Boxing
        Double objectDouble = 3.14;      // Auto Boxing

        // Back to primitive
        int backToInt = objectInt;       // Unboxing

        System.out.println("Primitive int: " + primitiveInt);
        System.out.println("Object Integer: " + objectInt);
        System.out.println("Primitive double: " + primitiveDouble);
        System.out.println("Object Double: " + objectDouble);
        System.out.println("Unboxed int: " + backToInt);

        // String as object
        String message = "Java is fun";
        System.out.println("Length: " + message.length());
        System.out.println("Uppercase: " + message.toUpperCase());
    }
}
```

### Output:
```
Primitive int: 42
Object Integer: 42
Primitive double: 3.14
Object Double: 3.14
Unboxed int: 42
Length: 12
Uppercase: JAVA IS FUN
```

---

## When to Use What?

| Use Primitives When | Use Objects When |
|---|---|
| You need simple values | You need methods (like String.length()) |
| Performance matters | Null values are needed |
| Working with arrays of numbers | Working with collections (ArrayList, HashMap) |
| No special behavior needed | You need object-oriented features |

---

## Key Points to Remember

- Primitives store values directly; objects store references.
- Primitives are faster and use less memory.
- Wrapper classes allow primitives to be used as objects.
- Auto boxing and unboxing convert between primitives and objects automatically.
- `String` is a special object type that is immutable.
