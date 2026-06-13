# String Methods in Java

---

## What is a String?

A `String` in Java is a **sequence of characters**. Strings are **immutable** — once created, they cannot be changed. Every modification creates a new String object.

Strings are objects of the `java.lang.String` class.

---

## Creating Strings

```java
// Method 1: String literal
String s1 = "Hello";

// Method 2: Using new keyword
String s2 = new String("Hello");

// Method 3: From character array
char[] chars = {'J', 'a', 'v', 'a'};
String s3 = new String(chars);
```

---

## Common String Methods

### Length and Character Access

```java
String s = "Hello World";

System.out.println(s.length());            // 11
System.out.println(s.charAt(0));           // H
System.out.println(s.charAt(4));           // o
```

### Case Conversion

```java
String s = "Hello World";

System.out.println(s.toUpperCase());       // HELLO WORLD
System.out.println(s.toLowerCase());       // hello world
```

### Searching

```java
String s = "Hello World Java";

System.out.println(s.indexOf('o'));            // 4 (first occurrence)
System.out.println(s.indexOf("World"));        // 6
System.out.println(s.lastIndexOf('o'));         // 13 (last occurrence)
System.out.println(s.contains("World"));       // true
System.out.println(s.startsWith("Hello"));     // true
System.out.println(s.endsWith("Java"));        // true
```

### Extracting Substrings

```java
String s = "Hello World";

System.out.println(s.substring(6));         // World (from index 6 to end)
System.out.println(s.substring(0, 5));      // Hello (from index 0 to 4)
```

### Trimming and Stripping

```java
String s = "   Hello World   ";

System.out.println(s.trim());              // "Hello World" (removes leading/trailing spaces)
System.out.println(s.strip());             // "Hello World" (Java 11+, Unicode aware)
System.out.println(s.stripLeading());      // "Hello World   "
System.out.println(s.stripTrailing());     // "   Hello World"
```

### Replacing

```java
String s = "Hello World";

System.out.println(s.replace('o', '0'));          // Hell0 W0rld
System.out.println(s.replace("World", "Java"));   // Hello Java
System.out.println(s.replaceAll("\\s", "-"));      // Hello-World
```

### Splitting

```java
String csv = "apple,banana,cherry";
String[] fruits = csv.split(",");

for (String f : fruits) {
    System.out.println(f);
}
```

### Output:
```
apple
banana
cherry
```

### Joining

```java
String[] words = {"Java", "is", "fun"};
String joined = String.join(" ", words);

System.out.println(joined);   // Java is fun
```

### Comparing

```java
String a = "Hello";
String b = "hello";
String c = "Hello";

// equals() — checks content
System.out.println(a.equals(b));          // false
System.out.println(a.equals(c));          // true

// equalsIgnoreCase() — ignores case
System.out.println(a.equalsIgnoreCase(b)); // true

// compareTo() — lexicographic comparison
System.out.println(a.compareTo(c));        // 0 (equal)
System.out.println(a.compareTo(b));        // negative (H comes before h)
```

### Type Checking

```java
String num = "12345";
String alpha = "Hello";
String empty = "";

System.out.println(num.isEmpty());        // false
System.out.println(empty.isEmpty());      // true
System.out.println(num.isBlank());        // false (Java 11+)
System.out.println("  ".isBlank());      // true (Java 11+)
```

### Converting to Other Types

```java
// String to int
int num = Integer.parseInt("123");

// String to double
double d = Double.parseDouble("3.14");

// int to String
String s1 = String.valueOf(100);
String s2 = Integer.toString(100);
String s3 = 100 + "";   // concatenation trick

// char to String
String s4 = String.valueOf(new char[]{'A', 'B', 'C'});
```

---

## String Methods Cheat Sheet

| Method | Description |
|---|---|
| `length()` | Returns the length |
| `charAt(i)` | Returns character at index i |
| `substring(start)` | Returns substring from start to end |
| `substring(start, end)` | Returns substring from start to end-1 |
| `indexOf(str)` | Returns index of first occurrence |
| `lastIndexOf(str)` | Returns index of last occurrence |
| `contains(str)` | Checks if string contains substring |
| `startsWith(str)` | Checks if string starts with substring |
| `endsWith(str)` | Checks if string ends with substring |
| `toUpperCase()` | Converts to uppercase |
| `toLowerCase()` | Converts to lowercase |
| `trim()` | Removes leading/trailing spaces |
| `strip()` | Removes leading/trailing spaces (Unicode) |
| `replace(old, new)` | Replaces occurrences |
| `replaceAll(regex, new)` | Replaces using regex |
| `split(regex)` | Splits into array |
| `join(delimiter, arr)` | Joins array into string |
| `equals(str)` | Compares content |
| `equalsIgnoreCase(str)` | Compares content ignoring case |
| `compareTo(str)` | Lexicographic comparison |
| `isEmpty()` | Checks if empty |
| `isBlank()` | Checks if empty or whitespace (Java 11+) |
| `toCharArray()` | Converts to char array |
| `valueOf(obj)` | Converts to String |
| `format(fmt, args)` | Returns formatted string |

---

## StringBuilder — Mutable Strings

Since `String` is immutable, use `StringBuilder` for frequent modifications.

```java
public class BuilderDemo {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder("Hello");

        sb.append(" World");      // append
        sb.insert(5, ",");        // insert
        sb.replace(0, 5, "Hi");   // replace
        sb.delete(2, 3);          // delete

        System.out.println(sb.toString());   // Hi World

        // Reverse
        sb.reverse();
        System.out.println(sb.toString());   // dlroW iH
    }
}
```

### StringBuilder Methods

| Method | Description |
|---|---|
| `append(str)` | Adds text to end |
| `insert(index, str)` | Inserts text at index |
| `delete(start, end)` | Deletes characters |
| `replace(start, end, str)` | Replaces characters |
| `reverse()` | Reverses the string |
| `toString()` | Converts to String |
| `length()` | Returns length |
| `charAt(i)` | Returns character at index |

---

## String vs StringBuilder

| Feature | String | StringBuilder |
|---|---|---|
| Mutability | Immutable | Mutable |
| Performance | Slow (creates new objects) | Fast (modifies same object) |
| Thread safety | Yes | No |
| Use case | Few modifications | Frequent modifications |

---

## Key Points to Remember

- Strings are **immutable** — every modification creates a new object.
- Use `equals()` to compare String content, not `==`.
- Use `StringBuilder` for loops or frequent string modifications.
- `substring()`, `indexOf()`, `contains()` are the most commonly used.
- Use `Integer.parseInt()` and `String.valueOf()` for type conversion.
- Use `split()` and `join()` for working with delimited strings.
