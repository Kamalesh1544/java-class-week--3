# Input Stream and Output Stream in Java

---

## What are Streams?

Streams are sequences of data that flow from a source to a destination. In Java I/O, streams provide a way to read from and write to data sources like files, network connections, and memory.

---

## Two Types of Streams

| Type | Direction | Purpose |
|---|---|---|
| **InputStream** | Source → Program | Read data |
| **OutputStream** | Program → Destination | Write data |

---

## Byte Streams (for binary data)

### InputStream — Read Data

`InputStream` is the **abstract base class** for all byte input streams.

| Class | Description |
|---|---|
| `FileInputStream` | Reads from a file |
| `ByteArrayInputStream` | Reads from a byte array |
| `BufferedInputStream` | Adds buffering to another input stream |
| `DataInputStream` | Reads primitive types (int, double, etc.) |
| `ObjectInputStream` | Reads objects from a stream |

### Common InputStream Methods:

| Method | Description |
|---|---|
| `read()` | Reads one byte (-1 if end of stream) |
| `read(byte[] b)` | Reads bytes into array |
| `read(byte[] b, int off, int len)` | Reads len bytes into array starting at offset |
| `available()` | Returns number of bytes available |
| `close()` | Closes the stream |
| `skip(long n)` | Skips n bytes |

---

### OutputStream — Write Data

`OutputStream` is the **abstract base class** for all byte output streams.

| Class | Description |
|---|---|
| `FileOutputStream` | Writes to a file |
| `ByteArrayOutputStream` | Writes to a byte array |
| `BufferedOutputStream` | Adds buffering to another output stream |
| `DataOutputStream` | Writes primitive types |
| `ObjectOutputStream` | Writes objects to a stream |

### Common OutputStream Methods:

| Method | Description |
|---|---|
| `write(int b)` | Writes one byte |
| `write(byte[] b)` | Writes byte array |
| `write(byte[] b, int off, int len)` | Writes len bytes from array |
| `flush()` | Forces any buffered output to be written |
| `close()` | Closes the stream |

---

## Reading a File — FileInputStream

```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamDemo {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.txt")) {
            int data;

            // read() returns -1 when end of file is reached
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Reading into a byte array (faster):
```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamArrayDemo {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.txt")) {
            byte[] buffer = new byte[1024];
            int bytesRead = fis.read(buffer);

            System.out.println("Bytes read: " + bytesRead);
            System.out.println(new String(buffer, 0, bytesRead));
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## Writing to a File — FileOutputStream

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo {
    public static void main(String[] args) {
        // Overwrites existing file
        try (FileOutputStream fos = new FileOutputStream("output.txt")) {
            String text = "Hello, World!";
            fos.write(text.getBytes());
            System.out.println("Written successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

### Append to file (add to end):
```java
// Pass true as second parameter to append
FileOutputStream fos = new FileOutputStream("output.txt", true);
```

---

## Copying a File

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopyDemo {
    public static void main(String[] args) {
        try (
            FileInputStream fis = new FileInputStream("source.txt");
            FileOutputStream fos = new FileOutputStream("destination.txt")
        ) {
            byte[] buffer = new byte[1024];
            int bytesRead;

            while ((bytesRead = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, bytesRead);
            }

            System.out.println("File copied successfully");
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
```

---

## try-with-resources

Always use try-with-resources to ensure streams are **automatically closed** after use. This prevents resource leaks.

```java
// Without try-with-resources (old way)
FileInputStream fis = null;
try {
    fis = new FileInputStream("file.txt");
    // use fis
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (fis != null) {
        fis.close();   // may throw exception
    }
}

// With try-with-resources (new way — recommended)
try (FileInputStream fis = new FileInputStream("file.txt")) {
    // use fis
} catch (IOException e) {
    e.printStackTrace();
}
// fis is automatically closed here
```

---

## Byte Stream vs Character Stream

| Feature | Byte Stream | Character Stream |
|---|---|---|
| Data type | Raw bytes (binary) | Characters (text) |
| Base classes | InputStream, OutputStream | Reader, Writer |
| Encoding | No encoding | Uses character encoding (UTF-8, etc.) |
| Use case | Images, audio, binary files | Text files |

---

## Key Points

- **InputStream** reads data, **OutputStream** writes data
- Always use **try-with-resources** to auto-close streams
- Use `read()` for single byte, `read(byte[])` for bulk read
- Use `write(byte[])` to write data
- Use `flush()` to force buffered data to be written
- Byte streams are for **binary data**, character streams are for **text data**
