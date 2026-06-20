# Dynamic Proxy

Dynamic Proxy lets you create proxy objects at runtime that intercept method calls and add behavior before or after the actual method runs. It is like having a middleman that sits between you and the real object — every time you call a method, the middleman intercepts it, does something extra, and then passes it to the real object. Java provides two ways to create dynamic proxies — java.lang.reflect.Proxy for interface-based proxies and CGLIB for class-based proxies. With the Proxy approach you define an InvocationHandler that contains the logic for what happens when a method is called. You create the proxy using Proxy.newProxyInstance() passing in the class loader, the interfaces to implement, and your handler. The proxy implements the same interfaces as the real object so it can be used as a drop-in replacement. This is exactly how Spring AOP works — when you use @Transactional or @Async, Spring creates a dynamic proxy around your method that adds transaction management or asynchronous execution. The main use cases are adding logging to methods without modifying them, implementing caching, handling transactions, enforcing security checks, and remote method invocation. The beauty is your original code stays clean and untouched — all the extra behavior is added through the proxy.

```java
import java.lang.reflect.*;

// The interface
interface UserService {
    void saveUser(String name);
    String getUser(int id);
}

// The real implementation
class UserServiceImpl implements UserService {
    public void saveUser(String name) {
        System.out.println("Saving user: " + name);
    }

    public String getUser(int id) {
        return "User_" + id;
    }
}

// The invocation handler — intercepts method calls
class LoggingHandler implements InvocationHandler {
    private Object target;

    LoggingHandler(Object target) {
        this.target = target;
    }

    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("[LOG] Before: " + method.getName());
        long start = System.currentTimeMillis();

        Object result = method.invoke(target, args);   // call real method

        long time = System.currentTimeMillis() - start;
        System.out.println("[LOG] After: " + method.getName() + " took " + time + "ms");

        return result;
    }
}

// Usage
UserService real = new UserServiceImpl();

UserService proxy = (UserService) Proxy.newProxyInstance(
    UserService.class.getClassLoader(),
    new Class[]{UserService.class},
    new LoggingHandler(real)
);

proxy.saveUser("Alice");
// [LOG] Before: saveUser
// Saving user: Alice
// [LOG] After: saveUser took 2ms

String user = proxy.getUser(1);
// [LOG] Before: getUser
// [LOG] After: getUser took 1ms
```
