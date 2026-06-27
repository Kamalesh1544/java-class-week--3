# Writing Good Unit Tests

## Characteristics of Good Tests
Good unit tests follow the **FIRST** principles:

| Principle | Description |
|-----------|-------------|
| **F**ast | Tests should run quickly |
| **I**ndependent | Tests should not depend on each other |
| **R**epeatable | Tests should produce same result every time |
| **S**elf-validating | Tests should pass or fail, no manual checks |
| **T**imely | Tests should be written with or before code |

## AAA Pattern (Arrange, Act, Assert)
Every test should follow this structure:

```java
@Test
void shouldCalculateTotalPrice() {
    // Arrange - Set up test data
    ShoppingCart cart = new ShoppingCart();
    cart.addItem(new Item("Laptop", 999.99));
    cart.addItem(new Item("Mouse", 29.99));
    
    // Act - Execute the method
    double total = cart.calculateTotal();
    
    // Assert - Verify the result
    assertEquals(1029.98, total, 0.01);
}
```

## Test Naming
Good names describe what is being tested:

```java
// Bad
@Test
void test1() { }

@Test
void calculator() { }

// Good
@Test
void shouldReturnSumWhenAddingTwoNumbers() { }

@Test
void shouldThrowExceptionWhenDividingByZero() { }

@Test
void shouldReturnEmptyListWhenNoItemsExist() { }
```

## One Assertion Per Test
Each test should verify one behavior:

```java
// Bad - Multiple behaviors
@Test
void testUser() {
    User user = new User("John");
    assertEquals("John", user.getName());     // Name
    assertTrue(user.isActive());               // Status
    assertNotNull(user.getCreatedAt());        // Timestamp
}

// Good - One behavior per test
@Test
void shouldStoreUserName() {
    User user = new User("John");
    assertEquals("John", user.getName());
}

@Test
void shouldBeActiveByDefault() {
    User user = new User("John");
    assertTrue(user.isActive());
}

@Test
void shouldSetCreatedAtOnCreation() {
    User user = new User("John");
    assertNotNull(user.getCreatedAt());
}
```

## Test Edge Cases
```java
class StringHelperTest {
    
    private final StringHelper helper = new StringHelper();
    
    @Test
    void shouldReturnLengthOfString() {
        assertEquals(5, helper.getLength("hello"));
    }
    
    // Edge cases
    @Test
    void shouldReturnZeroForEmptyString() {
        assertEquals(0, helper.getLength(""));
    }
    
    @Test
    void shouldHandleSingleCharacter() {
        assertEquals(1, helper.getLength("a"));
    }
    
    @Test
    void shouldHandleNullInput() {
        assertEquals(0, helper.getLength(null));
    }
    
    @Test
    void shouldHandleVeryLongString() {
        String longString = "a".repeat(10000);
        assertEquals(10000, helper.getLength(longString));
    }
}
```

## Avoid These Anti-Patterns

### 1. Testing Implementation Details
```java
// Bad - Tests implementation
@Test
void shouldCallInternalMethod() {
    Service service = new Service();
    // Don't verify internal method calls
    verify(service, times(1)).internalMethod();
}

// Good - Tests behavior
@Test
void shouldReturnCorrectResult() {
    Service service = new Service();
    assertEquals("expected", service.process("input"));
}
```

### 2. Magic Numbers
```java
// Bad
@Test
void test() {
    assertEquals(86400, user.getSessionTimeout());
}

// Good
@Test
void shouldHaveDefaultSessionTimeout() {
    int ONE_DAY_IN_SECONDS = 86400;
    assertEquals(ONE_DAY_IN_SECONDS, user.getSessionTimeout());
}
```

### 3. Overly Complex Tests
```java
// Bad - Too complex
@Test
void testComplexScenario() {
    // 50 lines of setup
    // Multiple assertions
    // Hard to understand
}

// Good - Split into smaller tests
@Test
void shouldHandleBasicScenario() {
    // Simple, focused test
}
```

## Test Data Builders
```java
class UserBuilder {
    private String name = "Default";
    private String email = "default@test.com";
    private boolean active = true;
    
    public UserBuilder withName(String name) {
        this.name = name;
        return this;
    }
    
    public UserBuilder withEmail(String email) {
        this.email = email;
        return this;
    }
    
    public UserBuilder inactive() {
        this.active = false;
        return this;
    }
    
    public User build() {
        return new User(name, email, active);
    }
}

// Usage
@Test
void shouldProcessActiveUser() {
    User user = new UserBuilder()
        .withName("John")
        .withEmail("john@test.com")
        .build();
    
    assertTrue(service.isActive(user));
}
```
