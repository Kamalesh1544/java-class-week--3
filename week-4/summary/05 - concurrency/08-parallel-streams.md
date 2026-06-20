# Parallel Streams

Parallel streams are the easiest way to add concurrency to your code without dealing with threads directly. When you call parallelStream() on a collection instead of stream(), Java splits the data into chunks, processes each chunk on a different thread using the ForkJoinPool, and then combines the results. It is literally one word difference from a regular stream but can give you massive speedups on large datasets. The default parallel pool uses the common ForkJoinPool which has as many threads as CPU cores. For simple operations like filtering, mapping, and reducing large collections parallel streams can be much faster than sequential ones. But they are not always better — for small datasets the overhead of splitting and merging outweighs the parallelism benefit. Also parallel streams are dangerous when the operation has side effects like modifying an external variable or writing to a shared collection because that introduces race conditions. The operations should be stateless and independent for parallel streams to work correctly. You can also use the parallel() method on any stream to make it parallel or use stream().sequential() to switch back. The key thing is to treat parallel streams as a performance optimization tool for CPU-intensive operations on large data, not as a default replacement for regular streams.

```java
import java.util.*;
import java.util.stream.*;

public class ParallelStreamDemo {
    public static void main(String[] args) {
        List<Integer> numbers = IntStream.rangeClosed(1, 10000000).boxed().toList();

        // Sequential
        long start = System.currentTimeMillis();
        long sum1 = numbers.stream().mapToLong(Integer::longValue).sum();
        System.out.println("Sequential: " + (System.currentTimeMillis() - start) + "ms");
        System.out.println("Sum: " + sum1);

        // Parallel — much faster for large data
        start = System.currentTimeMillis();
        long sum2 = numbers.parallelStream().mapToLong(Integer::longValue).sum();
        System.out.println("Parallel: " + (System.currentTimeMillis() - start) + "ms");
        System.out.println("Sum: " + sum2);

        // Parallel filter
        long evenCount = numbers.parallelStream()
            .filter(n -> n % 2 == 0)
            .count();
        System.out.println("Even count: " + evenCount);

        // Beware — side effects in parallel streams are dangerous
        List<Integer> shared = new ArrayList<>();
        // shared.add(1);   // NOT safe in parallel stream without synchronization
    }
}
```
