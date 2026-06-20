# Abstract Factory

Abstract Factory is a creational pattern that gives you a way to create families of related objects without specifying their exact classes. Think of it like a factory that produces different types of products but all from the same family. For example if you are building a UI toolkit you might need buttons and text boxes for Windows and a different set for macOS. Instead of scattering if-else checks everywhere to decide which one to create, you use an Abstract Factory that produces the right set of objects for the current platform. You define an interface for the factory that declares methods for creating each type of product, and then you have concrete factories for each family — one for Windows, one for macOS, one for Linux. The client code only talks to the factory interface so it does not know or care which specific products it gets. This makes it super easy to add new families later without touching existing code. The key benefit is that it ensures the products from the same family are compatible with each other because they all come from the same factory. The tradeoff is that adding a new product type means updating the factory interface and all its implementations which can get annoying.

```java
// Product interfaces
interface Button { void render(); }
interface TextBox { void render(); }

// Windows family
class WindowsButton implements Button {
    public void render() { System.out.println("Windows Button"); }
}
class WindowsTextBox implements TextBox {
    public void render() { System.out.println("Windows TextBox"); }
}

// macOS family
class MacButton implements Button {
    public void render() { System.out.println("Mac Button"); }
}
class MacTextBox implements TextBox {
    public void render() { System.out.println("Mac TextBox"); }
}

// Abstract Factory
interface GUIFactory {
    Button createButton();
    TextBox createTextBox();
}

class WindowsFactory implements GUIFactory {
    public Button createButton() { return new WindowsButton(); }
    public TextBox createTextBox() { return new WindowsTextBox(); }
}

class MacFactory implements GUIFactory {
    public Button createButton() { return new MacButton(); }
    public TextBox createTextBox() { return new MacTextBox(); }
}

// Client code — does not know which platform
GUIFactory factory = new MacFactory();
Button button = factory.createButton();
TextBox textBox = factory.createTextBox();
button.render();      // Mac Button
textBox.render();     // Mac TextBox
```
