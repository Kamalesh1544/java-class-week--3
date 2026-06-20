# Immutable Objects

Immutable objects are objects that cannot be changed after they are created. Once you set their values in the constructor, those values stay the same for the entire lifetime of the object. This is one of the simplest and most effective ways to handle thread safety because if an object cannot be modified, there is nothing to synchronize — multiple threads can read it freely without any risk of corruption. Think of it like a stone tablet — once the text is carved in it, nobody can change it. String in Java is immutable for this exact reason. To make a class immutable you follow a few rules — make the class final so it cannot be extended, make all fields private and final so they cannot be modified, do not provide setter methods, initialize all fields through the constructor, and if any field is a mutable object like a list or date, return a copy of it in the getter so the internal state cannot be modified from outside. The benefits are huge — thread safety without locks, no defensive copies needed, safer to use as map keys or set elements, and easier to reason about since the state never changes. The tradeoff is that every change creates a new object which can use more memory but in most cases the safety and simplicity are worth it. Java has many immutable classes built-in like String, Integer, Long, Double, LocalDate, and LocalDateTime.

```java
// Immutable class — all fields final, no setters
final class Money {
    private final double amount;
    private final String currency;

    Money(double amount, String currency) {
        this.amount = amount;
        this.currency = currency;
    }

    // No setters — no way to change values

    double getAmount() { return amount; }
    String getCurrency() { return currency; }

    // Safe to share between threads
    Money withAmount(double newAmount) {
        return new Money(newAmount, currency);   // return new object, don't modify
    }

    public String toString() {
        return amount + " " + currency;
    }
}

// Usage
public class ImmutableDemo {
    public static void main(String[] args) {
        Money money = new Money(100, "USD");

        // Cannot change it
        // money.amount = 200;   // compile error

        // Create new one instead
        Money updated = money.withAmount(200);

        System.out.println(money);      // 100.0 USD (unchanged)
        System.out.println(updated);    // 200.0 USD (new object)

        // Safe to use across threads — no synchronization needed
    }
}
```
