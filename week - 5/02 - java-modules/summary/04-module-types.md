# Module Types

## Overview of Module Categories
Java 9 introduced four types of modules to support both modular and legacy code.

## 1. Named Modules
Modules with an explicit `module-info.java` descriptor.

```
Feature:
├── Has module-info.java
├── Declares dependencies explicitly
├── Controls package visibility
└── Located on module path

Example:
└── com.example.myapp/
    ├── module-info.java
    ├── com/example/myapp/
    │   ├── Main.java
    │   └── Service.java
    └── resources/
```

```java
// module-info.java
module com.example.myapp {
    requires java.sql;
    exports com.example.myapp.api;
}
```

## 2. Automatic Modules
Legacy JAR files without `module-info.java`. Java automatically converts them.

```
Feature:
├── No module-info.java
├── Module name derived from JAR filename
├── All packages are exported
└── Located on module path

Module Name Derivation:
├── mylibrary.jar       → mylibrary
├── my-library.jar      → my.library
└── my_library.jar      → my.library
```

**Rules for automatic module names:**
- Remove `.jar` extension
- Replace `-` and `_` with `.`
- Remove non-alphanumeric characters

## 3. Unnamed Modules
Default container for classes not in any named module.

```
Feature:
├── No module-info.java
├── Located on classpath (not module path)
├── All packages are exported
├── Can access all exported packages
└── Cannot be required by named modules

Usage:
└── Legacy code running on classpath
    java -cp old-library.jar com.example.Main
```

## 4. The JDK Module (java.base)
The foundation module that all other modules depend on.

```
java.base (Implicit)
├── java.lang
├── java.util
├── java.io
├── java.net
└── ... (core packages)

// Automatically available, no need to declare:
// requires java.base;  ← This is implicit
```

## Module Type Comparison
| Aspect | Named | Automatic | Unnamed | JDK |
|--------|-------|-----------|---------|-----|
| Has module-info | ✅ | ❌ | ❌ | ✅ |
| Module name | Explicit | Derived from JAR | None | java.base |
| On module path | ✅ | ✅ | ❌ | ✅ |
| Exports | Controlled | All | All | All |
| Can be required | ✅ | ✅ | ❌ | ✅ |

## Practical Example: Using an Automatic Module
```java
// If you have old-library.jar (no module-info.java)
// Place it on module path

module com.example.myapp {
    requires old.library;  // Derived from old-library.jar
    requires java.sql;
}
```

## Migration Path
```
Step 1: Legacy JARs on classpath
    └── Unnamed modules

Step 2: Move JARs to module path
    └── Become automatic modules

Step 3: Add module-info.java
    └── Become named modules
```
