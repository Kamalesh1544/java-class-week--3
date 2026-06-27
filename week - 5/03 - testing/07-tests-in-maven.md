# Tests in Maven

## Maven Surefire Plugin
Maven uses the **Surefire Plugin** to run tests during the build lifecycle.

## Default Behavior
```bash
# Runs tests during test phase
mvn test

# Runs tests during package phase
mvn package

# Skips tests
mvn package -DskipTests

# Skips tests completely
mvn package -Dmaven.test.skip=true
```

## Maven Test Phases
```
test-compile → Compile test sources
     ↓
    test      → Run unit tests
     ↓
   package   → Package JAR (tests already passed)
     ↓
    install  → Install to local repository
```

## Project Structure
```
my-app/
├── src/
│   ├── main/
│   │   └── java/
│   │       └── com/example/
│   │           └── Calculator.java
│   └── test/
│       └── java/
│           └── com/example/
│               └── CalculatorTest.java
├── pom.xml
└── target/
    └── surefire-reports/
        └── TEST-com.example.CalculatorTest.xml
```

## Surefire Plugin Configuration
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>3.2.2</version>
    <configuration>
        <!-- Include test patterns -->
        <includes>
            <include>**/*Test.java</include>
            <include>**/*Tests.java</include>
        </includes>
        
        <!-- Exclude test patterns -->
        <excludes>
            <exclude>**/*IntegrationTest.java</exclude>
        </excludes>
        
        <!-- Parallel execution -->
        <parallel>methods</parallel>
        <threadCount>4</threadCount>
    </configuration>
</plugin>
```

## Full Maven Test Setup
```xml
<project>
    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
        <junit.version>5.10.0</junit.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <!-- Surefire for unit tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>
            
            <!-- JaCoCo for coverage -->
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>0.8.11</version>
                <executions>
                    <execution>
                        <goals><goal>prepare-agent</goal></goals>
                    </execution>
                    <execution>
                        <id>report</id>
                        <phase>test</phase>
                        <goals><goal>report</goal></goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

## Running Specific Tests
```bash
# Run all tests
mvn test

# Run specific test class
mvn test -Dtest=CalculatorTest

# Run specific test method
mvn test -Dtest=CalculatorTest#shouldAddNumbers

# Run multiple test classes
mvn test -Dtest="CalculatorTest,UserServiceTest"

# Run tests matching pattern
mvn test -Dtest="*Service*"
```

## Test Reports
```bash
# Generate surefire reports
mvn surefire-report:report

# View reports
target/surefire-reports/
├── TEST-com.example.CalculatorTest.xml
├── TEST-com.example.UserServiceTest.xml
└── surefire-reports.html
```

## Maven Failsafe Plugin (Integration Tests)
```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-failsafe-plugin</artifactId>
    <version>3.2.2</version>
    <executions>
        <execution>
            <goals>
                <goal>integration-test</goal>
                <goal>verify</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```

```bash
# Run integration tests
mvn verify

# Skip integration tests
mvn verify -DskipTests
```

## Common Commands
```bash
# Run tests with coverage
mvn clean test jacoco:report

# Run tests and see output
mvn test -q

# Run tests with verbose output
mvn test -X

# Run tests in parallel
mvn test -T 4

# Run tests and fail build on failure
mvn test --fail-at-end
```

## CI/CD Integration
```yaml
# GitHub Actions example
- name: Run Tests
  run: mvn test

- name: Generate Coverage
  run: mvn jacoco:report

- name: Upload Coverage
  uses: codecov/codecov-action@v3
```
