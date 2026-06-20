# Introduction — Concurrency

Concurrency is one of those topics that sounds intimidating but at its core it is pretty simple. It is the ability of your program to do multiple things at the same time. Well technically on a single core CPU it is not really doing things at the same time — it is just switching between tasks so fast that it feels simultaneous. But on modern multi-core processors you can genuinely have multiple tasks running in parallel on different cores. Java has built-in support for concurrency through threads which are lightweight units of execution within a single process. Every Java program starts with one thread called the main thread and you can spawn additional threads to do work in the background. Concurrency becomes important when you have tasks that take time like reading from a database, calling an API, processing large files, or handling multiple user requests in a web server. Instead of doing them one after another you can do them simultaneously which makes your application much faster and more responsive. But with great power comes great responsibility — concurrent programming introduces problems like race conditions where two threads try to modify the same data at the same time and corrupt it, deadlocks where two threads are waiting for each other forever, and thread safety issues that are hard to reproduce and debug. The whole point of learning concurrency properly is to understand these problems and know how to prevent them.

```java
// Simple example — two threads running simultaneously
public class ConcurrencyIntro {
    public static void main(String[] args) {
        Thread t1 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Thread 1: " + i);
                try { Thread.sleep(500); } catch (Exception e) {}
            }
        });

        Thread t2 = new Thread(() -> {
            for (int i = 1; i <= 5; i++) {
                System.out.println("Thread 2: " + i);
                try { Thread.sleep(500); } catch (Exception e) {}
            }
        });

        t1.start();
        t2.start();
    }
}
// Output — interleaved, not sequential
// Thread 1: 1
// Thread 2: 1
// Thread 1: 2
// Thread 2: 2
// ...
```
