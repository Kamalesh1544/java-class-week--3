# Tests in Maven — The Chill Version

Okay so you have your tests written and now you want to actually run them as part of your build. Maven handles this for you through something called the Surefire plugin. When you run mvn test Maven compiles your test code and then executes all the test classes it can find. Any class that ends with Test or starts with Test is automatically picked up and run.

The standard Maven project structure puts your main code in src/main/java and your test code in src/test/java. Maven knows to look for tests in that test folder. So you create your test class in the same package as your main class but under src/test instead of src/main. Maven finds it, compiles it, and runs it.

By default Maven runs tests during the test phase of the build lifecycle. So when you run mvn package it first compiles your code, then runs your tests, then packages everything into a JAR. If any test fails the build stops. That is the whole point. You do not want to package broken code.

Now sometimes you want to skip tests. Maybe you are in a hurry and you know your code works. You can run mvn package -DskipTests. This skips the test phase but still compiles your tests. Or you can run mvn package -Dmaven.test.skip=true which skips both compilation and execution of tests. Use the first one if you just want to build fast. Use the second one if you really do not want to deal with tests at all.

You can run specific tests by passing the test parameter. Like mvn test -Dtest=CalculatorTest runs only that one test class. Or mvn test -Dtest=CalculatorTest#shouldAddNumbers runs only that one method. Super useful when you are debugging a specific test.

The test reports go into the target/surefire-reports folder. There are XML files with detailed results and a summary. If you are using an IDE like IntelliJ it shows you the results directly in the test runner panel. You can see which tests passed, which failed, and why.

For integration tests there is the Failsafe plugin. Integration tests usually need more setup like a running database or external services. You put these tests in a class that ends with IT like UserServiceIT. Then you run mvn verify which compiles, runs unit tests with Surefire, and then runs integration tests with Failsafe. The difference is Surefire is for fast unit tests and Failsafe is for slower integration tests.

JaCoCo is another plugin you should set up. It gives you code coverage reports. You add it to your pom.xml and run mvn test jacoco:report. It generates an HTML report showing which lines of code were covered by your tests. Very handy for finding gaps in your test coverage.

The key commands to remember are mvn test for running tests, mvn package -DskipTests for skipping tests, and mvn test -Dtest=ClassName for running specific tests. That covers ninety percent of what you need day to day.
