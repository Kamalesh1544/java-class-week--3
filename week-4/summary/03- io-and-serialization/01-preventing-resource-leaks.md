# Preventing Resource Leaks

So basically when you open a file or a database connection in Java, it stays open until you explicitly close it. If you forget to close it, the system keeps holding onto that resource even after your program is done using it. That is called a resource leak. It is like leaving the tap running after you are done washing your hands — the water keeps flowing and eventually the tank runs dry. Over time your program slows down, eats up memory, and might even crash. The old way to handle this was writing a try-catch-finally block where you manually close everything in the finally section, but that gets messy really fast especially when you have multiple resources to manage. The cleaner way is to use try-with-resources which was introduced in Java 7. You just declare your resources inside the try parentheses and Java automatically closes them when the block ends, even if an exception happens. So instead of writing ten lines of cleanup code, you write it in two lines and you are done. Always use try-with-resources for things like FileInputStream, BufferedReader, Database connections — basically anything that implements AutoCloseable. It saves you from bugs and makes your code way cleaner.

```java
// Old way — messy
FileInputStream fis = null;
try {
    fis = new FileInputStream("file.txt");
    // do something
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (fis != null) {
        try { fis.close(); } catch (IOException e) {}
    }
}

// New way — clean
try (FileInputStream fis = new FileInputStream("file.txt")) {
    // do something
} catch (IOException e) {
    e.printStackTrace();
}
// fis is automatically closed here
```
