# Mockito

## What is Mockito?
Mockito is the most popular mocking framework for Java. It lets you create mock objects, stub methods, and verify interactions without writing manual mock classes.

## Maven Dependency
```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-core</artifactId>
    <version>5.8.0</version>
    <scope>test</scope>
</dependency>

<!-- For JUnit 5 integration -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-junit-jupiter</artifactId>
    <version>5.8.0</version>
    <scope>test</scope>
</dependency>
```

## Basic Usage

### Creating Mocks
```java
import org.mockito.Mockito;

// Create mock
UserRepository mockRepo = Mockito.mock(UserRepository.class);

// Or using annotation
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    UserRepository mockRepo;
    
    @InjectMocks
    UserService service;
}
```

### Stubbing Methods
```java
// When method is called, return specific value
when(mockRepo.findById(1L))
    .thenReturn(new User("John", "john@test.com"));

// When method is called, throw exception
when(mockRepo.findById(999L))
    .thenThrow(new UserNotFoundException("User not found"));

// Return different values on successive calls
when(mockRepo.findAll())
    .thenReturn(Arrays.asList(user1))
    .thenReturn(Arrays.asList(user1, user2));

// Void methods
doNothing().when(mockRepo).save(any());
doThrow(new RuntimeException()).when(mockRepo).delete(any());
```

### Using Mocks
```java
@Test
void shouldFindUser() {
    // Arrange
    when(mockRepo.findById(1L))
        .thenReturn(new User("John", "john@test.com"));
    
    // Act
    User user = service.findById(1L);
    
    // Assert
    assertEquals("John", user.getName());
}
```

## Annotations

### @Mock
Creates a mock object.
```java
@Mock
UserRepository mockRepo;
```

### @InjectMocks
Creates an instance and injects all @Mock dependencies.
```java
@Mock
UserRepository mockRepo;

@Mock
EmailService mockEmail;

@InjectMocks
UserService service;  // mockRepo and mockEmail injected
```

### @Spy
Wraps a real object.
```java
@Spy
List<String> spyList = new ArrayList<>();

@Test
void shouldSpyOnList() {
    spyList.add("first");
    verify(spyList).add("first");
    assertEquals(1, spyList.size());
}
```

## Complete Example
```java
@ExtendWith(MockitoExtension.class)
class UserServiceTest {
    
    @Mock
    private UserRepository userRepository;
    
    @Mock
    private EmailService emailService;
    
    @InjectMocks
    private UserService userService;
    
    @Test
    void shouldCreateUser() {
        // Arrange
        CreateUserRequest request = 
            new CreateUserRequest("John", "john@test.com");
        
        when(userRepository.save(any(User.class)))
            .thenReturn(new User(1L, "John", "john@test.com"));
        
        // Act
        User created = userService.createUser(request);
        
        // Assert
        assertEquals("John", created.getName());
        verify(userRepository).save(any(User.class));
        verify(emailService).send(eq("john@test.com"), anyString());
    }
    
    @Test
    void shouldThrowWhenUserNotFound() {
        // Arrange
        when(userRepository.findById(999L))
            .thenThrow(new UserNotFoundException("Not found"));
        
        // Act & Assert
        assertThrows(UserNotFoundException.class,
            () -> userService.findById(999L));
    }
}
```

## Mockito Best Practices
1. Mock interfaces not classes
2. Only stub methods you actually call
3. Verify behavior not state when possible
4. Use `any()` matchers for flexibility
5. Reset mocks between tests if needed
