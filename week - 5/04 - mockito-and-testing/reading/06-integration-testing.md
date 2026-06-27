# Integration Testing — The Chill Version

Alright so unit tests are great for testing individual classes in isolation. But what happens when you need to test how multiple classes work together? Like does your OrderService actually save orders to the database? Does your REST controller actually return the right HTTP status? That is where integration testing comes in. Integration tests verify that different parts of your system work together correctly.

The main difference between unit tests and integration tests is what you use for dependencies. In unit tests you mock everything. In integration tests you use real implementations. You connect to a real database. You make real HTTP calls. You use real message queues. The whole point is to test the actual integration between components not to fake it.

Spring Boot makes integration testing pretty easy. If you want to test your repository layer you use @DataJpaTest. It sets up an in-memory database and scans for repository classes. If you want to test your controller layer you use @WebMvcTest. It sets up MockMvc and scans for controller classes. If you want to test everything together you use @SpringBootTest. It loads the full application context.

The @Transactional annotation is your best friend in integration tests. Put it on your test class or individual test methods and Spring automatically rolls back all changes after each test. This means your tests do not pollute the database. You can run the same test a hundred times and the database stays clean. No manual cleanup needed.

Test Containers are a game changer for integration testing. Instead of using an in-memory database like H2 which might behave differently than your real database Test Containers spins up a real database in Docker. You get a real PostgreSQL or MySQL running in a container. Your tests run against the same database you use in production. This catches issues that in-memory databases would miss. The setup is a bit more involved but the confidence you get is worth it.

REST API testing with MockMvc is straightforward. You use mockMvc.perform() to make HTTP requests and then chain expectations. Like perform(post("/api/users").contentType(JSON).content(requestBody)).andExpect(status().isCreated()).andExpect(jsonPath("$.name").value("John")). This tests the entire request lifecycle from controller to service to repository. One test covers multiple layers.

The key thing about integration tests is that they are slower than unit tests. Connecting to a real database takes time. Making HTTP calls takes time. So you do not want to write hundreds of integration tests. Focus on the critical paths. The happy path through your application. The main business workflows. Leave the edge cases and boundary conditions to fast unit tests. Integration tests verify that the plumbing works. Unit tests verify that the logic is correct.

A good testing strategy uses both. Write fast unit tests for your business logic. Write slower integration tests for your critical workflows. Run unit tests on every commit. Run integration tests on a schedule or before deployment. The combination gives you confidence that your code works both in isolation and when connected to real dependencies.
