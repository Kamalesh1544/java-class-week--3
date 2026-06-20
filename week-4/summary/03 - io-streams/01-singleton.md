# Singleton Class in Java

---

## What is a Singleton?

A Singleton class allows only **one instance** of itself to be created throughout the application. No matter how many times you try to create an object, it always returns the same instance.

---

## Why Use Singleton?

- Database connections — one shared connection manager
- Configuration settings — one global config
- Logging — one shared logger
- Cache — one shared cache store
- Thread pools — one shared pool

---

## Steps to Create a Singleton

1. **Private constructor** — prevent `new` from outside
2. **Private static variable** — hold the single instance
3. **Public static method** — return the single instance

---

## Method 1: Eager Initialization

Instance is created **at class load time**. Simple and thread-safe.

```java
public class Database {
    private static Database instance = new Database();

    private Database() {
        System.out.println("Database instance created");
    }

    public static Database getInstance() {
        return instance;
    }

    public void connect() {
        System.out.println("Connected to database");
    }

    public static void main(String[] args) {
        Database db1 = Database.getInstance();
        Database db2 = Database.getInstance();

        db1.connect();
        System.out.println("Same instance? " + (db1 == db2));
    }
}
```

### Output:
```
Database instance created
Connected to database
Same instance? true
```

---

## Method 2: Lazy Initialization

Instance is created **only when needed**. Saves memory if instance is never used.

```java
public class Logger {
    private static Logger instance;

    private Logger() {
        System.out.println("Logger instance created");
    }

    public static Logger getInstance() {
        if (instance == null) {
            instance = new Logger();
        }
        return instance;
    }

    public void log(String message) {
        System.out.println("LOG: " + message);
    }

    public static void main(String[] args) {
        Logger l1 = Logger.getInstance();
        Logger l2 = Logger.getInstance();

        l1.log("Application started");
        System.out.println("Same instance? " + (l1 == l2));
    }
}
```

### Output:
```
Logger instance created
LOG: Application started
Same instance? true
```

---

## Method 3: Thread-Safe Singleton

Use `synchronized` to prevent multiple instances in multi-threaded environments.

```java
public class Config {
    private static Config instance;

    private Config() {
        System.out.println("Config loaded");
    }

    public static synchronized Config getInstance() {
        if (instance == null) {
            instance = new Config();
        }
        return instance;
    }

    public String getSetting(String key) {
        return "Value for " + key;
    }
}
```

---

## Method 4: Double-Checked Locking (Best Practice)

Combines thread safety with performance. Only synchronizes on first creation.

```java
public class Singleton {
    private static volatile Singleton instance;

    private Singleton() {
        System.out.println("Singleton created");
    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

---

## Method 5: Enum Singleton (Simplest)

The simplest and most recommended approach by Joshua Bloch.

```java
enum Database {
    INSTANCE;

    public void connect() {
        System.out.println("Connected");
    }
}

public class Main {
    public static void main(String[] args) {
        Database db = Database.INSTANCE;
        db.connect();
    }
}
```

---

## Comparison

| Method | Thread Safe | Lazy | Performance |
|---|---|---|---|
| Eager | Yes | No | Best |
| Lazy | No | Yes | Good |
| Synchronized | Yes | Yes | Slow |
| Double-Checked | Yes | Yes | Best |
| Enum | Yes | No | Best |

---

## Key Points

- Singleton ensures **only one instance** exists
- Constructor is **private**
- `getInstance()` returns the single instance
- Use **double-checked locking** for thread-safe lazy initialization
- Use **enum** for the simplest approach
