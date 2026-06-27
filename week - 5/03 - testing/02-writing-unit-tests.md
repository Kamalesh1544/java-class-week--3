# Writing Unit Tests

## What is a Unit Test?
A unit test verifies that a single unit of code (method or class) works correctly in isolation. The goal is to test behavior, not implementation details.

## Anatomy of a Good Unit Test
```
Given (Arrange)  → Set up test data and conditions
When  (Act)      → Execute the method being tested
Then  (Assert)   → Verify the expected outcome
```

## Example: Calculator
```java
class CalculatorTest {
    
    private Calculator calculator;
    
    @BeforeEach
    void setUp() {
        calculator = new Calculator();  // Arrange
    }
    
    @Test
    void shouldReturnSum() {
        // Arrange
        int a = 5;
        int b = 3;
        
        // Act
        int result = calculator.add(a, b);
        
        // Assert
        assertEquals(8, result);
    }
    
    @Test
    void shouldReturnProduct() {
        int result = calculator.multiply(4, 5);
        assertEquals(20, result);
    }
    
    @Test
    void shouldHandleNegativeNumbers() {
        int result = calculator.add(-3, -7);
        assertEquals(-10, result);
    }
}
```

## Testing Different Scenarios
### Happy Path (Expected behavior)
```java
@Test
void shouldReturnFullName() {
    User user = new User("John", "Doe");
    assertEquals("John Doe", user.getFullName());
}
```

### Edge Cases
```java
@Test
void shouldHandleEmptyStrings() {
    User user = new User("", "");
    assertEquals("", user.getFullName());
}

@Test
void shouldHandleNullValues() {
    assertThrows(IllegalArgumentException.class,
        () -> new User(null, null));
}
```

### Boundary Conditions
```java
@Test
void shouldRejectAgeBelowZero() {
    assertThrows(IllegalArgumentException.class,
        () -> new User("John", -1));
}

@Test
void shouldRejectAgeAboveLimit() {
    assertThrows(IllegalArgumentException.class,
        () -> new User("John", 150));
}
```

## Test Naming Conventions
```java
// Pattern: shouldExpectedBehaviorWhenCondition
@Test
void shouldThrowExceptionWhenDividingByZero() { }

@Test
void shouldReturnTrueWhenUserIsActive() { }

@Test
void shouldReturnEmptyListWhenNoDataExists() { }
```

## Test Isolation
Each test must be independent:
- No shared state between tests
- No dependency on execution order
- Each test creates its own data
- Clean up after each test

```java
class UserServiceTest {
    
    private UserService service;
    private Database db;
    
    @BeforeEach
    void setUp() {
        db = new InMemoryDatabase();  // Fresh DB for each test
        service = new UserService(db);
    }
    
    @AfterEach
    void tearDown() {
        db.clear();  // Clean up
    }
}
```
