# Java Linker

## What is the Java Linker?
The Java Linker is a new component introduced in Java 9 that resolves module dependencies and validates access at startup time. It replaces the traditional lazy class loading approach with eager resolution.

## Traditional Class Loading vs Linker
```
Before Java 9 (Lazy Loading):
├── Classes loaded on demand
├── Errors found at runtime
├── ClassNotFoundException late in execution
└── No upfront validation

After Java 9 (Eager Linking):
├── All dependencies resolved at startup
├── Errors found immediately
├── LinkageError at launch time
└── Upfront validation
```

## Linker Responsibilities

### 1. Resolution
Find and load all required modules.
```
Module A requires Module B requires Module C
    ↓
Linker resolves entire dependency chain
    ↓
All modules loaded before main() runs
```

### 2. Accessibility Checking
Validate that module access rules are respected.
```java
// At compile time AND runtime:
module A {
    requires B;
}

// Linker ensures:
// - Module B exists
// - Module B exports packages that A uses
// - No illegal reflective access
```

### 3. Integrity Verification
Ensure consistent state across modules.
- Version compatibility checks
- Circular dependency detection
- Missing dependency detection

## Linker in Action
```bash
# Traditional way - might fail later
java -cp app.jar com.example.Main
# ... runs for 5 minutes ...
# ClassNotFoundException: com.example.MissingClass

# Module way - fails immediately
java --module-path mods/ --module com.example.app/com.example.Main
# Error: module com.example.missing not found
# Fails fast, no wasted time
```

## Module Resolution Process
```
Step 1: Parse module-info.java
    ↓
Step 2: Build module graph
    ↓
Step 3: Resolve all requires
    ↓
Step 4: Check exports
    ↓
Step 5: Verify accessibility
    ↓
Step 6: Link modules
    ↓
Step 7: Start application
```

## Error Types from Linker
| Error | Cause | Example |
|-------|-------|---------|
| ModuleNotFoundException | Missing dependency | requires non-existent module |
| IllegalAccessError | Accessing non-exported package | Using internal API |
| LinkageError | Incompatible module versions | Version conflict |
| ResolutionException | Circular dependency | A→B→C→A |

## Benefits of the Linker
- **Fail Fast**: Errors caught at startup, not during execution
- **Performance**: No runtime class loading overhead
- **Security**: Enforces access rules from the start
- **Predictability**: All dependencies known upfront
- **Better Error Messages**: Clear indication of missing/inaccessible modules

## Example: Linker in Practice
```java
module com.example.webapp {
    requires java.servlet;      // Linker verifies this exists
    requires com.google.gson;   // Linker checks module path
    exports com.example.api;    // Linker validates exports
}
```

If `com.google.gson` is not on the module path:
```
Error occurred during initialization of VM
java.lang.module.FindException: Module com.google.gson not found
```
