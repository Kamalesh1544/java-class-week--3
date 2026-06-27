# Code Coverage — The Chill Version

Alright so you have written a bunch of tests and you think your code is covered. But how do you actually know how much of your code is being tested? That is where code coverage comes in. It is basically a tool that watches your tests run and tracks which lines of code actually get executed. Then it gives you a report saying hey eighty percent of your code was tested and twenty percent was not.

The most common metric is line coverage. It tells you what percentage of your code lines were hit during testing. If you have a hundred lines of code and your tests execute eighty of them you have eighty percent line coverage. Pretty straightforward.

Then there is branch coverage which is a bit smarter. It checks if your tests cover both sides of every if statement. Like if you have an if else block did your tests run through the if path and the else path? Branch coverage catches cases where you only test the happy path and completely ignore the else branch.

The most popular tool for Java is JaCoCo. It integrates with Maven so you just add it as a plugin in your pom.xml and run mvn test. It generates a report in HTML format that you can open in your browser. The report shows you exactly which lines were covered and which were not. Lines covered are green. Lines not covered are red. Lines partially covered are yellow.

Now here is the thing. Do not obsess over getting one hundred percent coverage. It is a nice goal but it is not always practical. Some code like getters and setters does not really need testing. Configuration classes do not need testing. The important thing is that your business logic is well tested. Your core functionality your edge cases your error handling. Those should be high coverage.

A common mistake is writing tests just to increase coverage without actually testing anything meaningful. Like calling a method and not asserting anything. That gives you line coverage but does not actually verify anything. Quality matters more than quantity.

Another thing to understand is that coverage tells you what was tested not what was not tested. If your coverage is ninety percent it means ninety percent of your code was executed during tests. It does not mean ninety percent of your bugs were found. You could have perfect coverage and still have bugs because your assertions are wrong.

The practical approach is to aim for seventy to eighty percent coverage on your business logic. Use the coverage report to find untested code. Focus on the critical paths first. Edge cases and error handling next. And do not waste time testing simple getters and setters. Use coverage as a guide not a goal. It helps you find gaps in your testing but it does not tell you the whole story.

The real value of coverage tools is that they show you what you missed. You look at the report and see oh I never tested the case where the input is null. Or I never tested what happens when the database connection fails. That is where you should focus your testing efforts next.
