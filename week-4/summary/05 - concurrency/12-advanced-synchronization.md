# Advanced Synchronization

Beyond the basic synchronized keyword Java has a whole toolbox of advanced synchronization mechanisms in the java.util.concurrent package that give you more flexibility, better performance, and finer control over concurrency. ReentrantLock is like synchronized but more powerful — it supports tryLock() which tries to acquire the lock without waiting, lockInterruptibly() which lets you interrupt a waiting thread, and timed locks which wait only for a specified duration. ReadWriteLock separates read and write locks — multiple threads can read simultaneously but writes require exclusive access which is great for read-heavy scenarios. Semaphore controls how many threads can access a resource at once — like limiting a parking lot to 100 cars. CountDownLatch lets one or more threads wait until a set of operations completes — useful for coordinating startup or shutdown. CyclicBarrier makes a group of threads wait for each other at a certain point before continuing — like runners waiting at the starting line. Phaser is a more flexible version of CyclicBarrier that supports dynamic thread registration. Atomic classes like AtomicInteger and AtomicLong provide lock-free thread-safe operations on single variables using hardware-level compare-and-swap operations which are faster than locks for simple operations. The general principle is to use the simplest mechanism that works for your use case — synchronized for simple cases, ReentrantLock for complex cases, atomic variables for simple counters, and higher-level constructs like CountDownLatch and CyclicBarrier for coordination between threads.

```java
import java.util.concurrent.locks.*;
import java.util.concurrent.*;

public class AdvancedSyncDemo {
    public static void main(String[] args) throws Exception {
        // ReentrantLock — more flexible than synchronized
        ReentrantLock lock = new ReentrantLock();

        lock.lock();
        try {
            // critical section
        } finally {
            lock.unlock();   // always unlock in finally
        }

        // tryLock — non-blocking
        if (lock.tryLock()) {
            try { /* do work */ }
            finally { lock.unlock(); }
        } else {
            System.out.println("Could not acquire lock");
        }

        // ReadWriteLock — multiple readers, single writer
        ReadWriteLock rwLock = new ReentrantReadWriteLock();
        rwLock.readLock().lock();     // multiple threads can hold this
        rwLock.readLock().unlock();
        rwLock.writeLock().lock();    // exclusive — only one thread
        rwLock.writeLock().unlock();

        // Semaphore — limit concurrent access
        Semaphore semaphore = new Semaphore(3);   // allow 3 threads
        semaphore.acquire();   // reduces permit count
        // do work
        semaphore.release();   // increases permit count

        // CountDownLatch — wait for events
        CountDownLatch latch = new CountDownLatch(3);
        latch.countDown();   // reduce count
        latch.countDown();
        latch.countDown();
        latch.await();       // blocks until count reaches 0

        // AtomicInteger — lock-free counter
        java.util.concurrent.atomic.AtomicInteger atomicCount = new java.util.concurrent.atomic.AtomicInteger(0);
        atomicCount.incrementAndGet();   // thread-safe increment
        atomicCount.addAndGet(5);        // thread-safe add
        System.out.println("Atomic: " + atomicCount.get());
    }
}
```
