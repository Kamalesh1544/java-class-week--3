# Creational Patterns

Creational patterns are all about how you create objects. Instead of just calling new ClassName() everywhere and hoping for the best, these patterns give you structured ways to create objects that are flexible, reusable, and follow good design principles. The main idea is to separate the logic of creating objects from the actual code that uses them so that the creation process can evolve without breaking everything else. Singleton is probably the most well-known one — it makes sure only one instance of a class ever gets created which is useful for things like database connections or configuration managers. Factory Method lets you define an interface for creating objects but let subclasses decide which class to instantiate — so the creation logic is deferred to the subclasses. Abstract Factory takes this further by giving you a way to create families of related objects without specifying their exact classes. Builder is great when you need to create complex objects step by step — instead of having a constructor with twenty parameters you call methods like .setName().setAge().build() which reads way better. Prototype lets you create new objects by copying an existing one instead of building from scratch which is useful when object creation is expensive. The common thread across all these patterns is that they give you control and flexibility over the creation process so your code stays clean and adaptable as requirements change.

```java
// Singleton — one instance only
class Database {
    private static Database instance = new Database();
    private Database() {}
    public static Database getInstance() { return instance; }
}

// Builder — step by step construction
class Pizza {
    String size;
    boolean cheese;
    boolean pepperoni;

    static class Builder {
        private Pizza pizza = new Pizza();

        Builder size(String size) { pizza.size = size; return this; }
        Builder cheese(boolean cheese) { pizza.cheese = cheese; return this; }
        Builder pepperoni(boolean pepperoni) { pizza.pepperoni = pepperoni; return this; }
        Pizza build() { return pizza; }
    }
}

Pizza pizza = new Pizza.Builder()
    .size("Large")
    .cheese(true)
    .pepperoni(true)
    .build();
```
