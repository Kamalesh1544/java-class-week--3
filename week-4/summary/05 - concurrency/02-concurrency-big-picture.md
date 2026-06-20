# Big Picture and Intuition — Concurrency

The big picture with concurrency is that you are managing multiple tasks that share the same resources. Think of a restaurant kitchen — you have multiple cooks working at the same time, sharing the same stove, the same knives, the same counter space. Without coordination it would be chaos — two cooks grabbing the same pan, someone chopping where someone else is cooking, orders getting mixed up. Concurrency is about creating order in that chaos. Java gives you several tools to work with. Threads are the basic building blocks — they let you run code in parallel. Executors and Thread Pools manage a group of threads so you do not have to create and destroy them manually for every task. Synchronization tools like locks, semaphores, and synchronized blocks make sure only one thread accesses a shared resource at a time. The java.util.concurrent package is your best friend here — it has thread-safe collections, atomic variables, executors, locks, and concurrent data structures all ready to use. The key intuition is that concurrency problems happen when multiple threads access shared mutable state without proper coordination. If you remember that one rule you are already ahead of most people. Immutable objects are your safest bet because they cannot be changed after creation so no synchronization is needed. When you must use mutable state, use the right synchronization tool for the job. The hardest part of concurrency is not writing the code — it is debugging it because concurrency bugs are timing dependent and may not appear every time you run the program.

```java
import java.util.concurrent.*;
import java.util.concurrent.atomic.*;

public class ConcurrencyBigPicture {
    public static void main(String[] args) throws Exception {
        // Thread Pool — manage threads automatically
        ExecutorService executor = Executors.newFixedThreadPool(3);

        // Submit tasks
        for (int i = 1; i <= 5; i++) {
            int taskNum = i;
            executor.submit(() -> {
                System.out.println("Task " + taskNum + " by " + Thread.currentThread().getName());
            });
        }

        executor.shutdown();
        executor.awaitTermination(5, TimeUnit.SECONDS);
    }
}
// Output (order may vary):
// Task 1 by pool-1-thread-1
// Task 2 by pool-1-thread-2
// Task 3 by pool-1-thread-3
// Task 4 by pool-1-thread-1
// Task 5 by pool-1-thread-2
```
