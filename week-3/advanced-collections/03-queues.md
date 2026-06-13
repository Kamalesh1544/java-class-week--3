# Queues in Java — Detailed Guide

---

## What is a Queue?

A Queue is a collection designed for **FIFO (First In, First Out)** processing. The element added first is removed first — like a line at a ticket counter.

---

## Queue Interface Methods

| Method | Throws Exception | Returns Special Value | Description |
|---|---|---|---|
| `add(e)` | Yes | — | Adds element, throws if full |
| `offer(e)` | No | true/false | Adds element, returns false if full |
| `remove()` | Yes | — | Removes head, throws if empty |
| `poll()` | No | null | Removes head, returns null if empty |
| `element()` | Yes | — | Returns head, throws if empty |
| `peek()` | No | null | Returns head, returns null if empty |

> **Tip:** Use `offer`, `poll`, and `peek` for safe operations without exceptions.

---

## 1. LinkedList as Queue

```java
import java.util.LinkedList;
import java.util.Queue;

public class LinkedListQueue {
    public static void main(String[] args) {
        Queue<String> queue = new LinkedList<>();

        // offer — add to end
        queue.offer("First");
        queue.offer("Second");
        queue.offer("Third");
        queue.offer("Fourth");

        System.out.println("Queue: " + queue);

        // peek — view head without removing
        System.out.println("Head: " + queue.peek());

        // poll — remove from front
        System.out.println("Removed: " + queue.poll());
        System.out.println("Removed: " + queue.poll());

        System.out.println("After removal: " + queue);

        // size
        System.out.println("Size: " + queue.size());

        // isEmpty
        System.out.println("Empty? " + queue.isEmpty());
    }
}
```

### Output:
```
Queue: [First, Second, Third, Fourth]
Head: First
Removed: First
Removed: Second
After removal: [Third, Fourth]
Size: 2
Empty? false
```

---

## 2. PriorityQueue — Heap-based

Elements are processed in **priority order** (natural ordering or custom comparator). The head is the **smallest** element (min-heap by default).

```java
import java.util.PriorityQueue;
import java.util.Queue;

public class PriorityQueueDemo {
    public static void main(String[] args) {
        Queue<Integer> pq = new PriorityQueue<>();

        pq.offer(50);
        pq.offer(10);
        pq.offer(30);
        pq.offer(20);
        pq.offer(40);

        System.out.println("PriorityQueue: " + pq);
        System.out.println("Head (smallest): " + pq.peek());

        // Elements come out in sorted order
        System.out.println("\nRemoving in priority order:");
        while (!pq.isEmpty()) {
            System.out.println(pq.poll());
        }
    }
}
```

### Output:
```
PriorityQueue: [10, 20, 30, 50, 40]
Head (smallest): 10

Removing in priority order:
10
20
30
40
50
```

---

### PriorityQueue with Custom Comparator (Max-Heap)

```java
import java.util.Comparator;
import java.util.PriorityQueue;

public class MaxHeapDemo {
    public static void main(String[] args) {
        // Max-heap — largest element first
        PriorityQueue<Integer> maxPQ = new PriorityQueue<>(Comparator.reverseOrder());

        maxPQ.offer(50);
        maxPQ.offer(10);
        maxPQ.offer(30);
        maxPQ.offer(20);

        System.out.println("Max-Heap order:");
        while (!maxPQ.isEmpty()) {
            System.out.println(maxPQ.poll());
        }
    }
}
```

### Output:
```
Max-Heap order:
50
30
20
10
```

---

### PriorityQueue with Objects

```java
import java.util.PriorityQueue;

public class PatientQueue {
    public static void main(String[] args) {
        PriorityQueue<String> patients = new PriorityQueue<>();

        patients.offer("Low priority");
        patients.offer("Critical");
        patients.offer("Medium priority");
        patients.offer("High priority");

        System.out.println("Patients in priority order:");
        while (!patients.isEmpty()) {
            System.out.println(patients.poll());
        }
    }
}
```

### Output:
```
Patients in priority order:
Critical
High priority
Low priority
Medium priority
```

> Note: Priority is based on natural alphabetical order here. For custom priority, use a Comparator.

---

## 3. ArrayDeque — Double-Ended Queue

Faster than LinkedList for queue and stack operations. Can add/remove from **both ends**.

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class ArrayDequeDemo {
    public static void main(String[] args) {
        Deque<String> deque = new ArrayDeque<>();

        // As Queue — add to end, remove from front
        deque.offer("A");
        deque.offer("B");
        deque.offer("C");

        System.out.println("Queue: " + deque);
        System.out.println("Poll: " + deque.poll());

        // As Stack — add to front, remove from front
        deque.push("X");
        deque.push("Y");

        System.out.println("After push: " + deque);
        System.out.println("Pop: " + deque.pop());

        // Add/Remove from both ends
        deque.offerFirst("Z");
        deque.offerLast("W");

        System.out.println("After both ends: " + deque);
        System.out.println("PollFirst: " + deque.pollFirst());
        System.out.println("PollLast: " + deque.pollLast());

        System.out.println("Final: " + deque);
    }
}
```

### Output:
```
Queue: [A, B, C]
Poll: A
After push: [Y, X, B, C]
Pop: Y
After both ends: [Z, X, B, C, W]
PollFirst: Z
PollLast: W
Final: [X, B, C]
```

---

## 4. Stack (Legacy — Use Deque Instead)

Last-In-First-Out (LIFO). Push to top, pop from top.

```java
import java.util.Stack;

