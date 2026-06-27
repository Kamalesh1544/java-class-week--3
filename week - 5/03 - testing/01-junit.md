# JUnit

## What is JUnit?
JUnit is a **unit testing framework** for Java. It is the most widely used testing library in the Java ecosystem. JUnit allows you to write and run repeatable automated tests to verify that your code works correctly.

## JUnit Versions
| Version | Year | Key Features |
|---------|------|--------------|
| JUnit 4 | 2006 | Annotations, @Test, @Before, @After |
| JUnit 5 | 2017 | Modular, extensions, parameterized tests |

## JUnit 5 Architecture
```
JUnit 5 Platform
├── JUnit Jupiter (Core)
│   ├── @Test
│   ├── @BeforeEach, @AfterEach
│   ├── @BeforeAll, @AfterAll
│   ├── Assertions
│   └── Extensions
├── JUnit Vintage (Legacy)
│   └── Runs JUnit 3/4 tests
└── JUnit Platform Launcher
    └── Runs tests via IDE or build tool
```

## Basic Example
```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

class CalculatorTest {
    
    @Test
    void shouldAddTwoNumbers() {
        Calculator calc = new Calculator();
        int result = calc.add(2, 3);
        assertEquals(5, result);
    }
    
    @Test
    void shouldDivideByZero() {
        Calculator calc = new Calculator();
        assertThrows(ArithmeticException.class, 
            () -> calc.divide(10, 0));
    }
}
```

## Common Annotations
| Annotation | Description |
|------------|-------------|
| @Test | Marks a method as a test |
| @BeforeEach | Runs before each test |
| @AfterEach | Runs after each test |
| @BeforeAll | Runs once before all tests |
| @AfterAll | Runs once after all tests |
| @Disabled | Skips the test |
| @DisplayName | Custom test name |

## Common Assertions
```java
// Equality
assertEquals(expected, actual);
assertEquals(expected, actual, "Custom message");

// Conditions
assertTrue(condition);
assertFalse(condition);
assertNull(object);
assertNotNull(object);

// Arrays
assertArrayEquals(expectedArray, actualArray);

// Exceptions
assertThrows(ExceptionClass.class, () -> { ... });

// Grouped
assertAll(
    () -> assertEquals(5, result),
    () -> assertTrue(result > 0)
);
```

## Maven Dependency
```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```
