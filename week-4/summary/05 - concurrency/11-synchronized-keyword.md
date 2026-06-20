# The synchronized Keyword

The synchronized keyword is Java's built-in way to control access to shared resources by multiple threads. When you mark a method or block as synchronized, only one thread can execute that code at a time on the same object. Other threads have to wait until the lock is released. It works by using an intrinsic lock also called a monitor lock — every object in Java has one. When a thread enters a synchronized method it acquires the lock on that object, and when it exits the method it releases the lock. If another thread is already holding the lock, the new thread blocks and waits. There are two ways to use it — synchronized method where the lock is on the entire method, and synchronized block where you specify which object to lock on. Synchronized blocks are generally better because they let you lock only the critical section instead of the whole method which reduces the time other threads have to wait. One important thing is that synchronized on an instance method locks on the object instance, but synchronized on a static method locks on the Class object itself which means it blocks all threads across all instances. Also synchronized is reentrant which means a thread that already holds a lock can acquire it again without deadlocking itself — this is useful for recursive calls within synchronized methods. The downside of synchronized is that it can cause contention and deadlocks if not used carefully, and it does not work with try-with-resources or non-blocking operations.

```java
class BankAccount {
    private int balance;

    BankAccount(int balance) {
        this.balance = balance;
    }

    // Synchronized method — lock on 'this'
    synchronized void deposit(int amount) {
        balance += amount;
        System.out.println(Thread.currentThread().getName() + " deposited " + amount + ", balance: " + balance);
    }

    synchronized void withdraw(int amount) {
        if (balance >= amount) {
            balance -= amount;
            System.out.println(Thread.currentThread().getName() + " withdrew " + amount + ", balance: " + balance);
        } else {
            System.out.println(Thread.currentThread().getName() + " insufficient funds");
        }
    }

    // Synchronized block — more precise locking
    void transfer(BankAccount target, int amount) {
        synchronized (this) {          // lock this account
            synchronized (target) {    // lock target account
                this.withdraw(amount);
                target.deposit(amount);
            }
        }
    }
}

// Usage
public class SynchronizedDemo {
    public static void main(String[] args) throws InterruptedException {
        BankAccount account = new BankAccount(1000);

        Thread t1 = new Thread(() -> {
            for (int i = 0; i < 5; i++) account.deposit(100);
        }, "Alice");

        Thread t2 = new Thread(() -> {
            for (int i = 0; i < 5; i++) account.withdraw(150);
        }, "Bob");

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println("Final balance: " + account.balance);
    }
}
```
