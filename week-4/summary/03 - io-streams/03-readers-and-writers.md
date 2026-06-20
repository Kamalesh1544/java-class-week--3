# Readers and Writers in Java

---

## What are Readers and Writers?

Readers and Writers are **character-based streams** used to read and write **text data**. They handle character encoding automatically, making them ideal for working with text files.

| Type | Direction | Purpose |
|---|---|---|
| **Reader** | Source → Program | Read characters |
| **Writer** | Program → Destination | Write characters |

---

## Reader Classes

| Class | Description |
|---|---|
| `FileReader` | Reads characters from a file |
| `StringReader` | Reads characters from a String |
| `CharArrayReader` | Reads characters from a char array |
| `BufferedReader` | Adds buffering to another reader |
| `InputStreamReader` | Bridges byte stream to character stream |
| `PushbackReader` | Allows pushing back characters |

### Common Reader Methods:

| Method | Description |
|---|---|
| `read()` | Reads one character (-1 if end) |
| `read(char[] cbuf)` | Reads characters into array |
| `read(char[] cbuf, int off, int len)` | Reads len characters into array |
| `close()` | Closes the reader |

---

## Writer Classes

| Class | Description |
|---|---|
| `FileWriter` | Writes characters to a file |
| `StringWriter` | Writes characters to a String |
| `CharArrayWriter` | Writes characters to a char array |
| `BufferedWriter` | Adds buffering to another writer |
| `PrintWriter` | Adds print/println convenience methods |
| `OutputStreamWriter` | Bridges character stream to byte stream |

### Common Writer Methods:

| Method | Description |
|---|---|
| `write(int c)` | Writes one character |
| `write(char[] cbuf)` | Writes character array |
| `write(String str)` | Writes a string |
| `write(String str, int off, int len)` | Writes part of a string |
| `flush()` | Forces buffered output to be written |
| `close()` | Closes the writer |

---

## Reading a Text File — FileReader

```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderDemo {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("input.txt")) {
            int character;

            while ((character = fr.read()) != -1) {
                System.out.print((char) character);
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Reading into a char array:
```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderArrayDemo {
    public static void main(String[] args) {
        try (FileReader fr = new FileReader("input.txt")) {
            char[] buffer = new char[1024];
            int charsRead = fr.read(buffer);

            System.out.println(new String(buffer, 0, charsRead));
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Writing to a Text File — FileWriter

```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterDemo {
    public static void main(String[] args) {
        try (FileWriter fw = new FileWriter("output.txt")) {
            fw.write("Hello, World!\n");
            fw.write("This is line 2.\n");
            fw.write("This is line 3.\n");
            System.out.println("Written successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Append mode:
```java
FileWriter fw = new FileWriter("output.txt", true);   // append = true
```

---

## InputStreamReader — Bridge from Byte to Character

Converts a byte stream into a character stream. Useful when you need to specify encoding.

```java
import java.io.*;
import java.util.Scanner;

public class InputStreamReaderDemo {
    public static void main(String[] args) {
        // Read from System.in (keyboard) with specified encoding
        try (InputStreamReader isr = new InputStreamReader(System.in, "UTF-8");
             BufferedReader br = new BufferedReader(isr)) {

            System.out.print("Enter your name: ");
            String name = br.readLine();
            System.out.println("Hello, " + name + "!");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## OutputStreamWriter — Bridge from Character to Byte

Converts a character stream into a byte stream.

```java
import java.io.*;

public class OutputStreamWriterDemo {
    public static void main(String[] args) {
        try (OutputStreamWriter osw = new OutputStreamWriter(
                new FileOutputStream("output.txt"), "UTF-8")) {
            osw.write("Hello, UTF-8!\n");
            System.out.println("Written with encoding");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## PrintWriter — Easy Text Output

Provides `print()` and `println()` methods for easy writing.

```java
import java.io.PrintWriter;
import java.io.IOException;

public class PrintWriterDemo {
    public static void main(String[] args) {
        try (PrintWriter pw = new PrintWriter("output.txt")) {
            pw.println("Name: Alice");
            pw.println("Age: 25");
            pw.println("City: Chennai");
            pw.printf("Score: %.2f%n", 95.5);
            System.out.println("Written successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Output file content:
```
Name: Alice
Age: 25
City: Chennai
Score: 95.50
```

---

## Reading and Writing — Complete Example

```java
import java.io.*;

public class ReadWriteDemo {
    public static void main(String[] args) {
        // Write
        try (PrintWriter pw = new PrintWriter("data.txt")) {
            pw.println("Apple");
            pw.println("Banana");
            pw.println("Cherry");
            pw.println("Date");
        } catch (IOException e) {
            System.out.println("Write error: " + e.getMessage());
        }

        // Read
        System.out.println("--- File Content ---");
        try (BufferedReader br = new BufferedReader(new FileReader("data.txt"))) {
            String line;
            int count = 1;
            while ((line = br.readLine()) != null) {
                System.out.println(count + ". " + line);
                count++;
            }
        } catch (IOException e) {
            System.out.println("Read error: " + e.getMessage());
        }
    }
}
```

### Output:
```
--- File Content ---
1. Apple
2. Banana
3. Cherry
4. Date
```

---

## Key Points

- **Readers** read characters, **Writers** write characters
- Use `FileReader` and `FileWriter` for text files
- Use `InputStreamReader`/`OutputStreamWriter` to bridge byte and character streams
- Use `PrintWriter` for convenient `print()`/`println()` output
- Always use **try-with-resources** to auto-close readers and writers
- Character streams handle **encoding** automatically (UTF-8, etc.)
