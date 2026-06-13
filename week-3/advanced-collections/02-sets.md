# Sets in Java — Detailed Guide

---

## What is a Set?

A Set is a collection that contains **no duplicate elements**. It models the mathematical set abstraction. If you try to add a duplicate, it simply ignores it.

---

## Set Interface Methods

| Method | Description |
|---|---|
| `add(element)` | Adds element (returns false if duplicate) |
| `remove(element)` | Removes element |
| `contains(element)` | Checks if element exists |
| `size()` | Returns number of elements |
| `isEmpty()` | Checks if set is empty |
| `clear()` | Removes all elements |
| `addAll(collection)` | Adds all elements from another collection |
| `retainAll(collection)` | Keeps only elements in both sets |
| `removeAll(collection)` | Removes elements in another collection |
| `toArray()` | Converts to array |
| `forEach(action)` | Iterates over elements (Java 8+) |

---

## 1. HashSet — Fast, No Order

Most commonly used Set. Uses hashing internally. Fastest for add, remove, and contains operations. Allows one null element.

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetDemo {
    public static void main(String[] args) {
        Set<String> fruits = new HashSet<>();

        // add — returns true if added, false if duplicate
        System.out.println(fruits.add("Apple"));      // true
        System.out.println(fruits.add("Banana"));     // true
        System.out.println(fruits.add("Cherry"));     // true
        System.out.println(fruits.add("Apple"));      // false (duplicate)

        System.out.println("Fruits: " + fruits);
        System.out.println("Size: " + fruits.size());

        // contains
        System.out.println("Has Banana? " + fruits.contains("Banana"));

        // remove
        fruits.remove("Cherry");
        System.out.println("After remove: " + fruits);
    }
}
```

### Output:
```
true
true
true
false
Fruits: [Banana, Apple, Cherry]
Size: 3
Has Banana? true
After remove: [Banana, Apple]
```

---

### Iterating a HashSet

```java
import java.util.HashSet;
import java.util.Set;

public class HashSetIterate {
    public static void main(String[] args) {
        Set<Integer> numbers = new HashSet<>();
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);
        numbers.add(40);

        // For-each loop
        System.out.println("--- For-each ---");
        for (int num : numbers) {
            System.out.println(num);
        }

        // forEach (Java 8+)
        System.out.println("\n--- forEach ---");
        numbers.forEach(n -> System.out.println(n));
    }
}
```

---

## 2. LinkedHashSet — Insertion Order

Maintains the order in which elements were inserted. Slightly slower than HashSet.

```java
import java.util.LinkedHashSet;
import java.util.Set;

public class LinkedHashSetDemo {
    public static void main(String[] args) {
        Set<String> colors = new LinkedHashSet<>();

        colors.add("Red");
        colors.add("Green");
        colors.add("Blue");
        colors.add("Yellow");

        System.out.println(colors);
        // [Red, Green, Blue, Yellow] — insertion order maintained
    }
}
```

---

## 3. TreeSet — Sorted Order

Elements are sorted in natural order (ascending). Uses Red-Black tree. Does not allow null elements.

```java
import java.util.TreeSet;
import java.util.Set;

public class TreeSetDemo {
    public static void main(String[] args) {
        Set<Integer> numbers = new TreeSet<>();

        numbers.add(50);
        numbers.add(10);
        numbers.add(30);
        numbers.add(20);
        numbers.add(40);

        System.out.println("Sorted: " + numbers);
        // [10, 20, 30, 40, 50]

        // Navigation methods
        TreeSet<Integer> ts = (TreeSet<Integer>) numbers;
        System.out.println("First: " + ts.first());      // 10
        System.out.println("Last: " + ts.last());        // 50
        System.out.println("Lower(30): " + ts.lower(30));  // 20 (just below)
        System.out.println("Higher(30): " + ts.higher(30)); // 40 (just above)
        System.out.println("HeadSet(30): " + ts.headSet(30)); // [10, 20]
        System.out.println("TailSet(30): " + ts.tailSet(30)); // [30, 40, 50]
    }
}
```

### Output:
```
Sorted: [10, 20, 30, 40, 50]
First: 10
Last: 50
Lower(30): 20
Higher(30): 40
HeadSet(30): [10, 20]
TailSet(30): [30, 40, 50]
```

---

### TreeSet with Custom Sorting

```java
import java.util.TreeSet;
import java.util.Comparator;

public class TreeSetCustomSort {
    public static void main(String[] args) {
        // Sort strings by length
        TreeSet<String> byLength = new TreeSet<>(
            Comparator.comparingInt(String::length)
        );

        byLength.add("Banana");
        byLength.add("Apple");
        byLength.add("Fig");
        byLength.add("Kiwi");
        byLength.add("Cherry");

        System.out.println(byLength);
        // [Fig, Kiwi, Apple, Banana, Cherry]
    }
}
```

---

## 4. EnumSet — Optimized for Enums

Very fast and memory-efficient. Elements must be from the same enum.

```java
import java.util.EnumSet;

