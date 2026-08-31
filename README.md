# Kotlin + Spring Boot Learning Lab

A practical Kotlin learning project built with **Spring Boot 4.1.1**, **Kotlin 2.2.21**, **Java 21**, Gradle Kotlin DSL, JPA, H2, Validation, Web MVC, and Kotlin Coroutines.

The goal is to learn Kotlin by writing and running real Spring Boot code instead of studying Kotlin syntax in isolation.

---

## 1. Prerequisites

Install the following before starting:

- JDK 21
- VS Code
- Git for Windows (recommended)
- Java Extension Pack for VS Code (recommended)
- Extension Pack for Java / Kotlin support as needed

### Verify Java

Open the VS Code terminal:

```bash
java -version
```

You should see Java 21.

Example:

```text
java version "21.x.x"
```

Also verify the Java compiler:

```bash
javac -version
```

Expected:

```text
javac 21.x.x
```

---

## 2. Project Creation

This project was created using **Spring Initializr**.

Open:

<https://start.spring.io/>

Use:

```text
Project:       Gradle - Kotlin
Language:      Kotlin
Spring Boot:   4.1.1
Packaging:     Jar
Java:          21
```

Add these dependencies:

```text
Spring Web MVC
Spring Data JPA
Validation
H2 Database
Spring Boot DevTools
```

Generate the project and extract it.

Open the extracted project folder in VS Code.

---

## 3. Expected Project Structure

The project should eventually look like:

```text
learning/
├── .gradle/
├── gradle/
│   └── wrapper/
├── src/
│   ├── main/
│   │   ├── kotlin/
│   │   │   └── com/
│   │   │       └── kotlin/
│   │   │           └── learning/
│   │   │               └── LearningApplication.kt
│   │   │
│   │   └── resources/
│   │       ├── static/
│   │       ├── templates/
│   │       └── application.yaml
│   │
│   └── test/
│
├── .gitignore
├── build.gradle.kts
├── gradlew
├── gradlew.bat
└── settings.gradle.kts
```

The old Java application file should NOT remain:

```text
src/main/java/com/kotlin/learning/LearningApplication.java
```

The application entry point should be Kotlin:

```text
src/main/kotlin/com/kotlin/learning/LearningApplication.kt
```

---

## 4. build.gradle.kts

Replace the complete contents of `build.gradle.kts` with:

```kotlin
plugins {
    kotlin("jvm") version "2.2.21"
    kotlin("plugin.spring") version "2.2.21"
    kotlin("plugin.jpa") version "2.2.21"

    id("org.springframework.boot") version "4.1.1"
    id("io.spring.dependency-management") version "1.1.7"
}

group = "com.kotlin"
version = "0.0.1-SNAPSHOT"

kotlin {
    compilerOptions {
        freeCompilerArgs.add("-Xannotation-default-target=param-property")
    }
}

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

repositories {
    mavenCentral()
}

dependencies {

    // Spring Web MVC
    implementation("org.springframework.boot:spring-boot-starter-webmvc")

    // Spring Data JPA
    implementation("org.springframework.boot:spring-boot-starter-data-jpa")

    // Validation
    implementation("org.springframework.boot:spring-boot-starter-validation")

    // H2 database
    implementation("org.springframework.boot:spring-boot-h2console")
    runtimeOnly("com.h2database:h2")

    // Kotlin + Spring integration
    implementation("org.jetbrains.kotlin:kotlin-reflect")
    implementation("com.fasterxml.jackson.module:jackson-module-kotlin")

    // Kotlin Coroutines
    implementation("org.jetbrains.kotlinx:kotlinx-coroutines-core")

    // Development
    developmentOnly("org.springframework.boot:spring-boot-devtools")

    // Testing
    testImplementation("org.springframework.boot:spring-boot-starter-data-jpa-test")
    testImplementation("org.springframework.boot:spring-boot-starter-validation-test")
    testImplementation("org.springframework.boot:spring-boot-starter-webmvc-test")
    testImplementation("org.jetbrains.kotlinx:kotlinx-coroutines-test")

    testRuntimeOnly("org.junit.platform:junit-platform-launcher")
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

---

## 5. settings.gradle.kts

Make sure `settings.gradle.kts` contains:

```kotlin
rootProject.name = "learning"
```

If Spring Initializr already generated the correct project name, you can leave it as-is.

---

## 6. Kotlin Application Class

Create:

```text
src/main/kotlin/com/kotlin/learning/LearningApplication.kt
```

Use:

```kotlin
package com.kotlin.learning

import org.springframework.boot.autoconfigure.SpringBootApplication
import org.springframework.boot.runApplication

@SpringBootApplication
class LearningApplication

fun main(args: Array<String>) {
    runApplication<LearningApplication>(*args)
}
```

This replaces the Java version:

```java
@SpringBootApplication
public class LearningApplication {

