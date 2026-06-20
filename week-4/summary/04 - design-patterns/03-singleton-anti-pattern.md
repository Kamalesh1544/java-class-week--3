# Edge Case: Singleton Anti-pattern

So Singleton is one of those patterns that sounds great in theory but can cause real problems if you overuse it. The anti-pattern part is when developers start using Singleton for everything just because it is easy, and suddenly your code is full of global state that is hard to test, hard to debug, and tightly coupled. The main issue is that Singleton introduces hidden dependencies — when class A uses a Singleton internally, anyone reading the code has no idea that A depends on that Singleton unless they dig into the implementation. This makes your code harder to understand and test because you cannot easily swap out the Singleton for a mock during unit tests. Another problem is that Singleton state persists across the entire application lifetime, so if something goes wrong with that state it affects everything and tracking down the bug becomes a nightmare. It also makes your code harder to parallelize because shared mutable state needs synchronization. The real anti-pattern is not Singleton itself but using it as a shortcut to avoid thinking about proper dependency management. If you find yourself using Singleton everywhere, that is usually a sign that your design needs rethinking. In most cases you are better off passing dependencies through constructors or using a dependency injection framework which gives you the same benefits without the global state headache.

```java
// Anti-pattern — Singleton everywhere
class UserManager {
    private static UserManager instance = new UserManager();
    private UserManager() {}
    public static UserManager getInstance() { return instance; }

    void sendEmail(String user) {
        // depends on EmailService singleton
        EmailService.getInstance().send(user);
    }
}

// Problem: How do you test UserManager?
// You cannot mock EmailService because it is hardcoded inside.

// Better approach — pass dependencies through constructor
class UserManager {
    private final EmailService emailService;

    UserManager(EmailService emailService) {
        this.emailService = emailService;   // inject, easy to mock in tests
    }

    void sendEmail(String user) {
        emailService.send(user);
    }
}
```
