# Parameterized and Repeated Tests — The Chill Version

Okay so imagine you have a method that validates passwords. It checks if the password is at least 8 characters, has an uppercase letter, has a digit, and has a special character. Now you want to test this method with like fifty different passwords. Some valid some invalid. Without parameterized tests you would have to write fifty separate test methods. That is a lot of code duplication and it is super annoying to maintain.

Parameterized tests solve this problem. Instead of writing fifty test methods you write one test method and give it fifty sets of inputs. JUnit runs the same test fifty times with different data. You just annotate your test with @ParameterizedTest and then tell JUnit where to get the data from.

The simplest way is using @ValueSource. You give it a list of values and it runs the test once for each value. Like if you are testing a method that checks if a number is positive you give it a bunch of positive numbers and it runs the test for each one. One annotation fifty tests. Beautiful.

Then there is @CsvSource which is even more powerful. CSV stands for comma separated values. You give it rows of data where each row has inputs and the expected output. Like "1, 2, 3" meaning input 1 and 2 should give output 3. Then "5, 5, 10" meaning 5 and 5 should give 10. JUnit parses each row and runs the test with those values. Super clean way to test multiple scenarios.

You can even load test data from a file using @CsvFileSource. Create a CSV file with your test cases and point the annotation to that file. This is great when you have hundreds of test cases and you do not want them cluttering up your Java code.

Repeated tests are simpler. Sometimes you just want to run the same test multiple times. Maybe you are testing something that involves randomness or concurrency and you want to make sure it works every time. Use @RepeatedTest and give it a number. Like @RepeatedTest(10) runs the test ten times. It is useful for catching flaky tests that sometimes pass and sometimes fail.

You can even combine both. Run a parameterized test multiple times. Like test a function with three different inputs and run each one five times. That gives you fifteen test executions from a single test method. Efficiency at its finest.
