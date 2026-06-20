# Edge Case: Dependency Injection Disadvantages

Dependency Injection is great but it is not perfect and understanding its downsides helps you use it wisely. The biggest issue is the added complexity — especially for beginners. Instead of just creating an object with new you now have to set up a container, configure dependencies, understand scopes, and deal with annotations like @Autowired and @Component. This can feel overwhelming when you are just trying to write a simple application. Another problem is that it can make debugging harder because the flow of object creation is no longer visible in your code — you have to dig into configuration files or framework internals to figure out where a dependency is coming from. Runtime errors replace compile time errors which means instead of catching problems early you discover them when the application actually runs. There is also the issue of over-engineering — sometimes people inject dependencies into everything even when a simple new would work just fine and the code would be clearer. Circular dependencies are another headache where class A depends on B and B depends on A and the framework has to resolve this somehow which can lead to unexpected behavior. Testing gets simpler with DI but setting up the test environment with all the mocking frameworks and containers adds its own complexity. Performance overhead is minimal but the framework startup time can be noticeable in large applications. The key takeaway is that DI is a powerful tool for large enterprise applications but for small projects or learning purposes it might add more complexity than it removes.

```java
// Simple without DI — clear and easy to understand
class OrderService {
    private Database db = new MySQLDatabase();

    void placeOrder(String item) {
        db.save("INSERT INTO orders VALUES ('" + item + "')");
    }
}

// With DI — more flexible but more setup needed
class OrderService {
    private final Database db;
    private final Logger logger;

    OrderService(Database db, Logger logger) {
        this.db = db;
        this.logger = logger;
    }

    void placeOrder(String item) {
        logger.log("Placing order for " + item);
        db.save("INSERT INTO orders VALUES ('" + item + "')");
    }
}

// Now you need to wire it together
// In Spring this is automatic but you still need to understand:
// - Which bean provides Database?
// - Which bean provides Logger?
// - What scope are they in (singleton, prototype)?
// - What if there are circular dependencies?
```
