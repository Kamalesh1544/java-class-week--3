# Code Coverage

## What is Code Coverage?
Code coverage measures how much of your source code is executed during testing. It helps identify untested parts of your application.

## Coverage Metrics
| Metric | Description |
|--------|-------------|
| Line Coverage | % of lines executed |
| Branch Coverage | % of branches (if/else) taken |
| Method Coverage | % of methods called |
| Class Coverage | % of classes instantiated |

## Coverage Levels
```
0%   → No tests
20%  → Basic tests
50%  → Moderate coverage
80%  → Good coverage
90%+ → Excellent coverage
100% → Perfect (but unrealistic)
```

## Example Coverage
```java
public int calculateDiscount(double price, boolean isMember) {
    int discount = 0;
    
    if (price > 100) {          // Branch 1
        discount = 10;
    } else if (price > 50) {    // Branch 2
        discount = 5;
    }
    
    if (isMember) {             // Branch 3
        discount += 5;
    }
    
    return discount;
}

// Tests covering different paths
@Test
void shouldGive10PercentForHighPrice() {
    assertEquals(10, calculateDiscount(150, false));
}

@Test
void shouldGive5PercentForMediumPrice() {
    assertEquals(5, calculateDiscount(75, false));
}

@Test
void shouldAddBonusForMember() {
    assertEquals(15, calculateDiscount(150, true));
}

// Coverage result:
// Line coverage: 100% (all lines executed)
// Branch coverage: 100% (all branches taken)
```

## JaCoCo (Java Code Coverage)
Most popular Java coverage tool. Integrates with Maven and Gradle.

### Maven Configuration
```xml
<plugin>
    <groupId>org.jacoco</groupId>
    <artifactId>jacoco-maven-plugin</artifactId>
    <version>0.8.11</version>
    <executions>
        <execution>
            <goals>
                <goal>prepare-agent</goal>
            </goals>
        </execution>
        <execution>
            <id>report</id>
            <phase>test</phase>
            <goals>
                <goal>report</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

### Generate Report
```bash
mvn clean test jacoco:report
```

### View Report
```
target/site/jacoco/index.html
```

## Coverage Report Example
```
Package: com.example.service
Instructions: 85% covered
Branches: 78% covered
Lines: 90% covered

Class: UserService.java
createUser()    → 100% covered
deleteUser()    → 100% covered
updateUser()    → 85% covered (missing null check)
findById()      → 100% covered
findAll()       → 0% not tested ← NEEDS TESTS
```

## What to Measure
```
High Priority:
├── Business logic
├── Edge cases
├── Error handling
└── Critical paths

Medium Priority:
├── Data access
├── Service layer
└── Utility methods

Low Priority:
├── Getters/Setters
├── DTOs
└── Configuration
```

## Coverage Anti-Patterns

### 1. 100% Coverage ≠ Bug Free
```java
// 100% coverage but still buggy
@Test
void test() {
    assertEquals(5, calculate(2, 3));
    // Missing: negative numbers, zero, overflow
}
```

### 2. Trivial Tests
```java
// 100% line coverage but meaningless
@Test
void getterShouldReturnField() {
    User user = new User("John");
    assertEquals("John", user.getName());  // Just testing getter
}
```

### 3. Ignoring Edge Cases
```java
// Only testing happy path
@Test
void shouldProcessOrder() {
    assertEquals("done", processOrder(validOrder));
    // Missing: null order, invalid order, empty order
}
```

## Practical Example
```java
class OrderServiceTest {
    
    private OrderService service;
    
    @BeforeEach
    void setUp() {
        service = new OrderService();
    }
    
    @Test
    void shouldCalculateTotalWithTax() {
        Order order = new Order(100.0, false);
        assertEquals(108.0, service.calculateTotal(order));
    }
    
    @Test
    void shouldApplyDiscountForMember() {
        Order order = new Order(100.0, true);
        assertEquals(97.2, service.calculateTotal(order));
    }
    
    @Test
    void shouldReturnZeroForEmptyOrder() {
        Order order = new Order(0.0, false);
        assertEquals(0.0, service.calculateTotal(order));
    }
    
    @Test
    void shouldThrowForNegativeAmount() {
        assertThrows(IllegalArgumentException.class,
            () -> service.calculateTotal(new Order(-10.0, false)));
    }
    
    // Coverage: 100% line, 100% branch
}
```
