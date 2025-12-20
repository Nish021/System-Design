# Abstraction

A word **Abstract** means existing only as an idea not a physical thing. **Abstraction** is the process of representing that idea by defining its essential behavior
without committing to its implementation.

Abstraction is core idea/principle of oops and Inheritance, Polymorphism and Encapsulation supports this idea.
- Encapsulation uses abstraction to hide data
- Polymorphism relies on abstract interfaces
- Inheritance + abstraction allows child classes to follow contracts without exposing base complexity

## ✅  Why it is one of the pillars of oops
- Reduce complexity : users interact with the interfaces not internal workings.
- Enable loose coupling : Components depend on behaviors, not concrete classes
- Allow implementations to change safely
  
## ✅  Essential Behavior
Essential behavior describes what an object guarantees to do,
independent of how it is implemented.

## ✅ Real-life analogy
- **Abstract:** An idea of sending your money to your friend online.
- **Abstraction:** GPay, PhonePe, Paytm, etc.  
  These apps let you perform the idea without knowing or worrying about internal steps such as bank APIs, UPI protocols, or OTP verification.

## ✅ What happens without abstraction

### Conceptually
- Users or other system parts must know **all internal details**.
- Everything becomes **tightly coupled**.
- Changing implementation anywhere breaks all dependent modules.
- 
**With abstraction:**  
- You use GPay or PhonePe to send money.  
- You don’t care **how** it works internally.

**Without abstraction:**  
- Every app/user would need to know bank APIs, authentication, transaction routing, etc.  
- If the bank changes API → **all apps break**.  
- Every new app is hard to build → reinventing the wheel.

## ✅ Good Example
```java
interface Validator<T> {
    ValidationResult validate(T input);
}
```

### Why it’s good:
- Focuses on essential behavior: The only promise is “I can validate this input.”
- Flexible & reusable: Works for any object type T (UserRequest, ReportData, etc.)
- Implementation-hidden: How validation is done is completely hidden.
- Single Responsibility: One method does one thing — validate

## ✅  Bad Example
```java
interface Validator<T> {
    ValidationResult validateEmail(String email);
    ValidationResult validateName(String name);
}
```

### Why it’s bad:
- Leaks implementation details: The interface exposes specific fields (email, name).
- Not reusable: Can’t easily use it for other objects.
- Violates SRP: Now handles multiple specific validations.
- Maintenance nightmare: Adding new fields breaks all implementations.

## ✅ JAVA USAGE

- **Abstract Class** : A class that can have both abstract methods (without implementation) and concrete methods (with implementation).
Used to share common behavior (implementation) and enforce essential methods in subclasses.
Supports single inheritance and shared state among closely related classes.

- **Interface** :  A contract or pure abstraction that defines what methods a class must implement.
Focuses on behavior, not implementation.
Enables loose coupling, multiple inheritance, and flexibility for unrelated classes.

### Abstract Class vs Interface

In Java, both **abstract classes** and **interfaces** provide abstraction, but they are used in slightly different scenarios.

| Feature                  | Abstract Class                          | Interface                                  |
|---------------------------|----------------------------------------|-------------------------------------------|
| **Purpose**               | Partial abstraction + common behavior  | Pure abstraction / contract               |
| **Methods**               | Can have abstract + concrete methods   | Abstract by default (Java 8+: default methods allowed) |
| **Fields / Variables**    | Can have instance variables            | Can only have constants (public static final) |
| **Multiple inheritance**  | Not allowed                             | Allowed (can implement multiple interfaces) |
| **Use case**              | When classes share **common behavior** | When classes only need to follow a **contract** or API |
| **Example**               | `abstract class Payment { ... }`       | `interface PaymentService { ... }`        |

### ✅ When to Prefer Abstract Class
- **Scenario 1: Shared behavior**  
  - Multiple payment types share common logging or validation  
  - Example: `abstract class Payment` with concrete `logPayment()` method  
- **Scenario 2: Fields or state**  
  - You need instance variables that subclasses will use  
  - Example: `protected double balance` in a bank account base class  
- **Scenario 3: Default implementation for some methods**  
  - Some methods are same for all subclasses, others are abstract  
  - Example: `calculateFee()` is abstract, `logTransaction()` is concrete  

### ✅ When to Prefer Interface
- **Scenario 1: Pure contract / multiple implementations**  
  - Different classes implement same behavior differently  
  - Example: `interface PaymentService` → implemented by `GPay`, `PhonePe`, `Paytm`  
- **Scenario 2: Multiple inheritance needed**  
  - A class can implement multiple interfaces  
  - Example: `class SmartCardPayment implements PaymentService, Auditable`  
- **Scenario 3: Loosely coupled system / DI friendly**  
  - Spring Boot / dependency injection relies on interfaces to inject dependencies  
  - Example: `ReportService` interface → injected into `ReportController`  

**Tip:**  
- Use **abstract class** if there’s shared behavior to reuse.  
- Use **interface** if you only need to define what the object can do, not how.


