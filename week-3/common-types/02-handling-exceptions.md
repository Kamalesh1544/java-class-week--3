# Handling Exceptions in Java

---

## The try-catch-finally Block

```java
try {
    // code that may throw exception
} catch (ExceptionType e) {
    // handle exception
} finally {
    // always executes (whether exception occurs or not)
}
```

---

## try-catch

Handles the exception and continues execution.

```java
public class TryCatchDemo {
    public static void main(String[] args) {
        try {
            int[] arr = {1, 2, 3};
            System.out.println(arr[10]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Index out of bounds: " + e.getMessage());
        }

        System.out.println("Program continues...");
    }
}
```

### Output:
```
Index out of bounds: Index 10 out of bounds for length 3
Program continues...
```

---

## try-catch-finally

`finally` runs **always** — whether exception occurs or not.

```java
public class FinallyDemo {
    public static void main(String[] args) {
        try {
            int result = 10 / 2;
            System.out.println("Result: " + result);
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        } finally {
            System.out.println("Finally block executed");
        }
    }
}
```

### Output:
```
Result: 5
Finally block executed
```

---

## Multiple Catch Blocks

Handle different exception types separately.

```java
public class MultipleCatchDemo {
    public static void main(String[] args) {
        try {
            int[] arr = {1, 2, 3};
            int num = Integer.parseInt("abc");

            System.out.println(arr[5]);
        } catch (NumberFormatException e) {
            System.out.println("Invalid number: " + e.getMessage());
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Index error: " + e.getMessage());
        } catch (Exception e) {
            System.out.println("General error: " + e.getMessage());
        }
    }
}
```

### Output:
```
Invalid number: For input string: "abc"
```

> Catch blocks must be ordered from **most specific to most general**.

---

## Multi-Catch (Java 7+)

Handle multiple exception types in one catch block.

```java
try {
    // code
} catch (IOException | SQLException | ClassNotFoundException e) {
    System.out.println("Error: " + e.getMessage());
}
```

---

## The throws Keyword

Declares that a method **may throw** an exception. The **caller** must handle it.

```java
import java.io.*;

public class ThrowsDemo {

    public static void readFile(String path) throws FileNotFoundException {
        FileInputStream fis = new FileInputStream(path);   // may throw exception
    }

    public static void main(String[] args) {
        try {
            readFile("test.txt");
        } catch (FileNotFoundException e) {
            System.out.println("File not found: " + e.getMessage());
        }
    }
}
```

---

## The throw Keyword

Used to **manually throw** an exception.

```java
public class ThrowDemo {
    public static void validateAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative");
        }
        System.out.println("Valid age: " + age);
    }

    public static void main(String[] args) {
        try {
            validateAge(-5);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }

        validateAge(25);
    }
}
```

### Output:
```
Error: Age cannot be negative
Valid age: 25
```

---

## Custom Exceptions

Create your own exception class by extending `Exception` or `RuntimeException`.

```java
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

class BankAccount {
    double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    public void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("Balance Rs." + balance + " is less than Rs." + amount);
        }
        balance -= amount;
        System.out.println("Withdrawn Rs." + amount + ". Remaining: Rs." + balance);
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount(5000);

        try {
            acc.withdraw(3000);
            acc.withdraw(3000);   // will throw exception
        } catch (InsufficientBalanceException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Output:
```
Withdrawn Rs.3000.0. Remaining: Rs.2000.0
Error: Balance Rs.2000.0 is less than Rs.3000.0
```

---

## Finally Without Catch

You can use `finally` without `catch` (using `try-finally`).

```java
public class TryFinallyDemo {
    public static void main(String[] args) {
        try {
            System.out.println("Inside try");
            return;   // even with return, finally runs
        } finally {
            System.out.println("Inside finally");
        }
    }
}
```

### Output:
```
Inside try
Inside finally
```

---

## Exception Methods

| Method | Description |
|---|---|
| `getMessage()` | Returns the error message |
| `toString()` | Returns exception class + message |
| `printStackTrace()` | Prints full stack trace |
| `getCause()` | Returns the cause of the exception |

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Message: " + e.getMessage());
    System.out.println("String: " + e.toString());
    e.printStackTrace();
}
```

---

## Key Points to Remember

- Use `try-catch` to handle exceptions.
- `finally` always runs — use it for cleanup.
- Order catch blocks from specific to general.
- Use `throws` in method signature to declare exceptions.
- Use `throw` to manually throw an exception.
- Create custom exceptions by extending `Exception`.
- Use try-with-resources for automatic resource management.
- Never leave catch blocks empty — always log or handle the error.
