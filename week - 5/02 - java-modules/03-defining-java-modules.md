# Defining Java Modules

## Creating a Module Descriptor
Every module must have a `module-info.java` file at the root of its source tree.

```java
// module-info.java
module com.example.bankapp {
    // Module declaration goes here
}
```

## Module Directives

### 1. requires
Declares a dependency on another module.
```java
module com.example.myapp {
    requires java.sql;           // JDK module
    requires com.google.gson;    // External module
    requires static javax.inject; // Optional dependency
}
```

**requires static** = Optional dependency (needed only at compile time)

### 2. exports
Makes a package accessible to other modules.
```java
module com.example.myapp {
    exports com.example.myapp.api;        // Public API
    exports com.example.myapp.model;      // Model classes
    
    // Export with restriction
    exports com.example.myapp.internal to 
        com.example.specificmodule;      // Only to specific module
}
```

### 3. opens
Opens a package for deep reflection (used by frameworks like Spring).
```java
module com.example.myapp {
    opens com.example.myapp.model;  // For Jackson, Hibernate
    opens com.example.myapp.dto to com.fasterxml.jackson.databind;
}
```

### 4. provides / uses
Service Provider Interface (SPI) for dependency injection.
```java
// Module providing a service
module com.example.email {
    provides com.example.EmailService 
        with com.example.SmtpEmailService;
}

// Module using a service
module com.example.app {
    uses com.example.EmailService;
}
```

## Complete Example
```java
module com.example.webapp {
    // Dependencies
    requires java.servlet;
    requires java.logging;
    requires com.fasterxml.jackson.databind;
    
    // Public API
    exports com.example.webapp.controller;
    exports com.example.webapp.model;
    
    // Internal packages (not exported)
    // com.example.webapp.service stays internal
    
    // Opens for framework reflection
    opens com.example.webapp.model to 
        com.fasterxml.jackson.databind;
    
    // Service provider
    provides com.example.webapp.spi.Formatter 
        with com.example.webapp.service.JsonFormatter;
}
```

## Package Visibility Rules
```
Package Access Levels:
├── Exported Package    → Accessible by any module
├── Opened Package      → Accessible + Reflectable
├── Unexported Package  → Only within same module
└── Unopened Package    → No reflection access
```
