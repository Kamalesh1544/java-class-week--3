# Annotations

Annotations are basically tags or labels you put on your code to give extra information to the compiler, frameworks, or runtime. They look like @ followed by a name and they do not change how your code runs by themselves — they just provide metadata that something else can read and act on. You have been using annotations all along without realizing it. @Override tells the compiler that a method is overriding a parent class method and if it is not actually overriding anything the compiler gives you an error which saves you from bugs. @Deprecated marks something as outdated so other developers know not to use it. @SuppressWarnings tells the compiler to shut up about certain warnings. These built-in annotations are processed by the compiler. But the real power comes from framework annotations like @Autowired, @GetMapping, @Entity, @Transactional which are read by frameworks at runtime or compile time to make decisions about your code. Spring uses annotations to wire dependencies automatically. Hibernate uses them to map Java objects to database tables. JUnit uses them to identify test methods. The annotation itself does nothing — it is the framework that reads it and does something with it. This is a really clean way to add behavior to code without cluttering it with logic. You just tag your code with the right annotations and the framework handles the rest.

```java
// Built-in annotations
@Override
public String toString() {
    return "This method overrides the parent";
}

@Deprecated
public void oldMethod() {
    // this method is outdated
}

@SuppressWarnings("unchecked")
public List<String> getData() {
    return (List<String>) getDataRaw();
}

// Framework annotations
@Autowired
private UserService userService;   // Spring auto-injects this

@GetMapping("/users")
public List<User> getUsers() {     // Spring maps this to HTTP GET /users
    return userService.getAll();
}

@Entity
@Table(name = "students")
public class Student {              // Hibernate maps this to a database table
    @Id
    @GeneratedValue
    private Long id;

    @Column(name = "student_name")
    private String name;
}
```
