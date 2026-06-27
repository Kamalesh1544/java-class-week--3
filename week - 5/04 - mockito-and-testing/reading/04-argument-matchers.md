# Argument Matchers — The Chill Version

So you know how you can stub a method to return something when called with specific arguments? Like when(mockRepo.findById(1L)).thenReturn(user). That works great when you know exactly what argument will be passed. But sometimes you do not care about the exact value. You just want the stub to work for any value of that type. That is where argument matchers come in.

The most basic matcher is any(). It matches any argument of the specified type. Like when(mockRepo.save(any(User.class))).thenReturn(user) means whenever someone saves any User object return this user. It does not matter what the actual user object is. As long as it is a User the stub kicks in. This is super useful when the argument is complex or when you just do not care about the specific value.

For strings there are several useful matchers. anyString() matches any string. eq("hello") matches only that exact string. argThat(s -> s.startsWith("PRE_")) matches any string that starts with PRE_. You can write custom conditions using lambda expressions. Like argThat(s -> s.length() > 10) matches any string longer than 10 characters. This gives you incredible flexibility in matching arguments.

Numbers work the same way. anyInt() matches any integer. anyDouble() matches any double. argThat(n -> n > 100) matches any number greater than 100. You can combine conditions like argThat(n -> n >= 1 && n <= 100) to match a range. This is great for testing boundary conditions.

Collections have their own matchers. anyList() matches any list. eq(Collections.emptyList()) matches only an empty list. argThat(l -> l.size() == 3) matches any list with exactly 3 elements. argThat(l -> l.contains("target")) matches any list containing a specific element. You can write complex conditions to match exactly what you need.

The really powerful ones are allOf() and anyOf(). allOf combines multiple conditions with AND logic. Like argThat(allOf(s -> s.length() > 8, s -> s.contains("@"), s -> s.matches(".*\\d.*"))) matches strings that are longer than 8 characters, contain an @ symbol, and contain at least one digit. anyOf combines conditions with OR logic. Like argThat(anyOf(s -> s.startsWith("ADMIN_"), s -> s.startsWith("USER_"))) matches strings that start with either ADMIN_ or USER_.

Custom matchers let you create reusable matching logic. You extend ArgumentMatcher and write your own matching logic. Then you can use it like any other matcher. This is useful when you have complex matching rules that you use in multiple tests. Create the matcher once and use it everywhere.

The important thing to remember is that whenever you use a matcher for one argument you have to use matchers for all arguments in that method call. You cannot mix matchers and exact values. Like verify(mockEmail).send(eq("user@test.com"), anyString()) works. But verify(mockEmail).send("user@test.com", anyString()) will throw an error. Either use all matchers or all exact values. Never mix them.
