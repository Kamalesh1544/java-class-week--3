# Migration to Java 9 Modules

## Migration Strategy Overview
Migrating to modules can be done incrementally. You don't need to convert everything at once.

## Phase 1: Preparation
### Analyze Your Project
```
Assessment Checklist:
├── Identify all dependencies (internal + external)
├── Map package usage between components
├── Check for internal API usage (sun.*, com.sun.*)
├── Identify reflection-heavy frameworks
└── Review build configuration
```

### Update Build Tools
```xml
<!-- Maven pom.xml -->
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
</properties>

<!-- Gradle build.gradle -->
plugins {
    id 'java-library'
}
java {
    sourceCompatibility = JavaVersion.VERSION_11
}
```

## Phase 2: Start with Simple Modules
### Create First module-info.java
```java
// Start with your least complex module
module com.example.utils {
    exports com.example.utils.helpers;
    exports com.example.utils.formatters;
    
    requires java.logging;
    requires java.sql;
}
```

### Test After Each Module Addition
```bash
# Build and test
mvn clean package

# Run with module path
java --module-path target/classes \
     --module com.example.utils/com.example.utils.Main
```

## Phase 3: Handle Dependencies

### External Libraries (Automatic Modules)
```java
// If library has no module-info.java
module com.example.myapp {
    requires automatic.library;  // Uses derived name
    requires java.sql;
}
```

### Create Wrapper Modules for Legacy Libraries
```java
// my-legacy-wrapper module-info.java
module my.legacy.wrapper {
    // Read the JAR as automatic module
    requires legacy.jar;
    
    // Re-export what other modules need
    exports com.legacy.api;
    exports com.legacy.model;
}
```

## Phase 4: Framework Configuration

### Spring Boot
```java
@SpringBootApplication
// Spring handles module-info automatically
// Just ensure your packages are open for reflection
```

```java
module com.example.springboot {
    requires spring.boot;
    requires spring.context;
    
    opens com.example.springboot to 
        spring.core,
        spring.context;
}
```

### Hibernate/JPA
```java
module com.example.orm {
    requires hibernate.core;
    requires java.persistence;
    
    // Open entities for reflection
    opens com.example.entity to 
        hibernate.core,
        hibernate.orm;
}
```

## Common Migration Patterns

### Pattern 1: Modular Monolith
```
Before:
└── single-app.jar (everything in one JAR)

After:
└── modular-app/
    ├── app-core/
    │   └── module-info.java
    ├── app-api/
    │   └── module-info.java
    └── app-service/
        └── module-info.java
```

### Pattern 2: Gradual Migration
```
Step 1: Move JARs to module path
    └── Automatic modules

Step 2: Add module-info to your code
    └── Named module

Step 3: Update libraries to modules
    └── Full modularization
```

## Troubleshooting Common Issues

### Issue 1: IllegalAccessError
```java
// Problem
module com.example.app {
    requires java.sql;
    // But you're using internal JDBC methods
}

// Solution: Use standard API or --add-opens
java --add-opens java.sql/com.sun.rowset=ALL-UNNAMED
```

### Issue 2: Missing Reflection Access
```java
// Problem: Jackson can't access your model
module com.example.api {
    requires com.fasterxml.jackson.databind;
    // Jackson throws IllegalAccessException
}

// Solution: Open the package
module com.example.api {
    requires com.fasterxml.jackson.databind;
    opens com.example.model to com.fasterxml.jackson.databind;
}
```

### Issue 3: Split Packages
```
// Problem: Two modules export same package
module-a exports com.example
module-b exports com.example  // CONFLICT!

// Solution: Restructure packages
module-a exports com.example.a
module-b exports com.example.b
```

## Migration Checklist
```
□ Update Java version to 9+
□ Analyze project dependencies
□ Identify internal API usage
□ Start with simple modules
□ Create module-info.java files
□ Handle automatic modules
□ Configure framework reflection
□ Test after each change
□ Update build scripts
□ Document module structure
```

## Best Practices
1. **Start Early**: Don't wait for everything to be perfect
2. **One Module at a Time**: Incremental migration
3. **Test Thoroughly**: Module changes can break things
4. **Use Tools**: jdeps, jlink, jmod help with migration
5. **Document Decisions**: Why you used opens, exports, etc.
6. **Train Team**: Ensure everyone understands modules
