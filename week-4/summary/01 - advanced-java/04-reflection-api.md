# Reflection API

Reflection is the ability to inspect and manipulate classes, methods, fields, and constructors at runtime without knowing them at compile time. It is like having X-ray vision for your code — you can look inside any object and see what fields it has, what methods are available, change field values, invoke methods, and even create new objects dynamically. You start by getting a Class object using Class.forName("com.example.MyClass") or by calling getClass() on any object. From there you can call getDeclaredMethods(), getDeclaredFields(), getDeclaredConstructors() to explore the class. You can use setAccessible(true) to access private fields and methods which is normally not allowed. This is how frameworks work under the hood — Spring uses reflection to instantiate your beans and inject dependencies, Hibernate uses it to map database columns to object fields, JUnit uses it to find and run test methods. Reflection is powerful but it comes with tradeoffs — it is slower than direct method calls, it bypasses compile-time type checking so errors show up at runtime, and it can break encapsulation. The general rule is use reflection when you have no other choice like when building frameworks or plugins. For regular application code you usually do not need it directly but understanding it helps you understand what frameworks are doing.

```java
import java.lang.reflect.*;

public class ReflectionDemo {
    public static void main(String[] args) throws Exception {
        // Get Class object
        Class<?> clazz = Class.forName("java.lang.String");

        // Create instance
        Object obj = clazz.getDeclaredConstructor(String.class).newInstance("Hello");

        // Get methods
        Method[] methods = clazz.getDeclaredMethods();
        for (Method m : methods) {
            System.out.println("Method: " + m.getName());
        }

        // Access private field
        class Secret {
            private String password = "hidden";
        }

        Secret secret = new Secret();
        Field field = Secret.class.getDeclaredField("password");
        field.setAccessible(true);   // bypass private
        System.out.println("Private field: " + field.get(secret));   // hidden

        // Change private field value
        field.set(secret, "changed");
        System.out.println("Changed to: " + field.get(secret));   // changed

        // Invoke method dynamically
        Method lengthMethod = String.class.getMethod("length");
        int length = (int) lengthMethod.invoke(obj);
        System.out.println("Length: " + length);   // 5
    }
}
```