    public static void main(String[] args) {
        SpringApplication.run(LearningApplication.class, args);
    }
}
```

---

## 7. Remove the Java Application

Delete:

```text
src/main/java/com/kotlin/learning/LearningApplication.java
```

Do not keep both the Java and Kotlin application entry points.

---

## 8. First Gradle Check

Open the VS Code terminal at the project root.

The terminal should be inside the directory containing:

```text
build.gradle.kts
gradlew
gradlew.bat
settings.gradle.kts
```

Check the directory:

```bash
ls
```

You should see something similar to:

```text
build.gradle.kts
gradle
gradlew
gradlew.bat
settings.gradle.kts
src
```

---

## 9. Windows Gradle Commands

This project contains the Gradle Wrapper, so you do NOT need to install Gradle globally.

On Windows, use:

```powershell
gradlew.bat
```

or from Git Bash:

```bash
./gradlew
```

Both use the project's Gradle Wrapper.

---

## 10. Check Gradle

Run:

```bash
./gradlew --version
```

Windows alternative:

```powershell
gradlew.bat --version
```

You should see:

```text
Gradle ...
JVM: 21...
```

---

## 11. Clean the Project

Before the first build, you can clean the project:

```bash
./gradlew clean
```

Windows:

```powershell
gradlew.bat clean
```

`clean` removes the previous build output from:

```text
build/
```

It does not delete your source code.

---

## 12. Build the Project

Run:

```bash
./gradlew clean build
```

Windows:

```powershell
gradlew.bat clean build
```

This performs the complete build process:

```text
clean
  ↓
compile Kotlin
  ↓
process resources
  ↓
compile tests
  ↓
run tests
  ↓
package application
  ↓
BUILD SUCCESSFUL
```

Expected result:

```text
BUILD SUCCESSFUL
```

If you see:

```text
BUILD FAILED
```

do not randomly change dependencies.

Read the first meaningful error in the terminal.

---

## 13. Run Tests Only

To run the tests:

```bash
./gradlew test
```

Windows:

```powershell
gradlew.bat test
```

Expected:

```text
BUILD SUCCESSFUL
```

Test reports are generated under:

```text
build/reports/tests/test/
```

---

## 14. Start Spring Boot

Run:

```bash
./gradlew bootRun
```

Windows:

```powershell
gradlew.bat bootRun
```

The application should start.

Look for something similar to:

```text
Started LearningApplication
```

and:

```text
Tomcat started on port 8080
```

The default URL is:

```text
http://localhost:8080
```

---

## 15. Stop Spring Boot

In the terminal running Spring Boot:

```text
Ctrl + C
```

Confirm termination if Windows asks.

---

## 16. Build the Executable JAR

Run:

```bash
./gradlew clean build
```

The generated JAR will be under:

```text
build/libs/
```

You can see it with:

```bash
ls build/libs
```

Run the JAR:

```bash
java -jar build/libs/learning-0.0.1-SNAPSHOT.jar
```

The exact JAR filename may differ if the project version changes.

---

## 17. First Kotlin REST API

Create:

```text
src/main/kotlin/com/kotlin/learning/UserController.kt
```

Add:

```kotlin
package com.kotlin.learning

import org.springframework.web.bind.annotation.GetMapping
import org.springframework.web.bind.annotation.RestController

@RestController
class UserController {

    @GetMapping("/hello")
    fun hello(): String {
        return "Hello from Kotlin"
    }
}
```

Start the application:

```bash
./gradlew bootRun
```

Open:

```text
http://localhost:8080/hello
```

Expected:

```text
Hello from Kotlin
```

---

## 18. Development Workflow

For every lesson, follow this cycle:

```text
1. Modify Kotlin code
        ↓
2. Save
        ↓
3. Run application
        ↓
4. Call API
        ↓
5. Observe result
        ↓
6. Change code
        ↓
7. Run again
        ↓
8. Intentionally break something
        ↓
9. Read compiler/runtime error
        ↓
