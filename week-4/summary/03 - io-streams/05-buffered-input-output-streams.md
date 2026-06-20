# Buffered Input and Output Streams in Java

---

## What are Buffered Streams?

Buffered streams add a **memory buffer** to byte streams. Instead of reading/writing one byte at a time to disk (slow), they read/write a chunk of bytes at once (fast).

```
Without buffer:
Program ← 1 byte ← Disk
Program ← 1 byte ← Disk
Program ← 1 byte ← Disk
(3 disk operations)

With buffer:
Program ← 8192 bytes ← Buffer ← Disk
(1 disk operation for 8192 bytes)
```

---

## BufferedInputStream

Wraps around an `InputStream` and adds buffering.

### Default buffer size: 8192 bytes

### Syntax:
```java
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.dat"));
```

### Methods:

| Method | Description |
|---|---|
| `read()` | Reads one byte |
| `read(byte[] b)` | Reads bytes into array |
| `read(byte[] b, int off, int len)` | Reads len bytes into array |
| `available()` | Returns estimated bytes available |
| `skip(long n)` | Skips n bytes |
| `mark(int readlimit)` | Marks current position |
| `reset()` | Returns to marked position |
| `close()` | Closes the stream |

### Example — Reading binary file:

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class BufferedInputStreamDemo {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("data.dat"))) {
            int data;

            while ((data = bis.read()) != -1) {
                System.out.print(data + " ");
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Example — Reading into byte array (faster):

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class BufferedInputStreamArrayDemo {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("data.dat"))) {
            byte[] buffer = new byte[4096];
            int bytesRead;

            while ((bytesRead = bis.read(buffer)) != -1) {
                System.out.println("Read " + bytesRead + " bytes");
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## BufferedOutputStream

Wraps around an `OutputStream` and adds buffering.

### Default buffer size: 8192 bytes

### Syntax:
```java
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.dat"));
```

### Methods:

| Method | Description |
|---|---|
| `write(int b)` | Writes one byte |
| `write(byte[] b)` | Writes byte array |
| `write(byte[] b, int off, int len)` | Writes len bytes from array |
| `flush()` | Forces buffer to be written to disk |
| `close()` | Flushes and closes the stream |

### Example — Writing binary data:

```java
import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class BufferedOutputStreamDemo {
    public static void main(String[] args) {
        try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("output.dat"))) {
            byte[] data = "Hello, Binary World!".getBytes();
            bos.write(data);
            System.out.println("Written successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Copying a File with Buffered Streams

```java
import java.io.*;

public class BufferedCopyDemo {
    public static void main(String[] args) {
        try (
            BufferedInputStream bis = new BufferedInputStream(new FileInputStream("source.dat"));
            BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("destination.dat"))
        ) {
            byte[] buffer = new byte[8192];
            int bytesRead;

            while ((bytesRead = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, bytesRead);
            }

            System.out.println("File copied successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Performance Comparison — Buffered vs Non-Buffered

```java
import java.io.*;

public class PerformanceDemo {
    public static void main(String[] args) {
        int size = 1000000;

        // Without buffer
        long start1 = System.currentTimeMillis();
        try (FileOutputStream fos = new FileOutputStream("no_buffer.dat")) {
            for (int i = 0; i < size; i++) {
                fos.write(i % 256);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end1 = System.currentTimeMillis();
        System.out.println("Without buffer: " + (end1 - start1) + "ms");

        // With buffer
        long start2 = System.currentTimeMillis();
        try (BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("with_buffer.dat"))) {
            for (int i = 0; i < size; i++) {
                bos.write(i % 256);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end2 = System.currentTimeMillis();
        System.out.println("With buffer: " + (end2 - start2) + "ms");
    }
}
```

### Typical Output:
```
Without buffer: 120ms
With buffer: 8ms
```

---

## Custom Buffer Size

You can specify the buffer size for better performance with large files:

```java
// Default: 8192 bytes (8 KB)
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.dat"));

// Custom: 16384 bytes (16 KB)
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.dat"), 16384);

// Custom: 65536 bytes (64 KB) — for very large files
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("file.dat"), 65536);
```

---

## Mark and Reset

BufferedInputStream supports marking a position and returning to it later.

```java
import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class MarkResetDemo {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(new FileInputStream("data.txt"))) {
            // Read first 5 bytes
            byte[] first = new byte[5];
            bis.read(first);
            System.out.println("First 5: " + new String(first));

            // Mark position
            bis.mark(10);   // readlimit = 10 bytes

            // Read next 5 bytes
            byte[] second = new byte[5];
            bis.read(second);
            System.out.println("Next 5: " + new String(second));

            // Reset to marked position
            bis.reset();

            // Read again from mark
            byte[] again = new byte[5];
            bis.read(again);
            System.out.println("Again: " + new String(again));

        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Output (for file with "ABCDEFGHIJ"):
```
First 5: ABCDE
Next 5: FGHIJ
Again: FGHIJ
```

---

## Buffered vs Non-Buffered — When to Use

| Scenario | Use |
|---|---|
| Reading small files | Non-buffered is fine |
| Reading large files | Buffered (much faster) |
| Reading one byte at a time | Buffered (prevents many disk reads) |
| Random access with mark/reset | BufferedInputStream |
| Writing large data | BufferedOutputStream |

---

## Key Points

- **BufferedInputStream** and **BufferedOutputStream** add buffering to byte streams
- Default buffer size is **8192 bytes**
- Buffered streams are **much faster** for large files
- Always call `flush()` before closing (or use try-with-resources)
- Use `mark()` and `reset()` to revisit positions in BufferedInputStream
- Custom buffer size can improve performance for very large files
- Buffered streams do **not** close the underlying stream when you close them (unless using try-with-resources)
