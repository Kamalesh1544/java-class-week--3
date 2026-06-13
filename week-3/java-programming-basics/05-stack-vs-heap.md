# Stack vs Heap Memory in Java

---

## What is Stack Memory?

Stack memory is used for **static memory allocation**. It stores:
- Local variables
- Method calls
- Method parameters
- Intermediate results

### Characteristics:
- **Fixed size** (determined at compile time)
- **Fast access** (LIFO — Last In, First Out)
- **Thread safe** (each thread has its own stack)
- **Automatic cleanup** (when method ends, its stack frame is removed)

### Where variables live on Stack:
```java
public void calculate() {
    int a = 10;       // stored on Stack
    int b = 20;       // stored on Stack
    int sum = a + b;  // stored on Stack
}
// a, b, sum are destroyed when method ends
```

---

## What is Heap Memory?

Heap memory is used for **dynamic memory allocation**. It stores:
- Objects
- Instance variables
- Arrays
- Strings (the actual content)

### Characteristics:
- **Dynamic size** (grows and shrinks at runtime)
- **Slower access** (compared to Stack)
- **Shared across threads** (not thread safe by default)
- **Garbage collected** (JVM automatically cleans up unused objects)

### Where objects live on Heap:
```java
public void createObject() {
    String name = new String("Java");  // object on Heap, reference on Stack
    int[] arr = new int[5];             // array on Heap, reference on Stack
}
```

---

## How Stack and Heap Work Together

```java
public class MemoryDemo {
    public static void main(String[] args) {
        int x = 10;                     // x on Stack
        String s = new String("Hello"); // s (reference) on Stack, "Hello" on Heap
        int[] arr = {1, 2, 3};          // arr (reference) on Stack, {1,2,3} on Heap
    }
}
```

### Visual Diagram:
```
Stack                          Heap
┌──────────┐                ┌──────────────┐
│ x = 10   │                │              │
│ s ───────│───────────────→│ "Hello"      │
│ arr ─────│───────────────→│ {1, 2, 3}    │
└──────────┘                └──────────────┘
```

---

## Method Call Stack (Stack Frames)

Each method call creates a **stack frame** on the Stack.

```java
public class StackFrameDemo {
    public static void main(String[] args) {
        int a = 5;
        add(a);
    }

    public static void add(int num) {
        int result = num + 10;
        System.out.println(result);
    }
}
```

### Stack Growth:
```
┌─────────────────┐
│ add()           │ ← Top (current)
│   num = 5       │
│   result = 15   │
├─────────────────┤
│ main()          │
│   a = 5         │
└─────────────────┘
```

### Stack Shrinks:
- When `add()` finishes, its frame is removed
- Control returns to `main()`
- When `main()` finishes, program ends


---

## Example Program

```java
public class MemoryDemo {
    int instanceVar = 100;   // Heap

    public void display() {
        int localVar = 50;   // Stack
        System.out.println("Instance variable: " + instanceVar);
        System.out.println("Local variable: " + localVar);
    }

    public static void main(String[] args) {
        MemoryDemo obj = new MemoryDemo();  // obj on Stack, object on Heap
        obj.display();
    }
}
```

### Output:
```
Instance variable: 100
Local variable: 50
```

---

## Key Points to Remember

- Stack stores local variables and method calls (fast, fixed size).
- Heap stores objects and instance variables (slower, dynamic size).
- Each thread has its own Stack; Heap is shared.
- Objects are garbage collected when no references exist.
- Watch out for `StackOverflowError` (infinite recursion) and `OutOfMemoryError` (too many objects).
