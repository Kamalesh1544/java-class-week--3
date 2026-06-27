# Parameterized and Repeated Tests

## Parameterized Tests
Run the same test multiple times with different inputs. Reduces code duplication when testing similar logic with various values.

### @ParameterizedTest
```java
@ParameterizedTest
@ValueSource(ints = {1, 2, 3, 4, 5})
void shouldRejectNegativeNumbers(int age) {
    assertThrows(IllegalArgumentException.class,
        () -> new User("John", age));
}
```

### Different Source Types
```java
// Strings
@ParameterizedTest
@ValueSource(strings = {"hello", "world", "java"})
void shouldProcessStrings(String input) {
    assertNotNull(input);
}

// Doubles
@ParameterizedTest
@ValueSource(doubles = {1.0, 2.5, 3.14})
void shouldHandleDecimals(double value) {
    assertTrue(value > 0);
}

// CSV (Comma Separated Values)
@ParameterizedTest
@CsvSource({
    "1, 2, 3",      // 1 + 2 = 3
    "5, 5, 10",     // 5 + 5 = 10
    "-1, 1, 0",     // -1 + 1 = 0
    "0, 0, 0"       // 0 + 0 = 0
})
void shouldAddNumbers(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}

// CSV from file
@ParameterizedTest
@CsvFileSource(resources = "/test-data.csv", numLinesToSkip = 1)
void shouldProcessCsvData(String name, int age, boolean valid) {
    assertEquals(valid, user.isValid(name, age));
}
```

### Enum Source
```java
@ParameterizedTest
@EnumSource(Role.class)
void shouldHaveValidRoles(Role role) {
    assertNotNull(role.getName());
}
```

## Repeated Tests
Run the same test multiple times. Useful for testing flaky code or concurrency issues.

### @RepeatedTest
```java
@RepeatedTest(10)
void shouldWorkEveryTime() {
    int result = calculator.add(2, 3);
    assertEquals(5, result);
}
```

### With Custom Name
```java
@RepeatedTest(value = 5, name = "Run {currentRepetition}/{totalRepetitions}")
void shouldRepeatWithInfo() {
    // Test runs 5 times with custom name
}
```

## Combining Both
```java
@ParameterizedTest
@CsvSource({"1,2,3", "4,5,9"})
@RepeatedTest(3)
void shouldCombineBoth(int a, int b, int expected) {
    assertEquals(expected, calculator.add(a, b));
}
```

## Practical Example: Password Validation
```java
class PasswordValidatorTest {
    
    private final PasswordValidator validator = new PasswordValidator();
    
    @ParameterizedTest
    @ValueSource(strings = {
        "Abcdef1!",    // Valid
        "StrongPass1@",// Valid
        "Test@123"     // Valid
    })
    void shouldAcceptStrongPasswords(String password) {
        assertTrue(validator.isValid(password));
    }
    
    @ParameterizedTest
    @CsvSource({
        "abc,      false",  // Too short
        "ABCDEF,   false",  // No digit
        "abcdef1,  false",  // No uppercase
        "ABCDEF1,  false",  // No special char
        "Ab1!,     false"   // Too short
    })
    void shouldRejectWeakPasswords(String password, boolean expected) {
        assertEquals(expected, validator.isValid(password));
    }
    
    @RepeatedTest(5)
    void shouldHandleConcurrentValidation() {
        // Test thread safety
        validator.validate("SecurePass1!");
    }
}
```

## Maven Dependency for Parameterized Tests
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter-params</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```