public class EnumSetDemo {
    enum Role {
        ADMIN, EDITOR, VIEWER, MODERATOR
    }

    public static void main(String> args) {
        EnumSet<Role> adminPerms = EnumSet.of(Role.ADMIN, Role.EDITOR);

        EnumSet<Role> allRoles = EnumSet.allOf(Role.class);

        EnumSet<Role> noAdmin = EnumSet.complementOf(adminPerms);

        System.out.println("Admin perms: " + adminPerms);
        System.out.println("All roles: " + allRoles);
        System.out.println("Not admin: " + noAdmin);
    }
}
```

### Output:
```
Admin perms: [ADMIN, EDITOR]
All roles: [ADMIN, EDITOR, VIEWER, MODERATOR]
Not admin: [VIEWER, MODERATOR]
```

---

## Set Operations

### Union (addAll)
```java
import java.util.HashSet;
import java.util.Set;

public class SetUnion {
    public static void main(String[] args) {
        Set<Integer> a = new HashSet<>(Set.of(1, 2, 3));
        Set<Integer> b = new HashSet<>(Set.of(3, 4, 5));

        Set<Integer> union = new HashSet<>(a);
        union.addAll(b);

        System.out.println("Union: " + union);
        // [1, 2, 3, 4, 5]
    }
}
```

### Intersection (retainAll)
```java
import java.util.HashSet;
import java.util.Set;

public class SetIntersection {
    public static void main(String[] args) {
        Set<Integer> a = new HashSet<>(Set.of(1, 2, 3, 4));
        Set<Integer> b = new HashSet<>(Set.of(3, 4, 5, 6));

        Set<Integer> intersection = new HashSet<>(a);
        intersection.retainAll(b);

        System.out.println("Intersection: " + intersection);
        // [3, 4]
    }
}
```

### Difference (removeAll)
```java
import java.util.HashSet;
import java.util.Set;

public class SetDifference {
    public static void main(String[] args) {
        Set<Integer> a = new HashSet<>(Set.of(1, 2, 3, 4));
        Set<Integer> b = new HashSet<>(Set.of(3, 4, 5, 6));

        Set<Integer> diff = new HashSet<>(a);
        diff.removeAll(b);

        System.out.println("Difference (A - B): " + diff);
        // [1, 2]
    }
}
```

---

## Checking for Duplicates

```java
import java.util.HashSet;
import java.util.List;

public class DuplicateCheck {
    public static boolean hasDuplicates(List<String> list) {
        return list.size() != new HashSet<>(list).size();
    }

    public static void main(String[] args) {
        List<String> names1 = List.of("Alice", "Bob", "Charlie");
        List<String> names2 = List.of("Alice", "Bob", "Alice");

        System.out.println(names1 + " → " + hasDuplicates(names1));   // false
        System.out.println(names2 + " → " + hasDuplicates(names2));   // true
    }
}
```

### Output:
```
[Alice, Bob, Charlie] → false
[Alice, Bob, Alice] → true
```

---

## Remove Duplicates from List

```java
import java.util.ArrayList;
import java.util.LinkedHashSet;
import java.util.List;
import java.util.Set;

public class RemoveDuplicates {
    public static void main(String[] args) {
        List<Integer> list = List.of(5, 3, 5, 1, 3, 2, 1, 4);

        // LinkedHashSet preserves order and removes duplicates
        Set<Integer> unique = new LinkedHashSet<>(list);
        List<Integer> result = new ArrayList<>(unique);

        System.out.println("Original: " + list);
        System.out.println("Without duplicates: " + result);
    }
}
```

### Output:
```
Original: [5, 3, 5, 1, 3, 2, 1, 4]
Without duplicates: [5, 3, 1, 2, 4]
```

---

## Set Comparison Table

| Set | Order | Null | Duplicates | Thread Safe | Performance |
|---|---|---|---|---|---|
| HashSet | No | 1 null | No | No | Fastest |
| LinkedHashSet | Insertion | 1 null | No | No | Fast |
| TreeSet | Sorted | No | No | No | Moderate |
| EnumSet | Enum order | No | No | No | Fastest for enums |
| CopyOnWriteArraySet | Insertion | Yes | No | Yes | Slow (writes) |

---

## Key Points to Remember

- Sets **never allow duplicates** — adding a duplicate is silently ignored.
- Use **HashSet** for fastest performance (no order needed).
- Use **LinkedHashSet** to maintain insertion order.
- Use **TreeSet** for sorted elements.
- Use **EnumSet** when elements are from an enum.
- Use `addAll()`, `retainAll()`, `removeAll()` for set operations.
- Use `LinkedHashSet` to remove duplicates from a list while preserving order.
