# Writing Unit Tests — The Chill Version

Alright so you know what JUnit is but how do you actually write good tests? The trick is to follow a simple pattern. Think of it as Given When Then or more technically Arrange Act Assert. First you set up your test data and conditions, then you run the thing you are testing, then you check if the result is what you expected. That is it. Every test follows this pattern.

Let me give you an example. Say you have a Calculator class with an add method. Your test would first create a Calculator instance, then call add with some numbers, then check if the result equals what you expected. Arrange create the calculator. Act call add(2, 3). Assert check that the result is 5. Simple right?

Now here is the important part. You need to test more than just the happy path. The happy path is when everything works perfectly. But what about edge cases? What happens when you pass zero? Negative numbers? Really large numbers? What if someone passes null? A good test covers all these scenarios. You test the normal case, the edge cases, and the error cases.

Another thing that trips people up is test naming. Do not just name your test test1 or testAdd. Name it something that describes what it is checking. Like shouldReturnSumWhenAddingTwoNumbers or shouldThrowExceptionWhenDividingByZero. When someone reads your test name they should immediately know what the test is verifying.

And remember one test should only check one thing. Do not put multiple assertions in a single test trying to verify five different behaviors. If the test fails you want to know exactly which behavior broke. So split your tests. One test for the name, one test for the email, one test for the status. Each test focused on one specific behavior.

Test isolation is another big deal. Each test should be independent. It should not depend on any other test running before it or after it. It should create its own data and clean up after itself. This way if one test fails the others still run correctly. Think of each test as its own little island. It works on its own without needing anything from the outside.

The beauty of writing good tests is that they become documentation. When someone new joins your team and wants to understand how your code works they can read the tests. The tests show exactly what the code is supposed to do, what inputs it takes, what outputs it produces, and what edge cases it handles. Tests are like living documentation that never goes out of date because they run with every build.
