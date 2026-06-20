# Dependency Injection

Dependency Injection is basically a technique where an object does not create its own dependencies, instead they are provided or injected from outside. Think of it like a restaurant — you do not go into the kitchen and cook your own food, the kitchen staff brings it to you. In code instead of a class creating its own dependencies using new, those dependencies are passed to it through the constructor, a setter method, or a field. This makes your code way more flexible and easier to test because you can swap out implementations without changing the class that uses them. For example if your UserService needs a database connection, instead of creating it inside UserService you pass it through the constructor. Then in production you pass a real database connection and in tests you pass a mock one. This is the foundation of loose coupling and it is what frameworks like Spring do automatically — they manage creating objects and wiring their dependencies together so you do not have to do it manually. There are three main types — constructor injection where dependencies come through the constructor, setter injection where they come through setter methods, and field injection where the framework injects directly into fields. Constructor injection is the most recommended because it makes dependencies explicit and ensures the object is always in a valid state when created.

```java
// Without dependency injection — tightly coupled
class UserService {
    private Database db = new MySQLDatabase();   // hardcoded dependency
    // cannot easily test with a different database
}

// With dependency injection — loosely coupled
class UserService {
    private final Database db;

    UserService(Database db) {   // dependency injected through constructor
        this.db = db;
    }

    void findUser(String id) {
        db.query("SELECT * FROM users WHERE id = " + id);
    }
}

// In production
UserService service = new UserService(new MySQLDatabase());

// In tests — easy to mock
UserService service = new UserService(new MockDatabase());

// Interface makes it even more flexible
interface Database {
    void query(String sql);
}

class MySQLDatabase implements Database {
    public void query(String sql) { System.out.println("MySQL: " + sql); }
}

class MockDatabase implements Database {
    public void query(String sql) { System.out.println("Mock: " + sql); }
}
```
