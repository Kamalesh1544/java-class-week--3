# Buffered Readers and Writers in Java

---

## What is Buffering?

Buffering means reading or writing data in **large chunks** instead of one byte/character at a time. This reduces the number of actual I/O operations and makes your program **much faster**.

Without buffering:
```
Read character 1 → disk access
Read character 2 → disk access
Read character 3 → disk access
(Each read is slow)
```

With buffering:
```
Read 8192 characters at once → disk access
Process character 1 from buffer (fast, in memory)
Process character 2 from buffer (fast, in memory)
...
(Only one disk access for many characters)
```

---

## BufferedReader

Wraps around another Reader and adds a **buffer** (default 8192 characters). Also provides the convenient `readLine()` method.

### Syntax:
```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));
```

### Methods:

| Method | Description |
|---|---|
| `read()` | Reads one character |
| `read(char[] cbuf, int off, int len)` | Reads characters into array |
| `readLine()` | Reads a full line of text |
| `close()` | Closes the reader |
| `ready()` | Checks if stream is ready to be read |

### Example — Reading line by line:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderDemo {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("input.txt"))) {
            String line;

            while ((line = br.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Example — Reading with line numbers:

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderLineNumbers {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new FileReader("input.txt"))) {
            String line;
            int lineNum = 1;

            while ((line = br.readLine()) != null) {
                System.out.println(lineNum + ": " + line);
                lineNum++;
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## BufferedWriter

Wraps around another Writer and adds a **buffer**. Reduces the number of disk writes by batching characters.

### Syntax:
```java
BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"));
```

### Methods:

| Method | Description |
|---|---|
| `write(int c)` | Writes one character |
| `write(char[] cbuf, int off, int len)` | Writes characters from array |
| `write(String str, int off, int len)` | Writes part of a string |
| `newLine()` | Writes a line separator |
| `flush()` | Forces buffer to be written to disk |
| `close()` | Flushes and closes the writer |

### Example — Writing to file:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterDemo {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("output.txt"))) {
            bw.write("First line");
            bw.newLine();
            bw.write("Second line");
            bw.newLine();
            bw.write("Third line");
            System.out.println("Written successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Example — Writing with loop:

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterLoop {
    public static void main(String[] args) {
        try (BufferedWriter bw = new BufferedWriter(new FileWriter("numbers.txt"))) {
            for (int i = 1; i <= 100; i++) {
                bw.write("Line " + i);
                bw.newLine();
            }
            System.out.println("100 lines written");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## BufferedReader + BufferedWriter — Copy a File

```java
import java.io.*;

public class BufferCopyDemo {
    public static void main(String[] args) {
        try (
            BufferedReader br = new BufferedReader(new FileReader("source.txt"));
            BufferedWriter bw = new BufferedWriter(new FileWriter("destination.txt"))
        ) {
            String line;

            while ((line = br.readLine()) != null) {
                bw.write(line);
                bw.newLine();
            }

            System.out.println("File copied successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Reading User Input with BufferedReader

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.io.IOException;

public class UserInputDemo {
    public static void main(String[] args) {
        try (BufferedReader br = new BufferedReader(new InputStreamReader(System.in))) {
            System.out.print("Enter your name: ");
            String name = br.readLine();

            System.out.print("Enter your age: ");
            int age = Integer.parseInt(br.readLine());

            System.out.println("Name: " + name + ", Age: " + age);
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## StringBuilder with BufferedWriter (Efficient)

```java
import java.io.*;

public class StringBuilderBufferDemo {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < 1000; i++) {
            sb.append("Line ").append(i).append("\n");
        }

        try (BufferedWriter bw = new BufferedWriter(new FileWriter("large_file.txt"))) {
            bw.write(sb.toString());
            System.out.println("Written using StringBuilder");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Performance Comparison

| Method | Speed | Buffer Size |
|---|---|---|
| FileReader (no buffer) | Slow | 1 character at a time |
| BufferedReader | Fast | 8192 characters |
| BufferedReader with large buffer | Faster | Custom buffer size |

### Custom buffer size:
```java
// Default: 8192 characters
BufferedReader br = new BufferedReader(new FileReader("file.txt"));

// Custom: 16384 characters (16 KB)
BufferedReader br = new BufferedReader(new FileReader("file.txt"), 16384);
```

---

## Key Points

- **BufferedReader** reads text efficiently with buffering
- **BufferedWriter** writes text efficiently with buffering
- `readLine()` in BufferedReader reads a full line (very useful)
- `newLine()` in BufferedWriter writes platform-specific line separator
- Always call `flush()` before closing if you need to ensure data is written
- Use **try-with-resources** to auto-close
- Buffered streams are **much faster** than non-buffered for large files
