# JUnit — The Chill Version

Okay so you have been writing code and you think it works but how do you really know? You run it manually, test a few things, and hope for the best. But what if I told you there is a way to automatically test your code every time you change something? That is exactly what JUnit does. It is basically a testing framework that lets you write small programs that test your actual programs. You write a test, run it, and if it passes your code is good. If it fails you know exactly what broke.

Think of it like this. You built a house and now you want to make sure everything works. You could walk around manually checking every door, every window, every wire. Or you could hire an inspector who has a checklist and tests everything systematically. JUnit is that inspector for your code. You give it a checklist of things to verify and it runs through them automatically.

JUnit 5 is the latest version and it is the one you should use. It is modular which means you only pull in what you need. The core part is called JUnit Jupiter and it has all the basics like @Test for marking test methods, @BeforeEach for setup code that runs before every test, and @AfterEach for cleanup code that runs after every test. There is also @BeforeAll and @AfterAll for code that runs once before or after all tests.

The assertions are what make tests actually useful. assertEquals checks if two things are equal. assertTrue and assertFalse check conditions. assertNull and assertNotNull check for null values. assertThrows checks if a method throws the right exception. These are your tools for saying hey this is what I expect and if it is not right the test fails.

To use JUnit you add a dependency to your pom.xml. Just grab the junit-jupiter artifact from Maven central and set the scope to test. Then mvn test will find and run all your test classes. Any class with methods annotated with @Test is a test class and any method with @Test is a test. Pretty straightforward once you get the hang of it.
