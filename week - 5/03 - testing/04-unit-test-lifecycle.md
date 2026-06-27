# Unit Test Lifecycle

## Test Lifecycle Overview
JUnit manages the lifecycle of tests automatically. Each test goes through a series of phases from setup to execution to cleanup.

## Lifecycle Phases
```
1. Test Discovery
   └── JUnit finds all @Test methods

2. Test Instantiation
   └── New instance of test class created

3. Before Each
   └── @BeforeEach runs

4. Test Execution
   └── @Test method runs

5. After Each
   └── @AfterEach runs

6. Repeat for next test
   └── Back to step 2 (new instance)

7. After All
   └── @AfterAll runs (once)
```

## Annotations and Their Order
```java
class LifecycleTest {
    
    @BeforeAll
    static void beforeAll() {
        // Runs ONCE before all tests
        // Must be static
        System.out.println("Before All");
    }
    
    @BeforeEach
    void setUp() {
        // Runs BEFORE EACH test
        System.out.println("Before Each");
    }
    
    @Test
    void testOne() {
        System.out.println("Test One");
    }
    
    @Test
    void testTwo() {
        System.out.println("Test Two");
    }
    
    @AfterEach
    void tearDown() {
        // Runs AFTER EACH test
        System.out.println("After Each");
    }
    
    @AfterAll
    static void afterAll() {
        // Runs ONCE after all tests
        // Must be static
        System.out.println("After All");
    }
}
```

## Execution Order
```
Before All
  Before Each
    Test One
  After Each
  Before Each
    Test Two
  After Each
After All
```

## Practical Example: Database Tests
```java
class UserRepositoryTest {
    
    private static Database db;          // Shared across all tests
    private UserRepository repository;   // Fresh for each test
    private User testUser;               // Fresh for each test
    
    @BeforeAll
    static void initDatabase() {
        // One-time setup: create database connection
        db = Database.connect("test-db");
    }
    
    @BeforeEach
    void setUp() {
        // Before each test: create fresh data
        repository = new UserRepository(db);
        testUser = new User("John", "john@test.com");
        repository.save(testUser);
    }
    
    @Test
    void shouldFindUserById() {
        User found = repository.findById(testUser.getId());
        assertNotNull(found);
        assertEquals("John", found.getName());
    }
    
    @Test
    void shouldDeleteUser() {
        repository.delete(testUser.getId());
        User found = repository.findById(testUser.getId());
        assertNull(found);
    }
    
    @AfterEach
    void cleanUp() {
        // After each test: remove test data
        repository.deleteAll();
    }
    
    @AfterAll
    static void closeDatabase() {
        // One-time cleanup: close database
        db.close();
    }
}
```

## Instance Per Test
JUnit creates a **new instance** of the test class for each test method. This ensures test isolation.

```java
class CalculatorTest {
    private int counter = 0;  // Reset for each test
    
    @Test
    void testOne() {
        counter++;
        assertEquals(1, counter);  // Always 1
    }
    
    @Test
    void testTwo() {
        counter++;
        assertEquals(1, counter);  // Always 1 (new instance)
    }
}
```

## Nested Tests
Organize related tests into groups.

```java
class UserTest {
    
    @Nested
    class Validation {
        @Test
        void shouldRejectEmptyName() { }
        
        @Test
        void shouldRejectInvalidEmail() { }
    }
    
    @Nested
    class Persistence {
        @Test
        void shouldSaveToDatabase() { }
        
        @Test
        void shouldLoadFromDatabase() { }
    }
}
```

## Exception Handling in Lifecycle
```java
class ExceptionTest {
    
    @BeforeEach
    void setUp() {
        // If this throws, test is marked ERROR
        throw new RuntimeException("Setup failed");
    }
    
    @Test
    void thisWillNotRun() {
        // Never executes if setUp fails
    }
}
```

## Summary Table
| Annotation | When | Static? | Runs |
|------------|------|---------|------|
| @BeforeAll | Before all tests | Yes | Once |
| @BeforeEach | Before each test | No | Per test |
| @Test | During test | No | Once per method |
| @AfterEach | After each test | No | Per test |
| @AfterAll | After all tests | Yes | Once |
