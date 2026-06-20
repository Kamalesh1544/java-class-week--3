# Decorator Pattern

Decorator pattern lets you add new behavior to an object dynamically without changing its class or modifying existing code. Think of it like wrapping a gift — you start with the base gift and then you add layers of wrapping paper, ribbon, and a card, each adding something extra without changing the gift itself. In code you create a base component interface and then a base implementation. Then you create decorator classes that also implement the same interface but wrap around the original object and add behavior before or after delegating the call to the wrapped object. The cool thing is you can stack multiple decorators on top of each other to combine behaviors. For example if you have a basic coffee you can wrap it with milk decorator and then sugar decorator to get a coffee with milk and sugar. Each decorator adds its own behavior and passes the rest to the next decorator. The client code does not know it is dealing with decorated objects, it just sees the base interface. This is super useful when you want to extend functionality without inheritance which can get rigid and messy. Java I/O streams use this pattern extensively — BufferedInputStream wraps FileInputStream to add buffering, DataInputStream wraps it to add data type reading, and so on.

```java
// Component interface
interface Coffee {
    String getDescription();
    double getCost();
}

// Base implementation
class SimpleCoffee implements Coffee {
    public String getDescription() { return "Simple Coffee"; }
    public double getCost() { return 50.0; }
}

// Decorator base
abstract class CoffeeDecorator implements Coffee {
    protected Coffee coffee;

    CoffeeDecorator(Coffee coffee) {
        this.coffee = coffee;
    }
}

// Concrete decorators
class MilkDecorator extends CoffeeDecorator {
    MilkDecorator(Coffee coffee) { super(coffee); }

    public String getDescription() { return coffee.getDescription() + " + Milk"; }
    public double getCost() { return coffee.getCost() + 20.0; }
}

class SugarDecorator extends CoffeeDecorator {
    SugarDecorator(Coffee coffee) { super(coffee); }

    public String getDescription() { return coffee.getDescription() + " + Sugar"; }
    public double getCost() { return coffee.getCost() + 10.0; }
}

// Usage — stack decorators
Coffee coffee = new SimpleCoffee();
coffee = new MilkDecorator(coffee);
coffee = new SugarDecorator(coffee);

System.out.println(coffee.getDescription());   // Simple Coffee + Milk + Sugar
System.out.println(coffee.getCost());           // 80.0
```
