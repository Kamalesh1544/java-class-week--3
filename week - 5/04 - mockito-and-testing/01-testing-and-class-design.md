# Testing and Class Design

## How Design Affects Testability
Good class design makes testing easy. Bad design makes testing hard. The way you structure your code directly impacts how easily you can write tests.

## Principles for Testable Code

### 1. Single Responsibility Principle
Each class should do one thing well. Small focused classes are easier to test than large multi-purpose classes.

```java
// Hard to test - does too many things
class UserManager {
    void createUser(String name) { ... }
    void sendEmail(String to, String body) { ... }
    void generateReport() { ... }
    void connectToDatabase() { ... }
}

// Easy to test - one responsibility each
class UserValidator {
    boolean isValid(String name) { ... }
}

class UserRepository {
    void save(User user) { ... }
}

class EmailService {
    void send(String to, String body) { ... }
}
```

### 2. Dependency Injection
Pass dependencies through constructors instead of creating them internally.

```java
// Hard to test - creates dependencies internally
class OrderService {
    private final Database db = new Database();  // Cannot mock
    
    void processOrder(Order order) {
        db.save(order);  // Cannot control behavior
    }
}

// Easy to test - dependencies injected
class OrderService {
    private final Database db;
    
    OrderService(Database db) {  // Inject dependency
        this.db = db;
    }
    
    void processOrder(Order order) {
        db.save(order);  // Can mock and control
    }
}
```

### 3. Program to Interfaces
Depend on abstractions, not concrete implementations.

```java
// Hard to test - depends on concrete class
class NotificationService {
    private final EmailSender sender = new SmtpEmailSender();
}

// Easy to test - depends on interface
class NotificationService {
    private final EmailSender sender;  // Can use any implementation
    
    NotificationService(EmailSender sender) {
        this.sender = sender;
    }
}
```

### 4. Avoid Static Methods
Static methods are hard to mock and test.

```java
// Hard to test
class DateHelper {
    static LocalDate today() {
        return LocalDate.now();  // Cannot mock
    }
}

// Easy to test
class DateHelper {
    private final Clock clock;
    
    DateHelper(Clock clock) {
        this.clock = clock;
    }
    
    LocalDate today() {
        return LocalDate.now(clock);  // Can inject mock clock
    }
}
```

## Testable vs Untestable Design
| Aspect | Testable | Untestable |
|--------|----------|------------|
| Dependencies | Injected | Created internally |
| Methods | Instance methods | Static methods |
| Classes | Small, focused | Large, multi-purpose |
| Coupling | Loose | Tight |
| State | Mutable with control | Global state |

## Refactoring for Testability
```java
// Before: Hard to test
class ReportGenerator {
    String generate() {
        Database db = new Database();      // Cannot mock
        List<Data> data = db.getAll();     // Cannot control
        String formatted = format(data);   // Tightly coupled
        EmailService.send(formatted);      // Side effect
        return formatted;
    }
}

// After: Easy to test
class ReportGenerator {
    private final DataRepository repository;
    private final ReportFormatter formatter;
    private final NotificationService notifier;
    
    ReportGenerator(DataRepository repo, 
                    ReportFormatter fmt,
                    NotificationService notify) {
        this.repository = repo;
        this.formatter = fmt;
        this.notifier = notify;
    }
    
    String generate() {
        List<Data> data = repository.getAll();
        String formatted = formatter.format(data);
        notifier.send(formatted);
        return formatted;
    }
}
```
