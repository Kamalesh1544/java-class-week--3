# Test Doubles — The Chill Version

Alright so you know how in movies they use stunt doubles for the actors during dangerous scenes? Test doubles are basically that for your code. When you are testing a class that depends on a database you do not want to actually connect to a real database. That would be slow and unreliable. Instead you use a test double. A fake object that stands in for the real thing and behaves the way you want it to.

There are five types of test doubles and each one has its own purpose. First is the Dummy. This is just a placeholder that fills a parameter slot. It is passed around but never actually used. Like when a method requires an AuditLog parameter but the method you are testing never touches the log. You just pass a Dummy object to make the compiler happy.

Second is the Stub. This is the most common one. A Stub provides predefined responses to method calls. You tell it when someone calls findById(1) return this specific user. When someone call findAll return this list. Stubs are great for setting up test data. They give you control over what your code receives so you can test different scenarios.

Third is the Spy. A Spy wraps a real object and records what happens to it. You can still call the real methods but you also get to see how many times a method was called or what arguments were passed. It is like putting a camera on your real object. The object still works normally but you get to watch what it does.

Fourth is the Mock. This is the powerful one. A Mock is like a Stub and a Spy combined. It gives you predefined responses AND it verifies that specific methods were called with specific arguments. You can say when someone calls save return this AND then verify that save was called exactly once with this exact object. Mocks are what most people use when they say they are mocking something.

Fifth is the Fake. A Fake is a working implementation that is not suitable for production. Like an InMemoryUserRepository that stores users in a HashMap instead of a real database. It actually works. You can save users and retrieve them. But it does not use a real database so it is fast and does not need any external setup. Fakes are great for integration style tests where you want realistic behavior without the overhead of real infrastructure.

The choice depends on what you need. If you just need test data use a Stub. If you need to verify interactions use a Mock. If you need realistic behavior without external dependencies use a Fake. If you need to watch a real object use a Spy. And if you just need to fill a parameter slot use a Dummy. Most of the time you will use Mocks and Stubs. The others are situational but good to know about.
