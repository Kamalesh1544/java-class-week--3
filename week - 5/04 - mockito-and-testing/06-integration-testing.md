# Integration Testing

## What is Integration Testing?
Integration testing verifies that multiple components work together correctly. Unlike unit tests that test one class in isolation, integration tests check how classes interact.

## Unit vs Integration Tests
| Aspect | Unit Test | Integration Test |
|--------|-----------|------------------|
| Scope | Single class | Multiple classes |
| Speed | Fast (milliseconds) | Slow (seconds/minutes) |
| Dependencies | Mocked | Real |
| Environment | In-memory | Database, network |
| Purpose | Verify logic | Verify interaction |

## Types of Integration Tests

### 1. Database Integration Tests
```java
@SpringBootTest
class UserRepositoryIntegrationTest {
    
    @Autowired
    private UserRepository repository;
    
    @Transactional
    @Test
    void shouldSaveAndRetrieveUser() {
        // Arrange
        User user = new User("John", "john@test.com");
        
        // Act
        repository.save(user);
        User found = repository.findById(user.getId()).orElse(null);
        
        // Assert
        assertNotNull(found);
        assertEquals("John", found.getName());
    }
}
```

### 2. API Integration Tests
```java
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class UserControllerIntegrationTest {
    
    @Autowired
    private TestRestTemplate restTemplate;
    
    @Test
    void shouldCreateUser() {
        // Arrange
        CreateUserRequest request = 
            new CreateUserRequest("John", "john@test.com");
        
        // Act
        ResponseEntity<User> response = restTemplate.postForEntity(
            "/api/users", request, User.class);
        
        // Assert
        assertEquals(HttpStatus.CREATED, response.getStatusCode());
        assertEquals("John", response.getBody().getName());
    }
    
    @Test
    void shouldReturn404WhenUserNotFound() {
        // Act
        ResponseEntity<User> response = restTemplate.getForEntity(
            "/api/users/999", User.class);
        
        // Assert
        assertEquals(HttpStatus.NOT_FOUND, response.getStatusCode());
    }
}
```

### 3. Service Integration Tests
```java
@SpringBootTest
@Transactional
class OrderServiceIntegrationTest {
    
    @Autowired
    private OrderService orderService;
    
    @Autowired
    private UserRepository userRepository;
    
    @Autowired
    private OrderRepository orderRepository;
    
    @Test
    void shouldCreateOrderWithExistingUser() {
        // Arrange
        User user = userRepository.save(new User("John", "john@test.com"));
        CreateOrderRequest request = 
            new CreateOrderRequest(user.getId(), items, total);
        
        // Act
        Order order = orderService.createOrder(request);
        
        // Assert
        assertNotNull(order);
        assertEquals(user.getId(), order.getUserId());
        assertEquals(1, orderRepository.count());
    }
}
```

## Test Containers
Run real databases in Docker containers for integration tests.

```java
@Testcontainers
class UserRepositoryContainerTest {
    
    @Container
    static PostgreSQLContainer<?> postgres = 
        new PostgreSQLContainer<>("postgres:15")
            .withDatabaseName("testdb")
            .withUsername("test")
            .withPassword("test");
    
    @DynamicPropertySource
    static void configureProperties(DynamicPropertyRegistry registry) {
        registry.add("spring.datasource.url", postgres::getJdbcUrl);
        registry.add("spring.datasource.username", postgres::getUsername);
        registry.add("spring.datasource.password", postgres::getPassword);
    }
    
    @Autowired
    private UserRepository repository;
    
    @Test
    void shouldSaveUser() {
        User user = repository.save(new User("John", "john@test.com"));
        assertNotNull(user.getId());
    }
}
```

## Best Practices

### 1. Use @Transactional
```java
@Test
@Transactional  // Rolls back after test
void shouldNotPersistData() {
    repository.save(user);
    // Data is rolled back automatically
}
```

### 2. Use Test Profiles
```java
@SpringBootTest
@ActiveProfiles("test")
class UserServiceTest {
    // Uses test configuration
}
```

### 3. Clean Up After Tests
```java
@BeforeEach
void setUp() {
    repository.deleteAll();
}

@AfterEach
void tearDown() {
    repository.deleteAll();
}
```

### 4. Use Test Data Builders
```java
class UserBuilder {
    private String name = "Default";
    private String email = "default@test.com";
    
    UserBuilder withName(String name) {
        this.name = name;
        return this;
    }
    
    UserBuilder withEmail(String email) {
        this.email = email;
        return this;
    }
    
    User build() {
        return new User(name, email);
    }
}

// Usage
User user = new UserBuilder()
    .withName("John")
    .withEmail("john@test.com")
    .build();
```

## Common Patterns
```java
// Slice tests - test one layer only
@DataJpaTest  // Tests JPA repositories only
@WebMvcTest   // Tests controllers only
@ServiceTest  // Tests service layer only

// Full integration test
@SpringBootTest
@AutoConfigureMockMvc
class FullApplicationTest {
    @Autowired
    private MockMvc mockMvc;
    
    @Test
    void shouldWorkEndToEnd() throws Exception {
        mockMvc.perform(post("/api/users")
            .contentType(MediaType.APPLICATION_JSON)
            .content("{\"name\":\"John\",\"email\":\"john@test.com\"}"))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.name").value("John"));
    }
}
```
