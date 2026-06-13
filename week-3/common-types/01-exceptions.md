# Exceptions in Java

---

## What is an Exception?

An exception is an **unexpected event** that disrupts the normal flow of a program during execution. It is an object that contains information about the error — type, message, and where it occurred.

---

## Exception Hierarchy

```
Throwable
├── Error (serious, cannot recover)
│   ├── OutOfMemoryError
│   ├── StackOverflowError
│   └── VirtualMachineError
│
└── Exception (can be handled)
    ├── IOException
    ├── SQLException
    ├── ClassNotFoundException
    │
    └── RuntimeException (Unchecked)
        ├── NullPointerException
        ├── ArrayIndexOutOfBoundsException
        ├── ArithmeticException
        ├── NumberFormatException
        └── ClassCastException
```

---

## Types of Exceptions

### 1. Checked Exceptions (Compile-time)
- Detected by the compiler at compile time
- Must be handled with `try-catch` or declared with `throws`
- Examples: `IOException`, `SQLException`, `FileNotFoundException`

```java
import java.io.*;

public class CheckedDemo {
    public static void main(String[] args) {
        FileInputStream file = new FileInputStream("test.txt");   // compile error if not handled
    }
}
```

### 2. Unchecked Exceptions (Runtime)
- Not checked at compile time
- Occur during program execution
- Examples: `NullPointerException`, `ArithmeticException`, `ArrayIndexOutOfBoundsException`

```java
public class UncheckedDemo {
    public static void main(String[] args) {
        int a = 10 / 0;    // ArithmeticException — not caught at compile time
    }
}
```


---

## Common Exceptions Table

| Exception | Cause |
|---|---|
| `NullPointerException` | Calling method on null reference |
| `ArrayIndexOutOfBoundsException` | Accessing invalid array index |
| `ArithmeticException` | Divide by zero |
| `NumberFormatException` | Invalid string to number conversion |
| `ClassCastException` | Invalid type casting |
| `FileNotFoundException` | File does not exist |
| `IOException` | I/O operation failed |
| `StringIndexOutOfBoundsException` | Invalid string index |

---