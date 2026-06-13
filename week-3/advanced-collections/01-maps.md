# Maps in Java — Detailed Guide

---

## What is a Map?

A Map is a collection of **key-value pairs**. Each key maps to exactly one value. You use the key to quickly find the value — like a dictionary where a word (key) maps to its definition (value).

---

## Map Interface Methods

| Method | Description |
|---|---|
| `put(key, value)` | Adds or updates a key-value pair |
| `get(key)` | Returns the value for a key |a
| `getOrDefault(key, default)` | Returns value or default if not found |
| `remove(key)` | Removes the key-value pair |
| `containsKey(key)` | Checks if key exists |
| `containsValue(value)` | Checks if value exists |
| `size()` | Returns number of pairs |
| `isEmpty()` | Checks if map is empty |
| `clear()` | Removes all pairs |
| `keySet()` | Returns all keys as a Set |
| `values()` | Returns all values as a Collection |
| `entrySet()` | Returns all key-value pairs as a Set |
| `putIfAbsent(key, value)` | Adds only if key doesn't exist |
| `replace(key, value)` | Replaces value for existing key |
| `merge(key, value, func)` | Merges value with existing using function |
| `forEach(action)` | Iterates over all entries (Java 8+) |

---

## 1. HashMap — Fast, No Order

The most commonly used Map. No guaranteed order. Allows one null key and multiple null values.

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> ages = new HashMap<>();

        // put — add entries
        ages.put("Alice", 25);
        ages.put("Bob", 30);
        ages.put("Charlie", 22);
        ages.put("Diana", 28);

        // get — retrieve value
        System.out.println("Alice's age: " + ages.get("Alice"));

        // containsKey
        System.out.println("Has Bob? " + ages.containsKey("Bob"));

        // containsValue
        System.out.println("Has age 22? " + ages.containsValue(22));

        // size
        System.out.println("Total entries: " + ages.size());

        // putIfAbsent — only adds if key doesn't exist
        ages.putIfAbsent("Alice", 99);    // Alice already exists, ignored
        ages.putIfAbsent("Eve", 35);      // Eve doesn't exist, added

        System.out.println(ages);
    }
}
```

### Output:
```
Alice's age: 25
Has Bob? true
Has age 22? true
Total entries: 4
{Diana=28, Alice=25, Charlie=22, Bob=30, Eve=35}
```

---

### Iterating a HashMap

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapIterate {
    public static void main(String[] args) {
        Map<String, Integer> scores = new HashMap<>();
        scores.put("Math", 95);
        scores.put("Science", 88);
        scores.put("English", 92);

        // Method 1: entrySet — get both key and value
        System.out.println("--- entrySet ---");
        for (Map.Entry<String, Integer> entry : scores.entrySet()) {
            System.out.println(entry.getKey() + " : " + entry.getValue());
        }

        // Method 2: keySet — get keys, then values
        System.out.println("\n--- keySet ---");
        for (String key : scores.keySet()) {
            System.out.println(key + " → " + scores.get(key));
        }

        // Method 3: forEach (Java 8+)
        System.out.println("\n--- forEach ---");
        scores.forEach((subject, mark) ->
            System.out.println(subject + " → " + mark)
        );
    }
}
```

### Output:
```
--- entrySet ---
Science : 88
English : 92
Math : 95

--- keySet ---
Science → 88
English → 92
Math → 95

--- forEach ---
Science → 88
English → 92
Math → 95
```

---

### Replace and Merge

```java
import java.util.HashMap;
import java.util.Map;

public class HashMapReplaceDemo {
    public static void main(String[] args) {
        Map<String, Integer> inventory = new HashMap<>();
        inventory.put("apple", 10);
        inventory.put("banana", 5);
        inventory.put("cherry", 8);

        // replace — updates value
        inventory.replace("banana", 20);
        System.out.println("After replace: " + inventory);

        // replace only if key exists
        inventory.replace("mango", 15);   // mango doesn't exist, ignored
        System.out.println("No mango: " + inventory);

        // merge — combine values
        inventory.merge("apple", 5, Integer::sum);   // 10 + 5 = 15
        System.out.println("After merge: " + inventory);

        // remove
        inventory.remove("cherry");
        System.out.println("After remove: " + inventory);
    }
}
```

### Output:
```
After replace: {cherry=8, banana=20, apple=10}
No mango: {cherry=8, banana=20, apple=10}
After merge: {cherry=8, banana=20, apple=15}
After remove: {banana=20, apple=15}
```

---

## 2. LinkedHashMap — Insertion Order

Maintains the order in which entries were inserted. Slightly slower than HashMap.

