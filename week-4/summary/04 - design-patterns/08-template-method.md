# Template Method Pattern

Template Method pattern defines the skeleton of an algorithm in a base class but lets subclasses override specific steps of that algorithm without changing its overall structure. Think of it like a recipe — the base class says first do this, then do that, then finish with this, but it leaves certain ingredients or techniques up to the subclasses. You write the main flow once in a template method and mark the variable parts as abstract methods that subclasses must implement. This gives you code reuse for the common parts while allowing flexibility for the specific parts. The base class controls the overall flow and the subclasses just fill in the details. A good example is a data processing pipeline where the steps are always read data, process data, and write results — the read and write steps might be the same for many processors but the processing logic changes. So you put the read and write in the base class and make process() abstract. The key thing is the base class method should not be overridden by subclasses — you usually make it final to enforce this. The template method calls the abstract hooks at the right points and the subclass provides the actual implementation. This is used heavily in frameworks like Spring where the framework defines the flow and you provide the custom logic.

```java
abstract class DataProcessor {

    // Template method — final so subclasses cannot change the flow
    final void process() {
        readData();
        processData();
        writeResults();
    }

    void readData() {
        System.out.println("Reading data from source");
    }

    abstract void processData();   // subclass decides how

    void writeResults() {
        System.out.println("Writing results to output");
    }
}

class SalesProcessor extends DataProcessor {
    void processData() {
        System.out.println("Processing sales data — calculating totals");
    }
}

class UserProcessor extends DataProcessor {
    void processData() {
        System.out.println("Processing user data — validating emails");
    }
}

// Usage
DataProcessor sales = new SalesProcessor();
sales.process();
// Reading data from source
// Processing sales data — calculating totals
// Writing results to output

DataProcessor users = new UserProcessor();
users.process();
// Reading data from source
// Processing user data — validating emails
// Writing results to output
```
