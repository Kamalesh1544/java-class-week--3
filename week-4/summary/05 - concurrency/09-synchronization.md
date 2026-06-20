# Synchronization

Synchronization is the mechanism that controls access to shared resources by multiple threads. Without synchronization two threads trying to modify the same data at the same time can corrupt it — this is called a race condition. Think of it like a bathroom with one key — only one person can use it at a time, and they lock the door behind them so nobody else can walk in while they are inside. Synchronization works the same way — only one thread can execute a synchronized block or method on the same object at a time, other threads have to wait until the lock is released. Java provides several ways to synchronize — the synchronized keyword for simple cases, ReentrantLock for more flexible locking, ReadWriteLock for situations where you have many readers and few writers, and atomic variables for simple operations like incrementing a counter. The synchronized keyword can be applied to methods or blocks. When applied to a method the entire method is locked, when applied to a block you can lock on a specific object which gives you more fine-grained control. The key thing to remember is that synchronization comes with a performance cost — threads waiting for locks are not doing useful work so you want to keep synchronized sections as short as possible. Over-synchronizing can actually make your program slower than a single-threaded version. The goal is to synchronize just enough to keep data consistent without killing performance.

```java
// Without synchronization — race condition
class Counter {
    int count = 0;

    void increment() {
        count++;   // NOT atomic — read, add, write
    }
}

// Multiple threads calling increment() will lose updates

// With synchronized — safe
class SafeCounter {
    int count = 0;

    synchronized void increment() {
        count++;
    }
}

// Using synchronized block — more fine-grained
class FineGrainedCounter {
    int count = 0;
    Object lock = new Object();

    void increment() {
        synchronized (lock) {   // lock only this section
            count++;
        }
    }

    void doSomethingElse() {
        // this part runs without lock
    }
}

// Testing
public class SyncDemo {
    public static void main(String[] args) throws InterruptedException {
        SafeCounter counter = new SafeCounter();

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 100000; i++) counter.increment();
        });

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 100000; i++) counter.increment();
        });

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Count: " + counter.count);   // always 200000
    }
}
```
