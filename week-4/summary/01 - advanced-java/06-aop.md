# Aspect Oriented Programming (AOP)

AOP is a programming paradigm that lets you separate cross-cutting concerns from your main business logic. Cross-cutting concerns are things like logging, security, transactions, and caching that cut across multiple parts of your application. Normally you would have to add logging code in every single method which makes your code repetitive and messy. AOP lets you define that behavior in one place and apply it automatically wherever you need it. The core concepts are Aspect which is a module that encapsulates a cross-cutting concern, Join Point which is a point where the aspect is applied like a method call, Advice which is the action taken at the join point like before or after a method runs, and Pointcut which is an expression that defines where the advice should be applied. Spring AOP is the most commonly used implementation in Java and it works using dynamic proxies under the hood. You annotate your methods with things like @Before, @After, @Around to tell Spring what to do before or after the method executes. For example you can write a logging aspect once and have it automatically log every method in your service layer without adding a single line of logging code in those methods. This keeps your business logic clean and focused while cross-cutting concerns are handled separately. The same idea applies to transactions — you put @Transactional on a method and Spring handles starting committing or rolling back the transaction for you.

```java
import org.aspectj.lang.annotation.*;

@Aspect
@Component
public class LoggingAspect {

    @Before("execution(* com.example.service.*.*(..))")
    public void logBefore(JoinPoint joinPoint) {
        System.out.println("Calling: " + joinPoint.getSignature().getName());
    }

    @After("execution(* com.example.service.*.*(..))")
    public void logAfter(JoinPoint joinPoint) {
        System.out.println("Finished: " + joinPoint.getSignature().getName());
    }

    @Around("execution(* com.example.service.*.*(..))")
    public Object measureTime(ProceedingJoinPoint joinPoint) throws Throwable {
        long start = System.currentTimeMillis();
        Object result = joinPoint.proceed();
        long time = System.currentTimeMillis() - start;
        System.out.println(joinPoint.getSignature().getName() + " took " + time + "ms");
        return result;
    }
}

// Without AOP — messy
class UserService {
    void saveUser(String name) {
        System.out.println("[LOG] saveUser called");   // logging everywhere
        // actual logic
        System.out.println("[LOG] saveUser finished");
    }
}

// With AOP — clean
class UserService {
    void saveUser(String name) {
        // just business logic — logging handled by aspect
    }
}
```
