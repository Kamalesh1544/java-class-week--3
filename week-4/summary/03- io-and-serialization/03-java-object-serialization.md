# Java Object Serialization

Serialization is basically converting a Java object into a stream of bytes so you can save it to a file, send it over a network, or store it in a database. Think of it like freezing a object into a byte format that you can later unfreeze back into the same object. This is super useful when you want to save the state of your program or send data between different parts of a system. To make a class serializable you just implement the Serializable interface which is a marker interface meaning it has no methods to implement — you just add it to your class declaration. Then you use ObjectOutputStream to write the object and ObjectInputStream to read it back. One important thing is that if your class has references to other objects, those other classes also need to be serializable or you mark them as transient so they get skipped during serialization. Transient fields are basically fields you do not want to save like passwords or temporary data. The whole process is pretty straightforward once you get the hang of it and it comes in handy a lot when working with real applications.

```java
import java.io.*;

// Step 1: Implement Serializable
class Employee implements Serializable {
    String name;
    double salary;
     String password;  

    Employee(String name, double salary, String password) {
        this.name = name;
        this.salary = salary;
        this.password = password;
    }
}

// Step 2: Serialize (write object to file)
Employee emp = new Employee("Alice", 50000, "secret123");

try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("employee.ser"))) {
    oos.writeObject(emp);
    System.out.println("Object saved");
}

// Step 3: Deserialize (read object from file)
try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("employee.ser"))) {
    Employee loaded = (Employee) ois.readObject();
    System.out.println("Name: " + loaded.name);        // Alice
    System.out.println("Salary: " + loaded.salary);    // 50000.0
    System.out.println("Password: " + loaded.password); // null (transient)
}
```
