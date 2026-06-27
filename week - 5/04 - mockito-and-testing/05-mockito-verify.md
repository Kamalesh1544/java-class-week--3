# Mockito Verify

## Why Verify?
Verification checks that specific methods were called with specific arguments. It ensures your code interacts with dependencies correctly.

## Basic Verification

### verify() Method
```java
@Test
void shouldCallSaveOnce() {
    // Arrange
    when(mockRepo.save(any())).thenReturn(user);
    
    // Act
    service.createUser("John", "john@test.com");
    
    // Assert
    verify(mockRepo).save(any(User.class));  // Called once
}
```

### Times
```java
// Exactly once (default)
verify(mockRepo).save(user);

// Exactly N times
verify(mockRepo, times(3)).save(user);

// Never called
verify(mockRepo, never()).delete(user);

// At least once
verify(mockRepo, atLeastOnce()).save(user);

// At least N times
verify(mockRepo, atLeast(2)).save(user);

// At most N times
verify(mockRepo, atMost(5)).save(user);
```

### Argument Verification
```java
@Test
void shouldPassCorrectArguments() {
    User user = new User("John", "john@test.com");
    
    service.createUser(user);
    
    // Verify exact arguments
    verify(mockRepo).save(eq(user));
    
    // Verify with argument matchers
    verify(mockEmail).send(
        eq("john@test.com"),
        argThat(msg -> msg.contains("Welcome"))
    );
}
```

## Argument Captors
Capture arguments passed to methods for detailed assertions.

```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository mockRepo;
    
    @Mock
    private EmailService mockEmail;
    
    @InjectMocks
    private UserService service;
    
    @Captor
    private ArgumentCaptor<User> userCaptor;
    
    @Captor
    private ArgumentCaptor<String> emailCaptor;
    
    @Test
    void shouldCaptureSavedUser() {
        // Act
        service.createUser("John", "john@test.com");
        
        // Capture argument
        verify(mockRepo).save(userCaptor.capture());
        
        // Assert captured value
        User capturedUser = userCaptor.getValue();
        assertEquals("John", capturedUser.getName());
        assertEquals("john@test.com", capturedUser.getEmail());
    }
    
    @Test
    void shouldCaptureEmailContent() {
        // Act
        service.createUser("John", "john@test.com");
        
        // Capture
        verify(mockEmail).send(
            eq("john@test.com"),
            emailCaptor.capture()
        );
        
        // Assert
        String capturedEmail = emailCaptor.getValue();
        assertTrue(capturedEmail.contains("Welcome"));
    }
}
```

## Verification Modes
```java
// Verify order
InOrder inOrder = inOrder(mockRepo, mockEmail);
inOrder.verify(mockRepo).save(user);
inOrder.verify(mockEmail).send(any(), any());

// Verify no more interactions
verifyNoMoreInteractions(mockRepo);
verifyNoInteractions(mockEmail);

// Verify at least this many interactions
verify(mockRepo, atLeast(1)).findAll();
verify(mockRepo, atMost(10)).findAll();
```

## Practical Examples

### Example 1: Service Layer
```java
@Test
void shouldProcessOrderAndNotify() {
    // Arrange
    Order order = new Order(1L, items, total);
    when(mockPayment.process(any())).thenReturn(success);
    
    // Act
    service.processOrder(order);
    
    // Assert
    verify(mockPayment).process(eq(order));
    verify(mockInventory).reserve(order.getItems());
    verify(mockEmail).send(
        eq(order.getCustomerEmail()),
        argThat(msg -> msg.contains("Order confirmed"))
    );
    verify(mockRepo).save(order);
}
```

### Example 2: Error Handling
```java
@Test
void shouldNotSendEmailOnPaymentFailure() {
    // Arrange
    when(mockPayment.process(any()))
        .thenThrow(new PaymentException("Failed"));
    
    // Act
    assertThrows(PaymentException.class,
        () -> service.processOrder(order));
    
    // Assert
    verify(mockPayment).process(eq(order));
    verify(mockEmail, never()).send(any(), any());
    verify(mockRepo, never()).save(any());
}
```

### Example 3: Multiple Calls
```java
@Test
void shouldHandleRetries() {
    // Arrange
    when(mockService.process(any()))
        .thenThrow(new TimeoutException())
        .thenReturn(success);
    
    // Act
    service.executeWithRetry(request);
    
    // Assert
    verify(mockService, times(2)).process(eq(request));
}
```

## Common Patterns
```java
// Verify method was NOT called
verify(mockRepo, never()).delete(user);

// Verify with timeout
verify(mockRepo, timeout(1000)).save(user);

// Verify in order
verify(mockRepo).save(user);
verify(mockEmail).send(any(), any());
verify(mockLogger).log(argThat(msg -> msg.contains("Created")));
```

## Troubleshooting
```java
// Problem: Verification fails with "Wanted but not invoked"
verify(mockRepo).save(expectedUser);
// Fix: Check if method is actually called

// Problem: "Actually, there were zero interactions"
verifyNoInteractions(mockRepo);
// Fix: Check if you're calling the right mock

// Problem: Argument mismatch
verify(mockRepo).save(any(User.class));
// Fix: Use argument matchers instead of exact values
```
