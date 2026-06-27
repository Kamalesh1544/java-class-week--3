# What is a Module?

## Definition
A **module** is a named, self-contained unit of code that contains packages and resources. It has a clear boundary defining what it exposes and what it requires.

## Module Components
```
my-module/
├── module-info.java          // Module descriptor
├── com/
│   └── example/
│       └── myapp/
│           ├── PublicClass.java      // Accessible
│           └── InternalClass.java    // Hidden
└── resources/
    └── config.properties
```

## Module Descriptor (module-info.java)
The `module-info.java` file is the heart of every module. It declares:
- **Module name**: Unique identifier
- **Dependencies**: Other modules it needs
- **Exports**: Packages it makes available to other modules
- **Opens**: Packages opened for reflection
- **Services**: What it provides and uses

```java
module com.example.myapp {
    // Dependencies
    requires java.sql;
    requires com.google.gson;
    
    // Exports
    exports com.example.myapp.api;
    
    // Opens for reflection
    opens com.example.myapp.model;
}
```

## Module Name Conventions
- Usually reversed domain name: `com.example.project`
- Must be unique across all modules
- Follows Java package naming rules
- Cannot start with `java.` or `jdk.` (reserved for JDK modules)

## Module Types
| Type | Description | Example |
|------|-------------|---------|
| Named Module | Has a module-info.java | Your application modules |
| Automatic Module | JAR without module-info.java | Legacy libraries |
| Unnamed Module | Default module for classpath | Non-modular code |

## Module Path vs Classpath
```
Classpath (Old Way):
  java -cp lib1.jar:lib2.jar com.example.Main

Module Path (New Way):
  java --module-path mods/ --module com.example.myapp/com.example.Main
```
