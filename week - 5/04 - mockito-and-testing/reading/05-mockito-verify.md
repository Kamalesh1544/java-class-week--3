# Mockito Verify — The Chill Version

So stubbing is great for setting up what your mocks return. But sometimes you need to check that certain methods were actually called. Like after creating a user did your code actually save it to the database? Did it send a welcome email? Did it log the action? Verification lets you check all of this.

The basic verify() method checks that a method was called at least once. Like verify(mockRepo).save(user) checks that save was called with that user. If it was not called the test fails. Simple enough. But you can get more specific with verification times. times(3) checks the method was called exactly three times. never() checks it was never called. atLeastOnce() checks it was called at least once. atLeast(2) checks it was called at least twice. atMost(5) checks it was called at most five times. These give you precise control over how many times a method should be called.

Argument verification works just like stubbing. You can use exact values or matchers. Like verify(mockEmail).send(eq("john@test.com"), argThat(msg -> msg.contains("Welcome"))) checks that send was called with that exact email and a message containing Welcome. You can verify complex argument conditions using the same matchers you use for stubbing.

Argument Captors are really cool. They let you capture the actual arguments that were passed to a method and then assert on them. Like if you want to verify that the user saved to the database has the correct name and email you use an ArgumentCaptor. You pass it to verify and then call getValue() on it to get the actual argument. Then you can assert that the captured user has the name and email you expect. This is much more flexible than trying to match the exact object.

InOrder verification checks that methods were called in a specific order. Like if creating a user should first save to the database and then send an email you use InOrder. You create an inOrder instance with the mocks you want to check and then verify the calls in sequence. If the methods were called out of order the test fails. This is useful for testing workflows where the order of operations matters.

verifyNoMoreInteractions() checks that no other methods were called on a mock besides what you already verified. Like if you verified that save was called but you did not verify delete you might have missed an unwanted delete call. verifyNoMoreInteractions catches that. verifyNoInteractions() checks that no methods at all were called on a mock. These are great for making sure your code does not have unexpected side effects.

The timeout() modifier lets you verify that a method was called within a specific time. Like verify(mockRepo, timeout(1000)).save(user) checks that save was called within one second. This is useful for testing asynchronous code where you need to wait for something to happen.

Common mistakes people make with verification are verifying too much and verifying too little. Do not verify every single method call. Focus on the important interactions. The ones that are critical to your business logic. And do not forget to verify important side effects like sending emails or saving to databases. The right balance is verifying the key interactions that prove your code works correctly without turning your test into a brittle implementation detail check.
