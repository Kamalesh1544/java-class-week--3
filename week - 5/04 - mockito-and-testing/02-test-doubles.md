# Test Doubles

## What are Test Doubles?
Test doubles are objects that stand in for real dependencies during testing. They let you control behavior and verify interactions without using real implementations.

## Types of Test Doubles

### 1. Dummy
Passed around but never actually used. Just fills a parameter.

```java
class UserValidator {
    boolean isValid(User user, AuditLog log) {
        // log is a Dummy - never used in this method
        return user.getName() != null;
    }
}

@Test
void shouldValidateUser() {
    AuditLog dummyLog = new AuditLog();  // Never used
    UserValidator validator = new UserValidator();
    assertTrue(validator.isValid(user, dummyLog));
}
```

### 2. Stub
Provides predefined responses to method calls.

```java
class UserRepositoryStub implements UserRepository {
    @Override
    public User findById(Long id) {
        return new User("John", "john@test.com");  // Always returns same user
    }
    
    @Override
    public List<User> findAll() {
        return Arrays.asList(
            new User("Alice", "alice@test.com"),
            new User("Bob", "bob@test.com")
        );
    }
}

@Test
void shouldFindUser() {
    UserRepository stub = new UserRepositoryStub();
    UserService service = new UserService(stub);
    
    User user = service.findById(1L);
    assertEquals("John", user.getName());
}
```

### 3. Spy
Wraps a real object and records calls. You can still call real methods.

```java
class UserSpy implements UserRepository {
    private int findByIdCount = 0;
    private final UserRepository real;
    
    UserSpy(UserRepository real) {
        this.real = real;
    }
    
    @Override
    public User findById(Long id) {
        findByIdCount++;  // Record the call
        return real.findById(id);  // Call real method
    }
    
    int getFindByIdCount() {
        return findByIdCount;
    }
}

@Test
void shouldCallFindById() {
    UserRepository real = new DatabaseUserRepository();
    UserSpy spy = new UserSpy(real);
    
    UserService service = new UserService(spy);
    service.findById(1L);
    
    assertEquals(1, spy.getFindByIdCount());
}
```

### 4. Mock
Pre-programmed responses AND verifies interactions.

```java
class UserRepositoryMock implements UserRepository {
    private User toReturn;
    private int saveCount = 0;
    
    void thenReturn(User user) {
        this.toReturn = user;
    }
    
    @Override
    public User findById(Long id) {
        return toReturn;
    }
    
    @Override
    public void save(User user) {
        saveCount++;
    }
    
    void verifySaveCalled(int times) {
        assertEquals(times, saveCount);
    }
}

@Test
void shouldSaveUser() {
    UserRepositoryMock mock = new UserRepositoryMock();
    mock.thenReturn(new User("John", "john@test.com"));
    
    UserService service = new UserService(mock);
    service.createUser("John", "john@test.com");
    
    mock.verifySaveCalled(1);
}
```

### 5. Fake
Working implementation but not suitable for production.

```java
class InMemoryUserRepository implements UserRepository {
    private final Map<Long, User> users = new HashMap<>();
    private long nextId = 1L;
    
    @Override
    public void save(User user) {
        users.put(nextId++, user);
    }
    
    @Override
    public User findById(Long id) {
        return users.get(id);
    }
    
    @Override
    public List<User> findAll() {
        return new ArrayList<>(users.values());
    }
}

@Test
void shouldPersistUser() {
    UserRepository fake = new InMemoryUserRepository();
    UserService service = new UserService(fake);
    
    service.createUser("John", "john@test.com");
    
    List<User> users = fake.findAll();
    assertEquals(1, users.size());
}
```

## Comparison Table
| Type | Behavior | Verification | Use Case |
|------|----------|--------------|----------|
| Dummy | None | None | Fill parameters |
| Stub | Predefined | None | Provide test data |
| Spy | Delegates to real | Records calls | Verify interactions |
| Mock | Pre-programmed | Full verification | Complex interactions |
| Fake | Working impl | State-based |替代外部依赖 |

## When to Use What
```
Simple test data → Stub
Need to verify calls → Mock
State-based testing → Fake
Wrap real object → Spy
Just fill parameter → Dummy
```
