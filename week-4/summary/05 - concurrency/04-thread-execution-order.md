# Edge Case: Thread Execution Order

One thing that surprises a lot of people when they start with concurrency is that you cannot predict the order in which threads will execute. When you start multiple threads the JVM and the operating system decide which thread gets CPU time and when. This is called scheduling and it is non-deterministic. So if you run the same program ten times you might get different output each time. This is not a bug — it is just how concurrency works. The thread scheduler decides based on factors like thread priority, CPU availability, and system load. You cannot rely on threads executing in any particular order unless you explicitly synchronize them. This can cause serious bugs if your code makes assumptions about execution order. For example if Thread A is supposed to set a value and Thread B is supposed to read it, you cannot just start both and hope B runs after A. B might run before A and read a stale or default value. This is called a race condition and it is one of the most common concurrency bugs. The solutions are using join() to wait for a thread to finish, using synchronized blocks to enforce ordering, using locks, or using concurrent utilities like CountDownLatch or CyclicBarrier. The takeaway is that concurrency is inherently unpredictable and your code should work correctly regardless of execution order.

```java
// Execution order is unpredictable
public class OrderDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 5; i++) {
            Thread t1 = new Thread(() -> System.out.print("A"));
            Thread t2 = new Thread(() -> System.out.print("B"));

            t1.start();
            t2.start();

            try {
                t1.join();   // wait for t1 to finish
                t2.join();   // wait for t2 to finish
            } catch (InterruptedException e) {}

            System.out.println();
        }
    }
}
// Without join — random output like ABBAABABBA
// With join — still AB or BA, but consistent within each pair

// Using join for guaranteed order
public class OrderedExecution {
    public static void main(String[] args) throws InterruptedException {
        Thread first = new Thread(() -> System.out.println("First"));
        Thread second = new Thread(() -> System.out.println("Second"));

        first.start();
        first.join();      // wait for first to finish

        second.start();    // only starts after first is done
        second.join();
    }
}
// Output:
// First
// Second
```