public class StackDemo {
    public static void main(String[] args) {
        Stack<String> stack = new Stack<>();

        stack.push("Bottom");
        stack.push("Middle");
        stack.push("Top");

        System.out.println("Stack: " + stack);
        System.out.println("Peek: " + stack.peek());
        System.out.println("Pop: " + stack.pop());
        System.out.println("Pop: " + stack.pop());
        System.out.println("Size: " + stack.size());
    }
}
```

### Output:
```
Stack: [Bottom, Middle, Top]
Peek: Top
Pop: Top
Pop: Middle
Size: 1
```

> **Note:** Prefer `ArrayDeque` over `Stack` for better performance.

---

## 5. BlockingQueue — Thread-Safe

Used in producer-consumer scenarios. Blocks when queue is full (on add) or empty (on remove).

```java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;

public class BlockingQueueDemo {
    public static void main(String[] args) throws InterruptedException {
        BlockingQueue<String> queue = new ArrayBlockingQueue<>(3);

        queue.offer("First");
        queue.offer("Second");
        queue.offer("Third");

        // queue.offer("Fourth");   // would return false (queue full)

        // Blocks until element is available
        System.out.println("Taken: " + queue.take());
        System.out.println("Taken: " + queue.take());

        System.out.println("Queue: " + queue);
    }
}
```

### Output:
```
Taken: First
Taken: Second
Queue: [Third]
```

---

## Queue Comparison Table

| Queue | Ordered | Bounded | Thread Safe | Use Case |
|---|---|---|---|---|
| LinkedList | FIFO | No | No | General purpose queue |
| PriorityQueue | Priority | No | No | Priority processing |
| ArrayDeque | FIFO / LIFO | Yes (fixed) | No | Fast queue/stack |
| ArrayBlockingQueue | FIFO | Yes | Yes | Producer-consumer |
| LinkedBlockingQueue | FIFO | Optional | Yes | Producer-consumer |
| PriorityQueue | Priority | No | No | Task scheduling |

---

## Real-World Examples

### BFS (Breadth-First Search) with Queue

```java
import java.util.*;

public class BFSDemo {
    public static void bfs(Map<String, List<String>> graph, String start) {
        Queue<String> queue = new LinkedList<>();
        Set<String> visited = new HashSet<>();

        queue.offer(start);
        visited.add(start);

        while (!queue.isEmpty()) {
            String node = queue.poll();
            System.out.print(node + " ");

            for (String neighbor : graph.getOrDefault(node, List.of())) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }
    }

    public static void main(String[] args) {
        Map<String, List<String>> graph = new HashMap<>();
        graph.put("A", List.of("B", "C"));
        graph.put("B", List.of("D", "E"));
        graph.put("C", List.of("F"));
        graph.put("D", List.of());
        graph.put("E", List.of());
        graph.put("F", List.of());

        bfs(graph, "A");
    }
}
```

### Output:
```
A B C D E F
```

---

### Task Scheduler with PriorityQueue

```java
import java.util.PriorityQueue;

public class TaskScheduler {
    public static void main(String[] args) {
        PriorityQueue<String> tasks = new PriorityQueue<>();

        tasks.offer("Low: Check emails");
        tasks.offer("HIGH: Fix production bug");
        tasks.offer("Medium: Write tests");
        tasks.offer("HIGH: Deploy hotfix");

        System.out.println("Processing tasks in priority order:");
        while (!tasks.isEmpty()) {
            System.out.println("→ " + tasks.poll());
        }
    }
}
```

### Output:
```
Processing tasks in priority order:
→ HIGH: Deploy hotfix
→ HIGH: Fix production bug
→ Low: Check emails
→ Medium: Write tests
```

---

## When to Use What?

| Scenario | Use |
|---|---|
| Simple FIFO processing | `LinkedList` or `ArrayDeque` |
| Priority-based processing | `PriorityQueue` |
| Stack (LIFO) | `ArrayDeque` |
| Producer-consumer (multi-thread) | `ArrayBlockingQueue` |
| BFS traversal | `LinkedList` or `ArrayDeque` |
| Task scheduling | `PriorityQueue` |

---

## Key Points to Remember

- Queue follows **FIFO** (First In, First Out).
- Use `offer`, `poll`, `peek` for safe operations.
- **PriorityQueue** processes elements in priority order (smallest first by default).
- **ArrayDeque** is faster than LinkedList for queue and stack operations.
- **BlockingQueue** is thread-safe — use for concurrent programming.
- Prefer `ArrayDeque` over `Stack` for stack operations.
- Use `Comparator.reverseOrder()` for max-heap in PriorityQueue.
