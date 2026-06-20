# Big Picture and Intuition — Design Patterns

Design patterns are basically solutions to problems that developers kept running into over and over again. Think of them like recipes — someone already figured out the best way to handle a particular situation and wrote it down so you do not have to reinvent the wheel every time. They are not code you copy paste, they are more like ideas or templates that you adapt to your own situation. The whole point is to write code that is easier to maintain, extend, and understand. There are three main categories. Creational patterns deal with how you create objects — like how to make sure you only have one instance of something or how to build complex objects step by step. Structural patterns deal with how you organize and connect different parts of your code — like wrapping an old interface to make it work with new code. Behavioral patterns deal with how different parts of your code communicate and share responsibility — like deciding which algorithm to use at runtime. The Gang of Four book from 1994 introduced 23 classic patterns and they are still the foundation of good software design today. You do not need to memorize all of them right away, just understand the intuition behind each category and the most commonly used ones will naturally come up in your work.

```java
// Without pattern — tightly coupled, hard to change
class PaymentProcessor {
    void process() {
        // hardcoded credit card logic
    }
}

// With Strategy pattern — flexible, easy to extend
interface PaymentStrategy {
    void pay(double amount);
}

class CreditCard implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " via Credit Card");
    }
}

class UPI implements PaymentStrategy {
    public void pay(double amount) {
        System.out.println("Paid " + amount + " via UPI");
    }
}

// Now you can switch payment methods without changing existing code
```
