# JSON in Java

---

## What is JSON?

JSON (JavaScript Object Notation) is a **lightweight data format** used to exchange data between a server and a client. It is human-readable and easy to parse.

### Example JSON:
```json
{
  "name": "Alice",
  "age": 25,
  "city": "Chennai",
  "skills": ["Java", "Python", "SQL"]
}
```

---

## What is Jackson?

Jackson is the **most popular JSON library** for Java. It is used in Spring Boot, and most enterprise applications. The main class is `ObjectMapper` which handles all JSON operations.

---

## Setup — Add Dependency

```xml
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.15.2</version>
</dependency>
```

---

## Java Object to JSON (Serialization)

Convert a Java object into a JSON string.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JacksonDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 25, "Chennai");

        ObjectMapper mapper = new ObjectMapper();
        String json = mapper.writeValueAsString(student);

        System.out.println(json);
    }
}

class Student {
    String name;
    int age;
    String city;

    Student() {}   // default constructor needed

    Student(String name, int age, String city) {
        this.name = name;
        this.age = age;
        this.city = city;
    }
}
```

### Output:
```
{"name":"Alice","age":25,"city":"Chennai"}
```

---

## JSON to Java Object (Deserialization)

Convert a JSON string back into a Java object.

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JacksonParseDemo {
    public static void main(String[] args) throws Exception {
        String json = "{\"name\":\"Bob\",\"age\":30,\"city\":\"Mumbai\"}";

        ObjectMapper mapper = new ObjectMapper();
        Student student = mapper.readValue(json, Student.class);

        System.out.println("Name: " + student.name);
        System.out.println("Age: " + student.age);
        System.out.println("City: " + student.city);
    }
}
```

### Output:
```
Name: Bob
Age: 30
City: Mumbai
```

---

## Pretty Print JSON

Make JSON output readable with proper formatting.

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.SerializationFeature;

public class PrettyJsonDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 25, "Chennai");

        ObjectMapper mapper = new ObjectMapper();
        mapper.enable(SerializationFeature.INDENT_OUTPUT);

        String prettyJson = mapper.writeValueAsString(student);
        System.out.println(prettyJson);
    }
}
```

### Output:
```json
{
  "name" : "Alice",
  "age" : 25,
  "city" : "Chennai"
}
```

---

## Read JSON from a File

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;

public class ReadJsonFile {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();
        Student student = mapper.readValue(new File("student.json"), Student.class);

        System.out.println("Name: " + student.name);
        System.out.println("Age: " + student.age);
    }
}
```

---

## Write JSON to a File

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;

public class WriteJsonFile {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 25, "Chennai");

        ObjectMapper mapper = new ObjectMapper();
        mapper.writerWithDefaultPrettyPrinter()
              .writeValue(new File("student.json"), student);

        System.out.println("JSON written to file");
    }
}
```

---

## JSON Array — Convert List to JSON

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.util.Arrays;
import java.util.List;

public class JsonArrayDemo {
    public static void main(String[] args) throws Exception {
        List<Student> students = Arrays.asList(
            new Student("Alice", 25, "Chennai"),
            new Student("Bob", 30, "Mumbai"),
            new Student("Charlie", 22, "Delhi")
        );

        ObjectMapper mapper = new ObjectMapper();
        String json = mapper.writeValueAsString(students);

        System.out.println(json);
    }
}
```

### Output:
```
[{"name":"Alice","age":25,"city":"Chennai"},{"name":"Bob","age":30,"city":"Mumbai"},{"name":"Charlie","age":22,"city":"Delhi"}]
```

---

## Parse JSON Array — Convert JSON to List

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.core.type.TypeReference;
import java.util.List;

public class ParseJsonArray {
    public static void main(String[] args) throws Exception {
        String json = "[{\"name\":\"Alice\",\"age\":25},{\"name\":\"Bob\",\"age\":30}]";

        ObjectMapper mapper = new ObjectMapper();
        List<Student> students = mapper.readValue(json, new TypeReference<List<Student>>() {});

        for (Student s : students) {
            System.out.println(s.name + " : " + s.age);
        }
    }
}
```

### Output:
```
Alice : 25
Bob : 30
```

---

## Working with Nested JSON

```json
{
  "company": "TechCorp",
  "employees": [
    {"name": "Alice", "department": "Engineering"},
    {"name": "Bob", "department": "Marketing"}
  ]
}
```

### Java classes:
```java
class Company {
    String name;
    List<Employee> employees;
}

class Employee {
    String name;
    String department;
}
```

### Parse:
```java
import com.fasterxml.jackson.databind.ObjectMapper;
import java.io.File;
import java.util.List;

public class NestedJsonDemo {
    public static void main(String[] args) throws Exception {
        ObjectMapper mapper = new ObjectMapper();
        Company company = mapper.readValue(new File("company.json"), Company.class);

        System.out.println("Company: " + company.name);
        for (Employee e : company.employees) {
            System.out.println(e.name + " - " + e.department);
        }
    }
}
```

### Output:
```
Company: TechCorp
Alice - Engineering
Bob - Marketing
```

---

## Jackson Annotations

Jackson provides annotations to control serialization and deserialization.

| Annotation | Description |
|---|---|
| `@JsonProperty("name")` | Use custom field name in JSON |
| `@JsonIgnore` | Skip this field completely |
| `@JsonInclude(Include.NON_NULL)` | Don't include null values |
| `@JsonFormat(pattern="yyyy-MM-dd")` | Format dates |
| `@JsonIgnoreProperties(ignoreUnknown=true)` | Ignore extra fields in JSON |

### Example with annotations:
```java
import com.fasterxml.jackson.annotation.*;

class Student {
    @JsonProperty("student_name")
    String name;

    int age;

    @JsonIgnore
    String password;   // will not appear in JSON

    @JsonInclude(JsonInclude.Include.NON_NULL)
    String city;   // won't appear if null

    Student() {}
    Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

```java
Student s = new Student("Alice", 25);
String json = new ObjectMapper().writeValueAsString(s);
System.out.println(json);
```

### Output:
```json
{"student_name":"Alice","age":25}
```

---

## Key Points

- Jackson is the **most popular** JSON library in Java
- `ObjectMapper` is the main class — handles all operations
- `writeValueAsString()` — Java object → JSON string
- `readValue()` — JSON string → Java object
- Use `SerializationFeature.INDENT_OUTPUT` for pretty printing
- Use `TypeReference<List<Student>>` for parsing JSON arrays
- Always have a **default no-arg constructor** in your Java class
- Use annotations (`@JsonProperty`, `@JsonIgnore`) to control output
