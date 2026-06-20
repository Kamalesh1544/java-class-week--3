# Adapter Pattern

Adapter pattern is used when you have two incompatible interfaces that need to work together. Instead of changing the existing code which might break other things, you create an adapter class that sits in between and translates one interface to the other. Think of it like a real world adapter — you have a three-pin plug but only a two-pin socket available, so you use an adapter to make it work. In code this happens all the time when you integrate third-party libraries or legacy systems. The new library expects a different interface than what your existing code provides so you write an adapter that wraps the old interface and presents it in the new format. The adapter does not change the underlying object, it just adds a translation layer. There are two types — class adapter which uses inheritance and object adapter which uses composition. Object adapter is more flexible because it can adapt any subclass of the adapted class. The key benefit is you avoid modifying existing tested code just to make it compatible with something new. You write one adapter class and everything works together without any changes to the original classes.

```java
// Old interface — what we have
interface OldPrinter {
    void printOld(String text);
}

class LegacyPrinter implements OldPrinter {
    public void printOld(String text) {
        System.out.println("Legacy: " + text);
    }
}

// New interface — what we need
interface NewPrinter {
    void print(String text);
}

// Adapter — makes old printer work with new interface
class PrinterAdapter implements NewPrinter {
    private OldPrinter oldPrinter;

    PrinterAdapter(OldPrinter oldPrinter) {
        this.oldPrinter = oldPrinter;
    }

    public void print(String text) {
        oldPrinter.printOld(text);   // translate new call to old call
    }
}

// Usage
OldPrinter legacy = new LegacyPrinter();
NewPrinter adapted = new PrinterAdapter(legacy);

adapted.print("Hello World");   // Legacy: Hello World
```
