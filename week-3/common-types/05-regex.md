# Regular Expressions (RegEx) in Java

---

## What is RegEx?

A Regular Expression (RegEx) is a **pattern** used to match character combinations in strings. It is used for search, validation, and text manipulation.

---

## Basic Syntax

```java
import java.util.regex.*;

public class RegexDemo {
    public static void main(String[] args) {
        String text = "Hello World 123";
        String pattern = "\\d+";   // matches one or more digits

        boolean matches = text.matches(".*\\d+.*");
        System.out.println("Contains digits: " + matches);
    }
}
```

### Output:
```
Contains digits: true
```

---

## Common Regex Patterns

| Pattern | Description | Example Match |
|---|---|---|
| `.` | Any single character | `a`, `b`, `1` |
| `\\d` | Digit (0-9) | `5`, `9` |
| `\\D` | Non-digit | `a`, `@` |
| `\\w` | Word character (a-z, A-Z, 0-9, _) | `a`, `Z`, `3` |
| `\\W` | Non-word character | `@`, `#`, ` ` |
| `\\s` | Whitespace | ` ` (space), `\t` |
| `\\S` | Non-whitespace | `a`, `1`, `@` |
| `\\b` | Word boundary | start/end of word |
| `^` | Start of string | `^Hello` |
| `$` | End of string | `world$` |
| `*` | Zero or more | `ab*c` → `ac`, `abc`, `abbc` |
| `+` | One or more | `ab+c` → `abc`, `abbc` (not `ac`) |
| `?` | Zero or one | `ab?c` → `ac`, `abc` |
| `{n}` | Exactly n times | `\\d{3}` → `123` |
| `{n,m}` | Between n and m times | `\\d{2,4}` → `12`, `1234` |
| `[abc]` | Character class (a or b or c) | `a`, `b`, `c` |
| `[^abc]` | Not a, b, or c | `d`, `1`, `@` |
| `[a-z]` | Range (a to z) | `m`, `z` |
| `(abc)` | Group | `abc` |
| `a\|b` | OR (a or b) | `a`, `b` |

---

## String Methods with RegEx

### matches() — Full String Match
```java
public class MatchesDemo {
    public static void main(String[] args) {
        String email = "user@gmail.com";

        // Simple email pattern
        boolean valid = email.matches("[a-zA-Z0-9._]+@[a-zA-Z0-9.]+\\.[a-zA-Z]+");
        System.out.println("Valid email: " + valid);
    }
}
```

### split() — Split by Pattern
```java
public class SplitDemo {
    public static void main(String[] args) {
        String data = "apple,banana;cherry orange";
        String[] fruits = data.split("[,;\\s]+");

        for (String f : fruits) {
            System.out.println(f);
        }
    }
}
```

### Output:
```
apple
banana
cherry
orange
```

### replaceAll() — Replace Using Pattern
```java
public class ReplaceDemo {
    public static void main(String[] args) {
        String text = "Hello 123 World 456";

        // Remove all digits
        String noDigits = text.replaceAll("\\d+", "");
        System.out.println("No digits: " + noDigits);

        // Replace digits with #
        String replaced = text.replaceAll("\\d+", "#");
        System.out.println("Replaced: " + replaced);
    }
}
```

### Output:
```
No digits: Hello  World 
Replaced: Hello # World #
```

---

## Pattern and Matcher Classes

```java
import java.util.regex.*;

public class MatcherDemo {
    public static void main(String[] args) {
        String text = "Call me at 9876543210 or 1234567890";
        Pattern pattern = Pattern.compile("\\d{10}");
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            System.out.println("Found: " + matcher.group());
        }
    }
}
```

### Output:
```
Found: 9876543210
Found: 1234567890
```

---

## Practical Examples

### Validate Phone Number (10 digits)
```java
public class PhoneValidator {
    public static boolean isValidPhone(String phone) {
        return phone.matches("\\d{10}");
    }

    public static void main(String[] args) {
        System.out.println(isValidPhone("9876543210"));   // true
        System.out.println(isValidPhone("12345"));         // false
        System.out.println(isValidPhone("98765abcde"));    // false
    }
}
```

### Validate Password (min 8 chars, 1 uppercase, 1 digit)
```java
public class PasswordValidator {
    public static boolean isValid(String password) {
        return password.matches("^(?=.*[A-Z])(?=.*\\d).{8,}$");
    }

    public static void main(String[] args) {
        System.out.println(isValid("Passw0rd"));   // true
        System.out.println(isValid("password"));    // false
        System.out.println(isValid("PASS1234"));    // true
    }
}
```

### Extract All Words
```java
import java.util.regex.*;

public class WordExtractor {
    public static void main(String[] args) {
        String text = "Hello World! Java is great.";
        Pattern p = Pattern.compile("\\b[a-zA-Z]+\\b");
        Matcher m = p.matcher(text);

        while (m.find()) {
            System.out.println(m.group());
        }
    }
}
```

### Output:
```
Hello
World
Java
is
great
```

---

## Quick Reference

| Pattern | Meaning |
|---|---|
| `\\d` | Digit |
| `\\D` | Non-digit |
| `\\w` | Word character |
| `\\W` | Non-word character |
| `\\s` | Whitespace |
| `\\S` | Non-whitespace |
| `.` | Any character |
| `*` | Zero or more |
| `+` | One or more |
| `?` | Zero or one |
| `{n}` | Exactly n |
| `[abc]` | Character class |
| `[^abc]` | Not in class |
| `(abc)` | Group |
| `a\|b` | Alternation |

---

## Key Points to Remember

- Use `Pattern` and `Matcher` for complex matching.
- Use `String.matches()` for simple full-string matching.
- Use `String.split()` to split strings by pattern.
- Use `String.replaceAll()` to replace using regex.
- Escape special characters with `\\` in Java strings.
- Use online regex testers to test patterns before coding.