```java
import java.util.LinkedHashMap;
import java.util.Map;

public class LinkedHashMapDemo {
    public static void main(String[] args) {
        Map<String, String> capitals = new LinkedHashMap<>();

        capitals.put("India", "New Delhi");
        capitals.put("USA", "Washington DC");
        capitals.put("Japan", "Tokyo");
        capitals.put("France", "Paris");

        System.out.println(capitals);
        // {India=New Delhi, USA=Washington DC, Japan=Tokyo, France=Paris}
    }
}
```

---

## 3. TreeMap — Sorted by Key

Entries are sorted by key in natural order (ascending). Uses Red-Black tree internally.

```java
import java.util.TreeMap;
import java.util.Map;

public class TreeMapDemo {
    public static void main(String[] args) {
        Map<String, Integer> students = new TreeMap<>();

        students.put("Charlie", 85);
        students.put("Alice", 92);
        students.put("Eve", 88);
        students.put("Bob", 90);

        System.out.println(students);
        // {Alice=92, Bob=90, Charlie=85, Eve=88} — sorted by name

        // First and last key
        System.out.println("First key: " + students.firstKey());
        System.out.println("Last key: " + students.lastKey());

        // Range views
        System.out.println("From Bob to Eve: " + students.subMap("Bob", "Eve"));
        System.out.println("HeadMap (before Charlie): " + students.headMap("Charlie"));
        System.out.println("TailMap (from Charlie): " + students.tailMap("Charlie"));
    }
}
```

### Output:
```
{Alice=92, Bob=90, Charlie=85, Eve=88}
First key: Alice
Last key: Eve
From Bob to Eve: {Bob=90, Charlie=85}
HeadMap (before Charlie): {Alice=92, Bob=90}
TailMap (from Charlie): {Charlie=85, Eve=88}
```

---

## 4. Hashtable — Thread Safe (Legacy)

Thread-safe but synchronized — slower. Does not allow null keys or values.

```java
import java.util.Hashtable;
import java.util.Map;

public class HashtableDemo {
    public static void main(String[] args) {
        Hashtable<String, Integer> table = new Hashtable<>();

        table.put("One", 1);
        table.put("Two", 2);
        table.put("Three", 3);

        System.out.println(table);
        System.out.println("Two: " + table.get("Two"));

        // table.put(null, 5);   // NullPointerException — null not allowed
    }
}
```

---

## 5. EnumMap — Key is an Enum

Optimized for enum keys. Very fast.

```java
import java.util.EnumMap;
import java.util.Map;

public class EnumMapDemo {
    enum Day {
        MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY
    }

    public static void main(String[] args) {
        Map<Day, String> activities = new EnumMap<>(Day.class);

        activities.put(Day.MONDAY, "Gym");
        activities.put(Day.TUESDAY, "Yoga");
        activities.put(Day.WEDNESDAY, "Swimming");
        activities.put(Day.THURSDAY, "Running");
        activities.put(Day.FRIDAY, "Rest");

        activities.forEach((day, activity) ->
            System.out.println(day + " → " + activity)
        );
    }
}
```

### Output:
```
MONDAY → Gym
TUESDAY → Yoga
WEDNESDAY → Swimming
THURSDAY → Running
FRIDAY → Rest
```

---

## Map Comparison Table

| Map | Order | Null Keys | Null Values | Thread Safe | Performance |
|---|---|---|---|---|---|
| HashMap | No | 1 key | Multiple | No | Fastest |
| LinkedHashMap | Insertion | 1 key | Multiple | No | Fast |
| TreeMap | Sorted | No | Multiple | No | Moderate |
| Hashtable | No | No | No | Yes (synchronized) | Slow |
| EnumMap | Enum order | No | Multiple | No | Fastest for enums |

---

## Real-World Example — Word Frequency Counter

```java
import java.util.HashMap;
import java.util.Map;

public class WordFrequency {
    public static void main(String[] args) {
        String text = "the cat sat on the mat the cat";

        Map<String, Integer> freq = new HashMap<>();
        String[] words = text.split(" ");

        for (String word : words) {
            freq.merge(word, 1, Integer::sum);   // add 1 to existing count
        }

        freq.forEach((word, count) ->
            System.out.println(word + " → " + count)
        );
    }
}
```

### Output:
```
cat → 2
the → 3
sat → 1
mat → 1
on → 1
```

---

## Key Points to Remember

- Use **HashMap** for fast lookups (no order needed).
- Use **LinkedHashMap** when insertion order matters.
- Use **TreeMap** when sorted order is needed.
- Use **Hashtable** for legacy thread-safe code (prefer ConcurrentHashMap).
- Use **EnumMap** when keys are enums.
- `merge()` is powerful for counting and aggregation.
- Never use mutable objects as keys (or override hashCode/equals properly).
