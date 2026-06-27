# Argument Matchers

## What are Argument Matchers?
Argument matchers let you match method arguments flexibly. Instead of matching exact values you can match patterns, types, or any value.

## Basic Matchers

### any()
Matches any argument of the specified type.
```java
// Any User object
when(mockRepo.save(any(User.class))).thenReturn(user);

// Any String
when(mockEmail.send(anyString(), anyString())).thenReturn(true);

// Any value of any type
when(mockService.process(any())).thenReturn(result);
```

### eq()
Matches exact value (optional - default behavior).
```java
// These are equivalent
when(mockRepo.findById(eq(1L))).thenReturn(user);
when(mockRepo.findById(1L)).thenReturn(user);
```

### String Matchers
```java
// Any string
when(mockValidator.validate(anyString())).thenReturn(true);

// Starts with
when(mockProcessor.process(argThat(s -> s.startsWith("PRE_"))))
    .thenReturn(processed);

// Contains
when(mockValidator.validate(argThat(s -> s.contains("error"))))
    .thenReturn(false);

// Ends with
when(mockFormatter.format(argThat(s -> s.endsWith(".txt"))))
    .thenReturn(formatted);

// Matches regex
when(mockValidator.validate(argThat(s -> s.matches("\\d{3}-\\d{4}"))))
    .thenReturn(true);

// Empty string
when(mockValidator.validate(eq(""))).thenReturn(false);

// Null
when(mockProcessor.process(isNull())).thenReturn(defaultResult);
```

### Number Matchers
```java
// Any integer
when(mockCalc.calculate(anyInt())).thenReturn(42);

// Any double
when(mockCalc.calculate(anyDouble())).thenReturn(3.14);

// Greater than
when(mockCalc.calculate(argThat(n -> n > 10))).thenReturn(100);

// Less than
when(mockCalc.calculate(argThat(n -> n < 0))).thenThrow(exception);

// Between
when(mockCalc.calculate(argThat(n -> n >= 1 && n <= 100)))
    .thenReturn(valid);
```

### Collection Matchers
```java
// Any list
when(mockService.processList(anyList())).thenReturn(result);

// Empty list
when(mockService.processList(eq(Collections.emptyList())))
    .thenReturn(emptyResult);

// List with specific size
when(mockService.processList(argThat(l -> l.size() == 3)))
    .thenReturn(threeItems);

// List containing specific element
when(mockService.processList(argThat(l -> l.contains("target"))))
    .thenReturn(found);
```

## Custom Matchers
```java
// Create custom matcher
class UserMatcher extends ArgumentMatcher<User> {
    private final String expectedName;
    
    UserMatcher(String expectedName) {
        this.expectedName = expectedName;
    }
    
    @Override
    public boolean matches(User user) {
        return user.getName().equals(expectedName);
    }
    
    @Override
    public String toString() {
        return "User with name: " + expectedName;
    }
}

// Use custom matcher
when(mockRepo.find(argThat(new UserMatcher("John"))))
    .thenReturn(john);

// Static helper method
class UserMatchers {
    static UserMatcher userWithName(String name) {
        return new UserMatcher(name);
    }
}

// Usage
when(mockRepo.find(argThat(userWithName("John"))))
    .thenReturn(john);
```

## allOf() and anyOf()
```java
// All conditions must match
when(mockValidator.validate(argThat(
    allOf(
        s -> s.length() > 8,
        s -> s.contains("@"),
        s -> s.matches(".*\\d.*")
    )
))).thenReturn(true);

// Any condition can match
when(mockValidator.validate(argThat(
    anyOf(
        s -> s.startsWith("ADMIN_"),
        s -> s.startsWith("USER_")
    )
))).thenReturn(true);
```

## Practical Examples
```java
@Test
void shouldMatchArguments() {
    // Exact match
    when(mockRepo.findById(1L)).thenReturn(user1);
    
    // Any value
    when(mockRepo.save(any(User.class))).thenReturn(saved);
    
    // Conditional match
    when(mockEmail.send(
        argThat(email -> email.endsWith("@company.com")),
        anyString()
    )).thenReturn(true);
    
    // Complex condition
    when(mockService.process(
        argThat(order -> 
            order.getTotal() > 100 && 
            order.isPremium()
        )
    )).thenReturn(premiumResult);
}
```

## Reset Matchers
```java
// Clear all stubbings
clearInvocations(mockRepo);

// Reset mock to default state
Mockito.reset(mockRepo);
```
