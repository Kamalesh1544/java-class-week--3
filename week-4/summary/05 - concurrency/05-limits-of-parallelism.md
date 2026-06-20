# Limits of Parallelism

More threads does not always mean faster execution. There are real limits to how much parallelism can help and understanding them saves you from writing code that is slower with more threads than without. The first limit is the number of CPU cores — if you have 4 cores and create 100 threads, only 4 can actually run at the same time, the rest are just waiting. The context switching that happens when the OS switches between threads takes time and resources so having too many threads actually slows things down. The second limit is the nature of the task itself. If a task is CPU-bound meaning it is doing heavy computation like sorting a huge array or calculating complex math, adding more threads beyond the number of cores does not help because the CPU is already fully utilized. Parallelism helps most with I/O-bound tasks where threads spend time waiting for disk reads, network responses, or database queries — while one thread waits, another can use the CPU. The third limit is shared resources — if all threads are trying to write to the same file or access the same database table, they end up waiting for locks and the benefits of parallelism are lost. This is called contention and it is a major bottleneck. The fourth limit is memory — each thread consumes stack memory and having thousands of threads can cause OutOfMemoryError. The practical takeaway is that parallelism is a tool not a magic solution. Use thread pools with a reasonable number of threads, understand whether your workload is CPU-bound or I/O-bound, and profile your application to see if adding more threads actually improves performance.

```java
// Too many threads — overhead exceeds benefit
public class ParallelismLimits {
    public static void main(String[] args) {
        int[] array = new int[1000000];
        for (int i = 0; i < array.length; i++) array[i] = i;

        // One thread — 100ms
        long start = System.currentTimeMillis();
        int sum = 0;
        for (int n : array) sum += n;
        System.out.println("Single thread: " + (System.currentTimeMillis() - start) + "ms");

        // 4 threads — faster on 4-core CPU
        start = System.currentTimeMillis();
        // ... parallel sum logic
        System.out.println("4 threads: faster");

        // 1000 threads — slower due to context switching
        // ... creating 1000 threads for a simple sum
        System.out.println("1000 threads: overhead kills performance");
    }
}

// Better approach — use thread pool with right size
import java.util.concurrent.*;

public class RightApproach {
    public static void main(String[] args) {
        // Use number of available cores
        int cores = Runtime.getRuntime().availableProcessors();
        ExecutorService executor = Executors.newFixedThreadPool(cores);

        System.out.println("Optimal threads: " + cores);
    }
}
```
