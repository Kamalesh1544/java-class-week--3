# Strategy Pattern

Strategy pattern lets you define a family of algorithms and make them interchangeable at runtime. Instead of hardcoding a specific behavior inside a class, you extract that behavior into a separate interface and let the class use whatever implementation is plugged in. This is super useful when you have multiple ways of doing the same thing and you want to switch between them without modifying the original class. For example a sorting class might support bubble sort, merge sort, and quick sort — instead of putting all that logic inside the sorter, you create a SortStrategy interface with a sort method and each algorithm becomes its own class that implements it. The sorter just holds a reference to the strategy and calls its sort method. At runtime you can swap strategies instantly — pass in a BubbleSort when you want simplicity or a QuickSort when you need speed. This follows the open-closed principle perfectly because you can add new algorithms without touching existing code. Real world uses include compression algorithms, payment methods, tax calculation rules, and validation strategies. The pattern makes your code flexible and testable because you can easily mock the strategy in unit tests.

```java
interface CompressionStrategy {
    void compress(String filename);
}

class ZipCompression implements CompressionStrategy {
    public void compress(String filename) {
        System.out.println("Compressing " + filename + " using ZIP");
    }
}

class GzipCompression implements CompressionStrategy {
    public void compress(String filename) {
        System.out.println("Compressing " + filename + " using GZIP");
    }
}

class FileCompressor {
    private CompressionStrategy strategy;

    FileCompressor(CompressionStrategy strategy) {
        this.strategy = strategy;
    }

    void setStrategy(CompressionStrategy strategy) {
        this.strategy = strategy;
    }

    void compress(String filename) {
        strategy.compress(filename);
    }
}

// Usage
FileCompressor compressor = new FileCompressor(new ZipCompression());
compressor.compress("photo.jpg");     // Compressing photo.jpg using ZIP

compressor.setStrategy(new GzipCompression());
compressor.compress("photo.jpg");     // Compressing photo.jpg using GZIP
```
