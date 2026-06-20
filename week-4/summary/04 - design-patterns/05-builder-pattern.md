# Builder Pattern

Builder pattern is used when you need to create an object that has a lot of optional parameters. Instead of creating ten different constructors or passing a bunch of null values, you build the object step by step using descriptive method calls. It reads almost like English — you say .setName("Alice").setAge(25).setCity("Chennai").build() and you get a fully constructed object at the end. The main class has a static inner class called Builder which has all the setter methods that return the builder itself so you can chain them together. When you are done setting everything you call .build() which creates the final immutable object. This is especially useful when some fields are optional — you only set what you need and the rest get default values. The pattern is everywhere in real code — StringBuilder uses it, many API clients use it, and frameworks like Lombok even generate it for you with one annotation. The key thing to remember is that the Builder should create an immutable object so once you call build() you cannot change it. This makes your code safer because you do not have to worry about someone accidentally modifying an object after it is created.

```java
class Employee {
    private final String name;
    private final int age;
    private final String department;
    private final double salary;

    private Employee(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.department = builder.department;
        this.salary = builder.salary;
    }

    static class Builder {
        private String name;
        private int age;
        private String department = "General";
        private double salary = 0;

        Builder(String name) { this.name = name; }

        Builder age(int age) { this.age = age; return this; }
        Builder department(String dept) { this.department = dept; return this; }
        Builder salary(double salary) { this.salary = salary; return this; }

        Employee build() { return new Employee(this); }
    }

    public String toString() {
        return name + " | " + age + " | " + department + " | Rs." + salary;
    }
}

// Usage — clean and readable
Employee emp = new Employee.Builder("Alice")
    .age(25)
    .department("Engineering")
    .salary(75000)
    .build();

System.out.println(emp);   // Alice | 25 | Engineering | Rs.75000.0
```
