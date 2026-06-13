# Collections in Java

---

## What is a Collection?

A Collection is a **framework** that provides a unified architecture for storing and manipulating **groups of objects**. Instead of creating your own data structures from scratch, Java gives you ready-made classes to store, access, and manage data efficiently.

Collections handle common tasks like:
- Adding and removing elements
- Searching for elements
- Sorting elements
- Iterating through elements

---

## Why Use Collections?

- **Reusable** — no need to write your own data structures
- **Optimized** — tested and performance-tuned
- **Type safe** — works with generics
- **Interoperable** —统一 API across different data structures

---

## Collection Hierarchy

```
Iterable
└── Collection
    ├── List (ordered, duplicates allowed)
    ├── Set (no duplicates)
    └── Queue (FIFO processing)

Map (key-value pairs — separate hierarchy)
```

---

## Types of Collections

### 1. List — Ordered, Duplicates Allowed

Maintains insertion order. Elements can be accessed by index.

| Class | Description |
|---|---|
| `ArrayList` | Backed by array. Fast random access. Best for read-heavy operations. |
| `LinkedList` | Doubly linked list. Fast insert/delete. Best for frequent additions/removals. |
| `Vector` | Thread-safe ArrayList (legacy). Slower due to synchronization. |
| `Stack` | LIFO (Last In First Out). Extends Vector (legacy). Use ArrayDeque instead. |

---

### 2. Set — No Duplicates

Does not allow duplicate elements.

| Class | Description |
|---|---|
| `HashSet` | No ordering guaranteed. Fastest. Allows one null. |
| `LinkedHashSet` | Maintains insertion order. Slightly slower than HashSet. |
| `TreeSet` | Sorted in natural order (ascending). No null allowed. |

---

### 3. Queue — FIFO Processing

Elements are processed in First-In-First-Out order.

| Class | Description |
|---|---|
| `PriorityQueue` | Processes elements in priority order (smallest first by default). |
| `ArrayDeque` | Double-ended queue. Faster than LinkedList for queue/stack operations. |
| `LinkedList` | Also implements Queue interface. |

---

### 4. Map — Key-Value Pairs

Stores data as key-value pairs. Keys are unique.

| Class | Description |
|---|---|
| `HashMap` | No ordering. Allows one null key. Fastest. |
| `LinkedHashMap` | Maintains insertion order. |
| `TreeMap` | Sorted by key. No null keys. |
| `Hashtable` | Thread-safe (legacy). No null keys/values. |
| `ConcurrentHashMap` | Thread-safe. Better performance than Hashtable. |

---

## Quick Comparison

| Collection | Ordered | Duplicates | Null Allowed | Thread Safe |
|---|---|---|---|---|
| ArrayList | Yes | Yes | Yes | No |
| LinkedList | Yes | Yes | Yes | No |
| HashSet | No | No | 1 null | No |
| LinkedHashSet | Insertion order | No | 1 null | No |
| TreeSet | Sorted | No | No | No |
| HashMap | No | Keys: No | 1 null key | No |
| LinkedHashMap | Insertion order | Keys: No | 1 null key | No |
| TreeMap | Sorted by key | Keys: No | No | No |

---

## Key Points to Remember

- **List** → ordered, duplicates allowed, access by index
- **Set** → no duplicates
- **Queue** → FIFO processing
- **Map** → key-value pairs
- Use **ArrayList** for fast reads, **LinkedList** for fast inserts/deletes
- Use **HashSet** for unique elements, **TreeSet** for sorted unique elements
- Use **HashMap** for fast key-value lookups
- Detailed explanations are in the `advanced-collections/` folder
