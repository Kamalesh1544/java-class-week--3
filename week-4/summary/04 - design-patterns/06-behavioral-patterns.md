# Behavioral Patterns

Behavioral patterns are all about how objects communicate with each other and how responsibilities are distributed among them. While creational patterns focus on how objects get created and structural patterns focus on how they are organized, behavioral patterns focus on the actual interaction — who does what, who talks to whom, and how the flow of control works. The core idea is to define clear contracts and communication paths between objects so that changes in one part do not break the whole system. Strategy pattern lets you swap algorithms at runtime without changing the class that uses them. Observer pattern lets objects subscribe to events so when something changes all the interested parties get notified automatically. Command pattern encapsulates an action as an object so you can queue, undo, or log operations. Template Method defines the skeleton of an algorithm in a base class but lets subclasses fill in specific steps. Iterator provides a uniform way to traverse different data structures without exposing their internals. State pattern lets an object change its behavior when its internal state changes. The common theme is loose coupling — objects know as little about each other as possible and communicate through well-defined interfaces. This makes your code easier to modify, test, and extend because you can swap out individual pieces without affecting the rest.

```java
// Strategy pattern — swap algorithms at runtime
interface SortStrategy {
    void sort(int[] array);
}

class BubbleSort implements SortStrategy {
    public void sort(int[] array) {
        System.out.println("Sorting with Bubble Sort");
    }
}

class QuickSort implements SortStrategy {
    public void sort(int[] array) {
        System.out.println("Sorting with Quick Sort");
    }
}

class Sorter {
    private SortStrategy strategy;

    Sorter(SortStrategy strategy) { this.strategy = strategy; }

    void setStrategy(SortStrategy strategy) { this.strategy = strategy; }

    void sort(int[] array) { strategy.sort(array); }
}

// Switch strategies at runtime
Sorter sorter = new Sorter(new BubbleSort());
sorter.sort(new int[]{5, 3, 1});   // Sorting with Bubble Sort

sorter.setStrategy(new QuickSort());
sorter.sort(new int[]{5, 3, 1});   // Sorting with Quick Sort
```
