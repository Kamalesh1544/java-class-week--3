# Loops in Java

---

## What is a Loop?

A loop is a control structure that **repeats a block of code** multiple times until a condition is met. Loops help avoid writing the same code again and again.

Java has **4 types of loops**:

1. `for` loop
2. `while` loop
3. `do-while` loop
4. `for-each` loop (enhanced for loop)

---

## 1. for Loop

Used when the **number of iterations is known**.

### Syntax
```java
for (initialization; condition; update) {
    // code to execute
}
```

### How it Works:
1. **initialization** — runs once at the start
2. **condition** — checked before each iteration; if false, loop stops
3. **update** — runs after each iteration

### Example
```java
public class ForLoopDemo {
    public static void main(String[] args) {
        // Print numbers 1 to 5
        for (int i = 1; i <= 5; i++) {
            System.out.println("Number: " + i);
        }
    }
}
```

### Output:
```
Number: 1
Number: 2
Number: 3
Number: 4
Number: 5
```

---

## 2. while Loop

Used when the **number of iterations is not known**. The loop runs as long as the condition is true.

### Syntax
```java
while (condition) {
    // code to execute
}
```

### Example
```java
public class WhileLoopDemo {
    public static void main(String[] args) {
        int count = 1;

        while (count <= 5) {
            System.out.println("Count: " + count);
            count++;
        }
    }
}
```

### Output:
```
Count: 1
Count: 2
Count: 3
Count: 4
Count: 5
```

---

## 3. do-while Loop

Similar to `while`, but the code block executes **at least once** before checking the condition.

### Syntax
```java
do {
    // code to execute
} while (condition);
```

### Example
```java
public class DoWhileDemo {
    public static void main(String[] args) {
        int num = 10;

        do {
            System.out.println("Number: " + num);
            num++;
        } while (num <= 5);
    }
}
```

### Output:
```
Number: 10
```
Even though the condition (`10 <= 5`) is false, the code ran **once**.

---

## 4. for-each Loop (Enhanced for Loop)

Used to iterate through **arrays and collections** easily.

### Syntax
```java
for (dataType element : array) {
    // code to execute
}
```

### Example
```java
public class ForEachDemo {
    public static void main(String[] args) {
        String[] colors = {"Red", "Green", "Blue"};

        for (String color : colors) {
            System.out.println("Color: " + color);
        }
    }
}
```

### Output:
```
Color: Red
Color: Green
Color: Blue
```

---

## Loop Control Statements

### break
**Stops** the loop immediately.

```java
public class BreakDemo {
    public static void main(String[] args) {
        for (int i = 1; i <= 10; i++) {
            if (i == 5) {
                break;   // stop when i is 5
            }
            System.out.println(i);
        }
    }
}
```

### Output:
```
1
2
3
4
```

---

### continue
**Skips** the current iteration and moves to the next.

```java
public class ContinueDemo {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            if (i == 3) {
                continue;   // skip when i is 3
            }
            System.out.println(i);
        }
    }
}
```

### Output:
```
1
2
4
5
```

---

## Nested Loops

A loop inside another loop.

### Example: Print multiplication table
```java
public class NestedLoopDemo {
    public static void main(String[] args) {
        for (int i = 1; i <= 3; i++) {
            for (int j = 1; j <= 3; j++) {
                System.out.print(i * j + "\t");
            }
            System.out.println();
        }
    }
}
```

### Output:
```
1    2    3
2    4    6
3    6    9
```

---

## Infinite Loop

A loop that runs forever (use carefully).

### Using for
```java
for (;;) {
    System.out.println("Infinite");
}
```

### Using while
```java
while (true) {
    System.out.println("Infinite");
}
```

---

## Comparison Table

| Loop | When to Use | Condition Check |
|---|---|---|
| `for` | Known number of iterations | Before each iteration |
| `while` | Unknown iterations, may not run at all | Before each iteration |
| `do-while` | Must run at least once | After each iteration |
| `for-each` | Iterating arrays/collections | Automatic |

---

## Example Program — All Loops Together

```java
public class LoopsDemo {
    public static void main(String[] args) {
        int[] nums = {10, 20, 30, 40, 50};

        // for loop
        System.out.println("--- for loop ---");
        for (int i = 0; i < nums.length; i++) {
            System.out.println(nums[i]);
        }

        // while loop
        System.out.println("\n--- while loop ---");
        int i = 0;
        while (i < nums.length) {
            System.out.println(nums[i]);
            i++;
        }

        // do-while loop
        System.out.println("\n--- do-while loop ---");
        int j = 0;
        do {
            System.out.println(nums[j]);
            j++;
        } while (j < nums.length);

        // for-each loop
        System.out.println("\n--- for-each loop ---");
        for (int num : nums) {
            System.out.println(num);
        }
    }
}
```

---

## Key Points to Remember

- Use `for` when you know the exact number of iterations.
- Use `while` when the loop should run based on a condition.
- Use `do-while` when the loop must execute at least once.
- Use `for-each` for iterating arrays and collections.
- Use `break` to stop a loop early.
- Use `continue` to skip an iteration.
- Avoid infinite loops unless intentional.
