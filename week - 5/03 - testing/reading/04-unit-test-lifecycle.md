# Unit Test Lifecycle — The Chill Version

So JUnit has this whole lifecycle thing going on behind the scenes. When you run your tests JUnit does not just randomly execute methods. There is a specific order and specific hooks where you can run your own code. Understanding this makes your tests way more powerful.

Here is how it works. First JUnit finds all the methods annotated with @Test. Then for each test method it creates a brand new instance of your test class. This is important. Every test gets its own fresh object. So if test one changes some variable test two starts with a clean slate. That is how test isolation works.

After creating the instance JUnit runs @BeforeEach. This is where you put your setup code. Like creating objects, initializing variables, connecting to a database, whatever you need to do before every single test. Think of it as preparing your workspace before starting work.

Then JUnit runs the actual @Test method. This is where your test logic lives. You call your code, check the results, assert that everything works.

After the test finishes JUnit runs @AfterEach. This is your cleanup code. Close database connections, delete temporary files, reset variables. Whatever you created in setUp you clean up here.

Then JUnit does the whole thing again for the next test. New instance, new setUp, new test, new tearDown. Fresh start every time.

There are also @BeforeAll and @AfterAll which run once before and after ALL tests in the class. These must be static methods because they run before any instance of the class is created. Use these for expensive operations that you only want to do once like setting up a database connection pool or loading a large dataset.

The practical use case is things like database testing. In @BeforeAll you connect to the database once. In @BeforeEach you create fresh test data and a fresh repository instance. In @AfterEach you clean up the test data so the next test starts clean. In @AfterAll you close the database connection. Nice clean lifecycle.

JUnit also supports nested tests using @Nested. You can group related tests together. Like all validation tests in one nest and all persistence tests in another. Each nest can have its own @BeforeEach so you can have different setup for different groups of tests. It keeps your test code organized and readable.

The key takeaway is that JUnit manages the lifecycle for you. You just need to know which annotation to use for which situation. Setup code goes in @BeforeEach. Teardown code goes in @AfterEach. One time setup goes in @BeforeAll. One time teardown goes in @AfterAll. And your actual test logic goes in @Test methods. Simple as that.
