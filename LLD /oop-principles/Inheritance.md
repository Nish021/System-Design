# Inheritance

## Definition
**Inheritance** is an OOP mechanism where a class (child/subclass) **inherits all the accessible properties (data and behavior)** of another class (parent/superclass).

It establishes an **“is-a” relationship**, allowing code reuse and extensibility.

---

## Why it is one of the pillars of OOP
- **Code Reusability:** Common behavior is written once and reused by subclasses  
- **Extensibility:** New functionality can be added without modifying existing code  
- **Polymorphism Support:** Enables runtime method overriding  
- **Maintainability:** Changes in base behavior propagate safely to subclasses  

---

## Core Idea / What it Answers
- Inheritance answers the question:  
  **“What can be reused or extended from an existing class?”**

---

## Accessible Properties in Java

A subclass inherits **only accessible members** of the parent class.

| Access Modifier | Inherited? | Notes |
|-----------------|-----------|------|
| `private`       | ❌ No     | Not accessible outside the class |
| `default`       | ✅ Yes    | Only within the same package |
| `protected`     | ✅ Yes    | Accessible in subclass |
| `public`        | ✅ Yes    | Accessible everywhere |

---

## Real-life Analogy
- **Parent Class:** Vehicle  
  - speed  
  - start()  
- **Child Class:** Car  
  - Inherits speed and start()  
  - Adds drive(), airConditioning()

A **Car is a Vehicle**, but a Vehicle is not necessarily a Car.

---

## Good Example
```java
class Vehicle {
    protected int speed;

    public void start() {
        System.out.println("Vehicle started");
    }
}

class Car extends Vehicle {
    public void drive() {
        System.out.println("Driving at speed " + speed);
    }
}
```

### Why it’s good:
- Reuses code: start() is reused without duplication
- Extensible: Car adds its own behavior
- Respects access control: Uses protected correctly
- Clear is-a relationship: Car is a Vehicle

---

## Bad Example
```java
class Car {
    int speed;

    public void start() {
        System.out.println("Car started");
    }
}
```

### Why it’s bad:
- Code duplication: Similar logic repeated across classes
- No reuse: Cannot leverage shared behavior
- Hard to maintain: Changes must be made in multiple places

---

## What happens without inheritance
- Repeated code across multiple classes
- Difficult to maintain shared behavior
- Harder to extend systems safely
- No natural support for polymorphism

---

## Java Usage
- Implemented using the extends keyword
- Java supports single inheritance for classes
- Supports method overriding

```java
  class Animal {
    void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("Bark");
    }
}
```
## Inheritance and Other OOP Pillars
- Abstraction: Inherited abstract methods define required behavior
- Encapsulation: Access modifiers control what is inherited
- Polymorphism: Child overrides parent behavior at runtime

---

## 🚫 When NOT to Use Inheritance

- No clear **is-a** relationship  
- Only goal is code reuse  
- Parent behavior changes frequently  
- Inheritance hierarchy is deep  
- Logic is procedural or conditional  

---

## ✅ Rule of Thumb

> **Prefer composition over inheritance unless a clear, stable “is-a” relationship exists.**

---

## Key Takeaway
> **Inheritance should be used carefully; misuse leads to tight coupling, incorrect domain modeling, and fragile systems.**
