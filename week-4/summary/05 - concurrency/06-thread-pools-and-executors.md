# Thread Pools and Executors

Creating a new thread for every task is expensive — it takes time and memory to create and destroy threads. Thread pools solve this by maintaining a pool of pre-created threads that are reused for multiple tasks. Instead of creating a thread, running a task, and destroying the thread, you just submit the task to the pool and one of the available threads picks it up. When the thread finishes it goes back to the pool waiting for the next task. Java provides the ExecutorService interface and the Executors factory class to create thread pools easily. There are several types of thread pools — newFixedThreadPool creates a pool with a fixed number of threads, newCachedThreadPool creates threads on demand and reuses them when possible, newSingleThreadExecutor runs everything on a single thread which is useful when order matters, and newScheduledThreadPool lets you schedule tasks to run after a delay or periodically. When you are done you call shutdown() on the executor to stop accepting new tasks and awaitTermination() to wait for running tasks to finish. If you do not shut down the executor your application will not terminate because the pool threads are non-daemon threads. Thread pools are the standard way to handle concurrency in real applications — almost every web server, application server, and framework uses them internally.

```java
import java.util.concurrent.*;

public class ThreadPoolDemo {
    public static void main(String[] args) throws Exception {
        // Fixed pool — 3 threads
        ExecutorService pool = Executors.newFixedThreadPool(3);

        for (int i = 1; i <= 6; i++) {
            int taskNum = i;
            pool.submit(() -> {
                System.out.println("Task " + taskNum + " running in " + Thread.currentThread().getName());
                try { Thread.sleep(1000); } catch (Exception e) {}
            });
        }

        pool.shutdown();
        pool.awaitTermination(10, TimeUnit.SECONDS);
    }
}
// Output (6 tasks, 3 threads — runs in 2 batches):
// Task 1 running in pool-1-thread-1
// Task 2 running in pool-1-thread-2
// Task 3 running in pool-1-thread-3
// Task 4 running in pool-1-thread-1
// Task 5 running in pool-1-thread-2
// Task 6 running in pool-1-thread-3

// Scheduled pool — run task after delay
ScheduledExecutorService scheduler = Executors.newScheduledThreadPool(1);
scheduler.schedule(
    () -> System.out.println("Runs after 2 seconds"),
    2, TimeUnit.SECONDS
);
scheduler.shutdown();
```
