# Threads

A thread is the smallest unit of execution in a Java program. Every Java application starts with one thread called the main thread and you can create additional threads to do work in parallel. There are two ways to create a thread — extending the Thread class or implementing the Runnable interface. The Runnable approach is preferred because Java does not support multiple inheritance so implementing Runnable keeps your class free to extend something else. When you call start() on a thread the JVM creates a new system thread and calls the run() method on it. Important thing — never call run() directly because that just runs the method on the current thread without creating a new one. The start() method is what actually creates a separate thread. Each thread has its own call stack but they all share the same heap memory which is where concurrency problems come from — multiple threads accessing the same objects in the heap. A thread can be in several states — NEW when created, RUNNABLE when started and waiting for CPU time, BLOCKED when waiting for a lock, WAITING when waiting indefinitely, TIMED_WAITING when waiting for a specific time, and TERMINATED when finished. You can control threads using methods like sleep() to pause execution, join() to wait for another thread to finish, and yield() to give other threads a chance to run.

```java
// Method 1: Implement Runnable (preferred)
public class ThreadDemo {
    public static void main(String[] args) {
        Runnable task = () -> {
            String name = Thread.currentThread().getName();
            System.out.println("Running in: " + name);
        };

        Thread t1 = new Thread(task, "Worker-1");
        Thread t2 = new Thread(task, "Worker-2");

        t1.start();
        t2.start();
    }
}

// Method 2: Extend Thread
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Running in: " + getName());
    }
}

// Thread methods
public class ThreadMethods {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            try {
                Thread.sleep(1000);   // pause for 1 second
                System.out.println("Thread done");
            } catch (InterruptedException e) {}
        });

        t1.start();
        t1.join();   // main thread waits for t1 to finish

        System.out.println("Main continues after t1 finishes");

        // Thread states
        System.out.println("State: " + t1.getState());   // TERMINATED
    }
}
```
