# Mockito — The Chill Version

Okay so writing manual mocks and stubs is tedious. You have to create entire classes just to fake a database connection. That is where Mockito comes in. It is a library that creates mock objects for you automatically. You do not have to write any mock implementation. Just tell Mockito what to mock and it generates the mock class behind the scenes using something called dynamic proxies. Sounds fancy but basically it creates a fake version of any class or interface you give it.

The basic workflow is simple. First you create a mock using Mockito.mock() or the @Mock annotation. Then you tell it what to do when certain methods are called using when().thenReturn(). Then you use the mock in your test. Finally you verify that the expected methods were called using verify(). That is the whole cycle. Create mock, stub methods, use it, verify it.

Let me show you a practical example. Say you have a UserService that depends on a UserRepository. In your test you do not want to use a real database. So you create a mock of UserRepository. You tell Mockito when someone calls findById with any ID return this specific user object. Then you pass that mock to your UserService and call the method you are testing. Your UserService thinks it is talking to a real database but it is actually talking to a mock that gives it canned responses.

The @Mock annotation is the cleanest way to create mocks. Put it on a field and Mockito creates the mock before each test. Then use @InjectMocks on the service you are testing and Mockito automatically injects all the mock dependencies into it. No manual wiring needed. It just works.

The when().thenReturn() syntax is how you stub methods. You can chain multiple return values. Like the first time someone calls findAll return a list with one user. The second time return a list with two users. You can also make methods throw exceptions using thenThrow. This is great for testing error handling. What happens when the database connection fails? You make the mock throw an exception and verify your code handles it gracefully.

Mockito also has argument matchers like any(), eq(), anyString(), anyInt(). These let you match any argument instead of specific values. Like when(mockRepo.save(any(User.class))).thenReturn(user) means whenever someone saves any user object return this user. You do not have to match the exact object. Just the type. This makes your tests more flexible and less brittle.

The verify() method is how you check that interactions happened. Like verify(mockRepo).save(user) checks that save was called once with that user. You can also check it was never called or called multiple times. This is useful for making sure side effects happen correctly. Like verifying that after creating an order the email service was called to send a confirmation.

One important thing to remember is to add the mockito-junit-jupiter dependency if you are using JUnit 5. Without it the @Mock annotation will not work. And always use @ExtendWith(MockitoExtension.class) on your test class. This tells JUnit to process Mockito annotations. Without this setup your mocks will be null and your tests will fail with confusing errors.