10. Fix it
```

This is the main learning method for this project.

---

## 19. Useful Gradle Commands

## Clean

```bash
./gradlew clean
```

## Compile

```bash
./gradlew compileKotlin
```

## Build

```bash
./gradlew build
```

## Clean + Build

```bash
./gradlew clean build
```

## Run tests

```bash
./gradlew test
```

## Start Spring Boot

```bash
./gradlew bootRun
```

## Show dependencies

```bash
./gradlew dependencies
```

## Show Gradle tasks

```bash
./gradlew tasks
```

---

## 20. Windows Git Bash vs PowerShell

Your VS Code screenshot shows Git Bash.

Therefore these commands work:

```bash
./gradlew clean build
./gradlew bootRun
./gradlew test
```

If you switch the VS Code terminal to PowerShell, use:

```powershell
.\gradlew.bat clean build
.\gradlew.bat bootRun
.\gradlew.bat test
```

Both are equivalent.

---

## 21. What We Will Learn

This project will progressively cover:

## Kotlin Fundamentals

```text
fun main()
val
var
type inference
explicit types
String
Int
Long
Double
Boolean
Char
String interpolation
multiline strings
```

## Null Safety

```text
String?
?.
?:
!!
let
nullable collections
nullable chaining
```

## Functions

```text
parameters
return types
expression body
default arguments
named arguments
```

## Control Flow

```text
if
when
for
ranges
while
```

## Object-Oriented Kotlin

```text
classes
constructors
properties
init
visibility
inheritance
open
override
interfaces
abstract classes
```

## Data Modeling

```text
data class
copy()
equals()
hashCode()
toString()
destructuring
```

## Kotlin Special Features

```text
object
companion object
const val
enum class
sealed class
```

## Collections

```text
List
MutableList
Set
Map
map
filter
find
firstOrNull
any
all
forEach
```

## Functional Kotlin

```text
lambda
higher-order functions
extension functions
```

## Scope Functions

```text
let
run
with
apply
also
```

## Error Handling

```text
try
catch
finally
throw
try as expression
```

## Generics

```text
<T>
generic classes
generic functions
```

## Initialization

```text
lateinit
lazy
```

## Spring + Kotlin

```text
Controller
Service
Repository
DTO
Validation
Exception handling
Dependency Injection
JPA
H2
REST APIs
```

## Coroutines

```text
suspend
launch
async
await
Dispatchers
withContext
```

## Flow

```text
Flow
emit
collect
StateFlow
MutableStateFlow
```

---

## 22. Recommended Learning Order

Do NOT jump directly into Coroutines.

Follow:

```text
01. Kotlin Basics
        ↓
02. Null Safety
        ↓
03. Functions
        ↓
04. Classes
        ↓
05. Inheritance / Interfaces
        ↓
06. Collections
        ↓
07. Lambdas
        ↓
08. Extension Functions
        ↓
09. Scope Functions
        ↓
10. Exceptions
        ↓
11. Generics
        ↓
12. Sealed / Enum / Object
        ↓
13. Spring REST API
        ↓
14. JPA + H2
        ↓
15. Coroutines
        ↓
16. Flow
        ↓
17. Real Application Architecture
```

---

## 23. Java → Kotlin Translation

Because this project is intended for a Java developer, constantly compare the two.

### Java

```java
final String name = "John";
```

### Kotlin

```kotlin
val name = "John"
```

---

### Java

```java
String name = null;
```

### Kotlin

```kotlin
var name: String? = null
```

---

### Java

```java
if (name != null) {
    System.out.println(name.length());
}
```

### Kotlin

```kotlin
name?.let {
    println(it.length)
}
```

---

### Java

```java
String result;

if (age >= 18) {
    result = "Adult";
} else {
    result = "Minor";
}
```

### Kotlin

```kotlin
val result = if (age >= 18) {
    "Adult"
} else {
    "Minor"
}
```

---

### Java

```java
switch (status) {
    case 200:
        return "Success";
    case 404:
        return "Not Found";
    default:
        return "Unknown";
}
```

### Kotlin

```kotlin
return when (status) {
    200 -> "Success"
    404 -> "Not Found"
    else -> "Unknown"
}
```

---

### Java POJO

```java
public class User {

    private Long id;
    private String name;

    public User(Long id, String name) {
        this.id = id;
        this.name = name;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```

### Kotlin

```kotlin
data class User(
    val id: Long,
    val name: String
)
```

---

## 24. First Milestone

The first milestone is NOT "finish Kotlin syntax."

The first milestone is:

```text
Spring Boot starts
        ↓
Kotlin application runs
        ↓
Controller works
        ↓
GET /hello works
        ↓
Create User data class
        ↓
GET /users works
        ↓
Use List / map / filter
        ↓
Introduce Service
        ↓
Introduce Repository
        ↓
Introduce H2
```

At that point you are already writing real Kotlin backend code.

---

## 25. Current Starting Point

After completing the initial setup, verify these commands in order:

```bash
java -version
```

```bash
./gradlew --version
```

```bash
./gradlew clean
```

```bash
./gradlew clean build
```

```bash
./gradlew bootRun
```

Then test:

```text
http://localhost:8080/hello
```

Expected:

```text
Hello from Kotlin
```

Only after this works should we proceed to the actual Kotlin learning exercises.

---

## 26. Important Rule

Do not blindly copy Kotlin code.

For each new concept:

```text
Read
 ↓
Run
 ↓
Modify
 ↓
Break
 ↓
Observe compiler error
 ↓
Fix
 ↓
Explain it in Java terms
```

The purpose of this project is to make Kotlin syntax become natural while working inside a real Spring Boot application.
