# The Stream API

The Stream API is just a way to process collections of data without writing loops. Instead of manually iterating through a list and doing stuff with each element you describe a pipeline of operations — like filter this, transform that, collect the results — and the stream handles all the looping internally. It is a more functional style of programming and once you get used to it you will find yourself writing less code that is easier to read. A stream is not a data structure itself, it does not store anything, it just takes data from a source like a list or array and passes it through a pipeline of operations. You start by calling .stream() on a collection, then you chain intermediate operations like filter, map, sorted which are lazy meaning they only run when you trigger a terminal operation. Terminal operations like collect, forEach, count, reduce actually execute the pipeline and give you a result. The beauty of streams is you can chain everything into one clean line instead of writing nested loops with temporary variables. And if you have a huge dataset you can even call parallelStream() to process things in parallel across multiple threads automatically.

```java
import java.util.*;
import java.util.stream.*;

public class StreamDemo {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie", "David", "Eve");

        // Filter, transform, collect — all in one chain
        String result = names.stream()
            .filter(name -> name.length() > 3)      // keep names longer than 3
            .map(String::toUpperCase)                // convert to uppercase
            .sorted()                                // sort alphabetically
            .collect(Collectors.joining(", "));      // join with comma

        System.out.println(result);   // ALICE, CHARLIE, DAVID

        // Count even numbers
        List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8);
        long evens = numbers.stream()
            .filter(n -> n % 2 == 0)
            .count();
        System.out.println("Even count: " + evens);   // 4

        // Sum
        int sum = numbers.stream()
            .mapToInt(Integer::intValue)
            .sum();
        System.out.println("Sum: " + sum);   // 36
    }
}
```
