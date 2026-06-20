# Method References

Method references are basically a shortcut for writing lambdas when all you are doing is calling an existing method. Instead of writing a full lambda expression you can just refer to the method directly using the :: operator. It makes your code shorter and cleaner without losing any readability. There are four types of method references you will run into. First is a static method reference like Integer::parseInt where you are calling a static method. Second is an instance method reference on a particular object like System.out::println where you are calling println on that specific out object. Third is an instance method reference on an arbitrary object of a given type like String::toLowerCase where it calls toLowerCase on whatever string you pass in. And fourth is a constructor reference like ArrayList::new which creates a new ArrayList. The whole point is to write less boilerplate when the lambda is trivial. If your lambda is just doing one method call on something, a method reference will do the same job in fewer characters and it reads more naturally.

```java
import java.util.*;
import java.util.stream.*;

public class MethodRefDemo {
    public static void main(String[] args) {
        List<String> names = List.of("alice", "bob", "charlie");

        // Lambda
        names.forEach(name -> System.out.println(name));

        // Method reference (shorthand)
        names.forEach(System.out::println);

        // Static method reference
        List<String> nums = List.of("1", "2", "3");
        List<Integer> integers = nums.stream()
            .map(Integer::parseInt)
            .toList();
        System.out.println(integers);   // [1, 2, 3]

        // Constructor reference
        List<String> list = names.stream()
            .collect(ArrayList::new, ArrayList::add, ArrayList::addAll);
        System.out.println(list);
    }
}
```
