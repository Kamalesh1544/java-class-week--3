# How Java Loads Classes

When you run a Java program the JVM does not load all your classes at once. It loads them on demand — meaning a class is only loaded when it is first referenced in the code. The whole process goes through three main phases — loading, linking, and initialization. Loading is when the ClassLoader reads the .class file from the disk or network and creates a Class object in memory. Linking has three steps — verification checks that the bytecode is valid and safe, preparation allocates memory for static variables and assigns default values, and resolution replaces symbolic references with actual memory addresses. Initialization is when the JVM runs the static initializer blocks and assigns actual values to static variables. The JVM uses a hierarchy of ClassLoaders — the Bootstrap ClassLoader loads core Java classes like String and Math from the JDK, the Extension ClassLoader loads classes from the ext directory, and the Application ClassLoader loads classes from your classpath. Each ClassLoader delegates to its parent first which is called the parent delegation model. So when your code references a class, the Application ClassLoader asks the Extension ClassLoader which asks the Bootstrap ClassLoader. If the parent can find it, it loads it, otherwise it comes back down the chain. This ensures core Java classes are never accidentally overridden by user code. Understanding this helps you figure out ClassNotFoundException and NoClassDefFoundError which are basically the JVM telling you it could not find a class somewhere in this chain.

```java
// See which classloader loaded a class
public class ClassLoaderDemo {
    public static void main(String[] args) {
        // String is loaded by Bootstrap ClassLoader (null because it is native)
        System.out.println(String.class.getClassLoader());
        // output: null

        // Your class is loaded by Application ClassLoader
        System.out.println(ClassLoaderDemo.class.getClassLoader());
        // output: sun.misc.Launcher$AppClassLoader@...

        // Check parent delegation
        ClassLoader appLoader = ClassLoaderDemo.class.getClassLoader();
        System.out.println("App loader: " + appLoader);
        System.out.println("Parent: " + appLoader.getParent());
        // Parent is Extension ClassLoader

        // Load a class dynamically
        try {
            Class<?> clazz = Class.forName("java.lang.String");
            System.out.println("Loaded: " + clazz.getName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```
