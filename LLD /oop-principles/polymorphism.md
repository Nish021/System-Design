# Polymorphism

## Definition
**Polymorphism** is an OOP mechanism that allows **the same interface or parent type to represent multiple implementations**, where the behavior executed depends on the actual object at runtime.

In simple terms, **one reference, many forms**.

---

## Why it is one of the pillars of OOP
- **Flexibility:** Same code works with different object types  
- **Loose coupling:** Code depends on abstractions, not implementations  
- **Extensibility:** New behavior can be added without changing existing code  
- **Runtime behavior selection:** Correct method is chosen dynamically  

---

## Core Idea / What it Answers
- Polymorphism answers the question:  
  **“Which behavior should execute for this object?”**

The answer depends on the **actual object**, not the reference type.

---

## Types of Polymorphism in Java

### 1️⃣ Compile-time Polymorphism
- Achieved using **method overloading**
- Method resolution happens at compile time
- Same method name, different parameters

---

### 2️⃣ Runtime Polymorphism
- Achieved using **method overriding**
- Method resolution happens at runtime
- Enabled through inheritance and abstraction

---

## Real-life Analogy
- **Role:** Remote control  
- **Behavior:** Power button  

The same button:
- Turns ON a TV  
- Turns ON an AC  
- Turns ON a projector  

The action performed depends on the **actual device**, not the button itself.

---

## Good Use Case

### Scenario
A system needs to process different types of payments without changing core logic.

### Why polymorphism works well
- Payment handling code depends on a **common abstraction**
- New payment methods can be added easily
- No conditional logic required

---

## Bad Use Case (Without Polymorphism)

### Scenario
Behavior is decided using large conditional blocks.

### Why this is bad
- Violates Open–Closed Principle  
- Difficult to extend  
- Hard to maintain  
- High risk of bugs  

---

## What Happens Without Polymorphism
- Code becomes filled with `if-else` or `switch` statements  
- Adding new behavior requires modifying existing code  
- System becomes rigid and tightly coupled  

---

## Relationship with Other OOP Pillars

- **Abstraction:** Defines the common behavior that enables polymorphism  
- **Inheritance:** Allows child classes to provide specialized behavior  
- **Encapsulation:** Hides internal behavior behind method calls  

Polymorphism **exists because of abstraction and inheritance**.

---

## Polymorphism vs Inheritance (Important Distinction)

- **Inheritance**: Code reuse and hierarchy  
- **Polymorphism**: Behavior variation at runtime  

Inheritance enables polymorphism, but **they are not the same thing**.

---

## Key Takeaway
> **Polymorphism allows the same parent reference or interface to exhibit different behaviors based on the actual object type at runtime, making systems flexible and extensible.**
