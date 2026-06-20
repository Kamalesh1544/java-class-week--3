# Structural Patterns

Structural patterns are about how you assemble and connect different objects and classes to form larger structures while keeping them flexible and efficient. Think of it like building with LEGO blocks — you are combining smaller pieces into bigger structures but you want to make sure the connections are clean and you can swap pieces easily. The main idea is to simplify relationships between objects and make sure changes in one part do not ripple through the entire system. Adapter pattern lets incompatible interfaces work together by converting one interface to another — like a power adapter that lets you plug an Indian plug into a European socket. Decorator pattern adds new behavior to objects dynamically without modifying their original class — like wrapping a gift with different layers of paper and ribbon, each adding something new. Facade pattern provides a simple interface to a complex subsystem — instead of dealing with ten different classes you call one simple method. Proxy pattern creates a placeholder object that controls access to the real object — useful for lazy loading, access control, or logging. Bridge pattern separates abstraction from implementation so they can vary independently. Composite pattern lets you treat individual objects and compositions of objects uniformly — like how a folder can contain both files and other folders. The common theme is keeping things loosely coupled so you can modify, extend, or replace parts of your system without breaking everything else.

```java
// Adapter pattern — make incompatible interfaces work together
interface EuropeanSocket {
    int voltage();
    String plugType();
}

class EuropeanPlug implements EuropeanSocket {
    public int voltage() { return 230; }
    public String plugType() { return "Type C"; }
}

// Indian socket expects different interface
interface IndianSocket {
    int voltage();
    String plugType();
}

// Adapter — makes European plug work with Indian socket
class EuropeanToIndianAdapter implements IndianSocket {
    private EuropeanSocket european;

    EuropeanToIndianAdapter(EuropeanSocket european) {
        this.european = european;
    }

    public int voltage() { return european.voltage(); }
    public String plugType() { return "Type D (adapted from " + european.plugType() + ")"; }
}

// Usage
IndianSocket indian = new EuropeanToIndianAdapter(new EuropeanPlug());
System.out.println(indian.voltage());     // 230
System.out.println(indian.plugType());    // Type D (adapted from Type C)
```
