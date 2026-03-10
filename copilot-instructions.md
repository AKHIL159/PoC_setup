# 🧑‍💻 Combined Coding Standards and NPE Handling Guide (Spring Boot - Java)

This document consolidates coding standards, null safety practices, and security precautions for Java Spring Boot projects. It also integrates GitHub Copilot guidance to produce production-ready, secure code.

---

### ✅ General Principles

- Follow **SOLID**, **DRY**, **KISS**, and **YAGNI** principles.
- Respect clear **separation of concerns** across layers: Controller, Service, Repository, DTO, and Entity.
- Prefer **Dependency Injection** over manual instantiation.
- Avoid logic in controllers or repositories—keep it in the service layer.
- Use **custom exceptions** for clarity and maintainability.
- Annotate service methods that modify data with `@Transactional`.

---

### 🛑 Private Static Final Usage

Use `private static final` for:
- Constants (e.g., error codes, default settings).
- Reusable values that should not change at runtime.
- Utility objects (e.g., `Logger` instances).

Example:
```java
private static final String ERROR_MESSAGE = "Invalid Request";
private static final Logger LOGGER = LoggerFactory.getLogger(MyService.class);
```

---

### 📛 Naming Conventions

| Type           | Convention          | Example                         |
|----------------|---------------------|----------------------------------|
| Class          | PascalCase          | `UserService`, `OrderController`|
| Method         | camelCase           | `getUserDetails()`              |
| Constant       | UPPER_SNAKE_CASE    | `MAX_RETRIES`, `ERROR_CODE_404` |
| Variable/Field | camelCase           | `totalAmount`                   |
| Package        | lowercase.with.dots | `com.example.orders.controller` |

---

### 🧱 Code Formatting

- Indentation: 4 spaces
- Max line: 120 characters
- Use `{}` for all blocks
- Place annotations on their own line above declaration

---

### 📂 Project Structure

```
src/main/java/com/example/project/
├── config/         # Configuration (e.g., CORS, Swagger)
├── controller/     # REST endpoints
├── dto/            # Data Transfer Objects
├── entity/         # JPA Entities
├── exception/      # Custom exceptions
├── repository/     # Spring JPA interfaces
├── security/       # Spring Security configuration
├── service/        # Business logic
└── util/           # Utility classes
```

---

### 📚 Documentation and Exception Templates

#### JavaDoc for Public APIs:
```java
/**
 * Gets user by ID.
 * @param userId the user ID
 * @return UserDto
 * @throws UserNotFoundException if not found
 */
```

#### Automatic Exception Template:
```java
public class CustomException extends RuntimeException {
    public CustomException(String message) {
        super(message);
    }
}
```

---

### 🔒 Security Best Practices

- Use `@Valid`, `@NotNull`, and `@Email` for request validation.
- Use `BCryptPasswordEncoder` for passwords.
- Never log or expose credentials or tokens.
- Store credentials in secure storage (e.g., Azure Key Vault, env vars).
- ❗ **Never hardcode secrets** or commit them to Git.
- **Reminder**: Credentials should **never** be committed under IPC or any other modules.
- Add `.env`, `.secrets`, and `.credentials` to `.gitignore`.

---


# 🚫 NullPointerException Handling Guidelines (Java - Spring Boot)

NullPointerExceptions (NPEs) are one of the most common and critical runtime issues in Java applications. This guide defines best practices to proactively avoid and defensively handle NPEs, especially when working with Spring Boot and modern Java features like Stream API.



## ✅ Best Practices to Avoid NPE

### 1. Use `Optional` to Avoid Null Returns
```java
public Optional<User> findById(Long id) {
    return userRepository.findById(id);
}
```

### 2. Use `Objects.requireNonNull()` or `Objects.nonNull()`
```java
Objects.requireNonNull(user, "User must not be null");
```

### 3. Avoid Calling Methods on Possibly Null Objects
Always add a null check before accessing object methods or fields.

### 4. Use `orElse`, `orElseThrow`, or `orElseGet` with `Optional`
```java
User user = optionalUser.orElseThrow(() -> new UserNotFoundException("User not found"));
```

### 5. Default Empty Collections Instead of Null
```java
List<String> safeList = list != null ? list : Collections.emptyList();
```

Or use:
```java
List<String> safeList = Optional.ofNullable(list).orElseGet(ArrayList::new);
```


## ☕ Java Stream API: Defensive Patterns

### 1. Filter Out Nulls in Stream
```java
list.stream()
    .filter(Objects::nonNull)
    .forEach(System.out::println);
```

### 2. Avoid `.get()` on Optional without isPresent()
```java
optional.ifPresent(user -> doSomething(user));
```



## 🛡️ Service & Controller Layer Practices

- Never assume repository/service methods will return non-null.
- Wrap `findById()` and similar calls with `Optional` or null checks.
- Validate request bodies with `@Valid`, and annotate fields with `@NotNull`.



## 🧪 Testing for NPE Scenarios

- Write negative unit tests to simulate null inputs.
- Use `assertThrows(NullPointerException.class, ...)` where needed.
- Use tools like **SpotBugs** or **SonarLint** to detect possible null dereferences.



## 🚫 Don’ts

- ❌ Don’t return `null` from controller or service methods.
- ❌ Don’t use `.get()` on Optional without presence check.
- ❌ Don’t assign or inject nullable beans without default fallback.



By applying these guidelines, you ensure greater resilience, fewer production bugs, and better alignment with modern Java development standards.


---

### 🧠 GitHub Copilot Instructions

#### Copilot Must:
- Suggest well-structured Java code
- Place business logic in the service layer
- Use `Optional`, `Objects.nonNull`, `orElseThrow()`
- Generate tests using JUnit 5 and Mockito
- Include JavaDoc in public methods

#### Copilot Must Not:
- Return raw nulls
- Hardcode sensitive data
- Add logic in controller layer
- Skip validation or exception handling

---

### 🧪 Testing Standards

- JUnit 5 + Mockito
- Test class naming: `ClassNameTest`
- Use `@SpringBootTest` for integration testing
- Include NPE cases in negative testing

---

### ✅ Commit Message Format

Follow Conventional Commits:
```text
<type>(scope): <description>
```

Examples:
```text
feat(user): add login feature
fix(order): correct discount calculation
docs(security): update password policy
```

---

### ✅ Credentials and IPC

- **Instruction:** Never include credentials in IPC or configuration files.
- Enforce security scanning (e.g., Git hooks, GitHub secrets detector).
- Educate developers with warning comments like:
  ```java
  // 🚫 Do NOT add credentials or access tokens here.
  ```