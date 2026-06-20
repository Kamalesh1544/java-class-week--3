# Defining Custom Annotations

You can create your own annotations in Java and they work just like the built-in ones. To define a custom annotation you use the @interface keyword instead of class or interface. Inside it you declare elements that look like methods but they act as attributes of the annotation. For example you might create a @Todo annotation that takes a priority and a description — like @Todo(priority = "high", description = "fix login bug"). The annotation itself does nothing, you need to write a processor that reads the annotation and does something with it. You can also add meta-annotations to control how your annotation behaves — @Retention tells Java when the annotation should be available (at compile time, in the class file, or at runtime), @Target tells Java where the annotation can be placed (on a method, a field, a class, etc.), and @Documented makes it show up in Javadoc. The most common use case is runtime retention because that lets frameworks read your annotations using reflection and make decisions based on them. Custom annotations are super useful for things like marking methods that need logging, marking fields that should be validated, creating your own ORM annotations, or defining test categories. The pattern is always the same — define the annotation, write a processor that uses reflection to find annotations and act on them.

```java
import java.lang.annotation.*;

// Define the annotation
@Retention(RetentionPolicy.RUNTIME)    // available at runtime
@Target(ElementType.METHOD)            // can only be placed on methods
@Documented
public @interface LogExecutionTime {
    String value() default "unknown";   // optional attribute with default
}

// Another example
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface ValidateNotNull {
    String message() default "Value cannot be null";
}

// Use the annotation
public class UserService {

    @LogExecutionTime(value = "fetchUser")
    public User fetchUser(String id) {
        // method logic
        return new User(id);
    }

    @ValidateNotNull(message = "Name is required")
    private String name;
}

// Process the annotation (framework would do this)
import java.lang.reflect.Method;

public class AnnotationProcessor {
    public static void process(Object obj) {
        Method[] methods = obj.getClass().getDeclaredMethods();

        for (Method method : methods) {
            if (method.isAnnotationPresent(LogExecutionTime.class)) {
                LogExecutionTime annotation = method.getAnnotation(LogExecutionTime.class);
                System.out.println("Method: " + method.getName());
                System.out.println("Log value: " + annotation.value());
            }
        }
    }

    public static void main(String[] args) {
        process(new UserService());
    }
}
```
