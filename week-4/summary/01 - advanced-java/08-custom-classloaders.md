# Custom ClassLoaders

Custom ClassLoaders let you define your own way of loading classes into the JVM instead of relying on the default ones. This is useful when you need to load classes from non-standard locations like a database, an encrypted file, a network URL, or when you want to implement hot deployment where classes are reloaded without restarting the application. To create a custom ClassLoader you extend the ClassLoader class and override the findClass method. Inside findClass you read the bytes of the .class file from wherever your custom source is, then call defineClass to convert those bytes into an actual Class object. The parent delegation model still applies — your custom ClassLoader asks its parent first and only loads the class itself if the parent cannot find it. One important thing to remember is that each ClassLoader has its own namespace so the same class loaded by two different ClassLoaders is treated as two completely different classes by the JVM. This can cause issues like ClassCastException if you are not careful. Real world use cases include application servers like Tomcat that load each web application with its own ClassLoader so multiple apps can use different versions of the same library. IDEs use custom ClassLoaders to reload code as you edit it. Plugin systems use them to load plugin code dynamically without restarting the main application. OSGi is built entirely around custom ClassLoaders for modular Java applications. The key takeaway is that ClassLoaders give you fine-grained control over how classes enter the JVM which is essential for advanced deployment scenarios.

```java
import java.io.*;

public class CustomClassLoader extends ClassLoader {

    @Override
    protected Class<?> findClass(String name) throws ClassNotFoundException {
        String path = name.replace(".", File.separator) + ".class";

        try {
            // Read the .class file bytes
            byte[] bytes = readClassFile(path);

            // Convert bytes to Class object
            return defineClass(name, bytes, 0, bytes.length);
        } catch (IOException e) {
            throw new ClassNotFoundException("Cannot load " + name, e);
        }
    }

    private byte[] readClassFile(String path) throws IOException {
        // Could read from database, network, encrypted file, etc.
        File file = new File("build/classes/" + path);
        FileInputStream fis = new FileInputStream(file);
        byte[] bytes = new byte[(int) file.length()];
        fis.read(bytes);
        fis.close();
        return bytes;
    }

    // Usage
    public static void main(String[] args) throws Exception {
        CustomClassLoader loader = new CustomClassLoader();
        Class<?> clazz = loader.loadClass("com.example.MyClass");
        Object obj = clazz.getDeclaredConstructor().newInstance();
        System.out.println("Loaded: " + clazz.getName());
        System.out.println("ClassLoader: " + clazz.getClassLoader());
    }
}
```
