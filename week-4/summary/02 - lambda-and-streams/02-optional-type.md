# Optional Type

Optional is basically a container object that may or may not contain a value. The whole point of it is to deal with null in a cleaner way so you do not get those annoying NullPointerExceptions all over your code. Instead of returning null from a method you return an Optional and then the caller decides what to do if the value is not there. It is like a box that either has something in it or is empty, and you can check before reaching in. You create an Optional using Optional.of() when you know the value is not null, Optional.ofNullable() when it might be null, and Optional.empty() for an empty one. Then you can use methods like isPresent() to check if a value exists, or get() to grab the value. But the better way to use Optional is with orElse() which gives you a default value if nothing is there, orElseGet() which uses a supplier to generate a fallback, or orElseThrow() which throws an exception. You can also chain operations with map() and filter() which lets you transform or check the value without dealing with null checks everywhere. Basically Optional forces you to handle the case where data is missing and it makes your code way more defensive and readable than scattering null checks everywhere.

```java
import java.util.Optional;

public class OptionalDemo {
    public static void main(String[] args) {
        // Creating Optionals
        Optional<String> present = Optional.of("Hello");
        Optional<String> nullable = Optional.ofNullable(null);
        Optional<String> empty = Optional.empty();

        // isPresent and get
        if (present.isPresent()) {
            System.out.println(present.get());   // Hello
        }

        // orElse — gives default if empty
        String value1 = nullable.orElse("Default");
        System.out.println(value1);   // Default

        // orElseGet — uses supplier for default
        String value2 = nullable.orElseGet(() -> "Computed Default");
        System.out.println(value2);   // Computed Default

        // orElseThrow — throws exception if empty
        // String value3 = empty.orElseThrow(() -> new RuntimeException("No value!"));

        // map — transforms the value inside
        Optional<Integer> length = present.map(String::length);
        System.out.println(length.orElse(0));   // 5

        // filter — checks condition
        Optional<String> filtered = present.filter(s -> s.startsWith("H"));
        System.out.println(filtered.isPresent());   // true

        // Real world usage — safe method calls
        String result = getUserName(null);
        System.out.println(result);   // Guest
    }

    static String getUserName(String name) {
        return Optional.ofNullable(name)
            .orElse("Guest");
    }
}
```
