# Stream API — Collectors

Collectors are the tool you use at the end of a stream pipeline to gather all the processed results into something useful — like a List, a Set, a Map, or even a single formatted string. Without collectors you would have to manually loop and collect results yourself which defeats the whole purpose of using streams. The most common collector is Collectors.toList() which collects stream elements into a List. But there is way more you can do. Collectors.toSet() gives you a Set which automatically removes duplicates. Collectors.toMap() lets you build a Map from stream elements by specifying what the key and value should be. Collectors.joining() concatenates all strings in the stream with a delimiter — super handy for building CSV lines or comma-separated values. Collectors.groupingBy() groups elements by some criteria like grouping students by their grade. Collectors.partitioningBy() splits elements into two groups based on a boolean condition. Collectors.counting(), Collectors.summingInt(), Collectors.minBy(), Collectors.maxBy() give you quick statistics. Basically if you need to collect stream results into any kind of structure, there is a collector for it and you do not have to write any manual collection logic.

```java
import java.util.*;
import java.util.stream.*;

public class CollectorsDemo {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie", "David", "Eve");

        // toList
        List<String> list = names.stream()
            .filter(n -> n.length() > 3)
            .collect(Collectors.toList());
        System.out.println(list);   // [Alice, Charlie, David]

        // toSet (removes duplicates)
        List<Integer> nums = List.of(1, 2, 2, 3, 3, 3);
        Set<Integer> set = nums.stream()
            .collect(Collectors.toSet());
        System.out.println(set);   // [1, 2, 3]

        // joining
        String joined = names.stream()
            .collect(Collectors.joining(", "));
        System.out.println(joined);   // Alice, Bob, Charlie, David, Eve

        // groupingBy
        List<String> words = List.of("apple", "banana", "avocado", "blueberry", "cherry");
        Map<Character, List<String>> grouped = words.stream()
            .collect(Collectors.groupingBy(w -> w.charAt(0)));
        System.out.println(grouped);
        // {a=[apple, avocado], b=[banana, blueberry], c=[cherry]}

        // counting
        long count = names.stream()
            .collect(Collectors.counting());
        System.out.println("Count: " + count);   // 5

        // summingInt
        List<Integer> values = List.of(10, 20, 30, 40);
        int sum = values.stream()
            .collect(Collectors.summingInt(Integer::intValue));
        System.out.println("Sum: " + sum);   // 100

        // toMap
        Map<String, Integer> nameLength = names.stream()
            .collect(Collectors.toMap(n -> n, String::length));
        System.out.println(nameLength);
        // {Alice=5, Bob=3, Charlie=7, David=5, Eve=3}
    }
}
```
