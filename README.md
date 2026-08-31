# kotlin-learning

A Spring Boot 4 + Kotlin scratch project for learning. Web MVC, Spring Data JPA, Bean Validation and an in-memory H2 database are wired up; there are no controllers or entities yet.

## Requirements

- **JDK 21** — the build declares a Java 21 toolchain, so Gradle will download a matching JDK if the one on your `PATH` is a different version.
- Nothing else. The Gradle wrapper (`gradlew`) pulls Gradle 9.7.1 itself; do not install Gradle separately.

## Run the app

```powershell
.\gradlew.bat bootRun
```

```bash
./gradlew bootRun            # macOS / Linux
```

The app starts on **http://localhost:8080**. Startup is done when the log prints:

```
Started LearningApplicationKt in 3.4 seconds
```

Stop it with `Ctrl+C`.

> Hitting `http://localhost:8080/` returns a 404 error page — that is expected, since no controller is mapped to `/` yet.

`spring-boot-devtools` is on the classpath, so the app restarts automatically when you recompile. In IntelliJ that means **Build → Recompile** (or Ctrl+F9); from the terminal, run `.\gradlew.bat build` in a second window while `bootRun` keeps running.

## Run the tests

```powershell
.\gradlew.bat test
```

`LearningApplicationTests.contextLoads()` boots the full Spring context, so it fails fast if a bean or configuration is broken.

## Build a jar

```powershell
.\gradlew.bat build
java -jar build\libs\kotlin-learning-0.0.1-SNAPSHOT.jar
```

`build` also runs the tests. Two jars are produced: the executable `kotlin-learning-0.0.1-SNAPSHOT.jar` (use this one) and `-plain.jar`, which contains only this project's classes.

## H2 database console

The database is in-memory and is recreated empty on every restart. The console lives at **http://localhost:8080/h2-console**.

By default the JDBC URL is a random UUID, printed at startup on the `H2ConsoleAutoConfiguration` line — you have to copy it into the console's login form each time. To pin it to a stable name instead, add to `src/main/resources/application.yaml`:

```yaml
spring:
  datasource:
    url: jdbc:h2:mem:learningdb
    username: sa
    password:
```

Then log in with JDBC URL `jdbc:h2:mem:learningdb`, user `sa`, and an empty password.

## Layout

```
src/main/kotlin/com/kotlin/learning/LearningApplication.kt   entry point (top-level `fun main`)
src/main/resources/application.yaml                          configuration
src/test/kotlin/com/kotlin/learning/LearningApplicationTests.kt
```

The Kotlin `plugin.spring` and `plugin.jpa` compiler plugins in `build.gradle.kts` are what let Spring and Hibernate work with Kotlin's final-by-default classes — `plugin.spring` un-finalizes `@Configuration`/`@SpringBootApplication` classes for CGLIB proxying, and `plugin.jpa` does the same for `@Entity` classes and generates the no-arg constructor JPA requires. Keep them when you start adding entities.
