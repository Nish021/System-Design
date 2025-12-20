# Encapsulation

**Definition:**  
Encapsulation is one of the pillars of OOP. It **bundles data (fields) and behavior (methods) together** and **restricts direct access to the internal state** of an object.

It defines **“how the outside world can interact with the object”** — typically through public methods (getters/setters).  
Its main goal is **data protection and controlled access**, ensuring objects maintain a valid state.

Encapsulation supports abstraction by hiding the **internal implementation details** from the user.

---

## Why it is one of the pillars of OOP
- **Data Security:** Protects sensitive data from illegitimate access  
- **Controlled Access:** External code can interact only through defined methods  
- **Modularity & Maintainability:** Changes in internal implementation do not affect external code  
- **Supports Abstraction:** Internal complexity is hidden from the client  

---

## Real-life analogy
- **Abstract Idea:** A capsule of medicine has ingredients inside.  
- **Encapsulation:** You can take the medicine without knowing its internal chemical composition.  
- You interact with the **interface (pill)**, but internal details are hidden, preventing misuse.

---

## Good Example
```java
class BankAccount {
    private double balance; // data hidden

    public void deposit(double amount) { // controlled access
        if(amount > 0) balance += amount;
    }

    public void withdraw(double amount) {
        if(amount > 0 && amount <= balance) balance -= amount;
    }

    public double getBalance() {
        return balance;
    }
}
```

### Why it’s good:
- Data (balance) is private → cannot be accessed directly
- Methods control how data is modified → ensures valid state
- Hides internal details from the client → supports abstraction
  
---

## Bad Example
```java
class BankAccount {
    public double balance; // public data

    public void deposit(double amount) {
        balance += amount;
    }
}
```
### Why it’s bad:
- Data is public → anyone can modify it illegally
- No control over invalid modifications (e.g., negative deposits)
- Violates data security principle of encapsulation

---

## What happens without encapsulation
- External code can directly modify internal data → invalid or inconsistent state
- No control over access → increases bugs and system fragility
- Harder to maintain, test, and evolve the system

```java
BankAccount account = new BankAccount();
account.balance = -1000; // invalid state, no control
```
- With encapsulation, this would be prevented through private fields and validation methods

---

## Java Usage
- Private fields to hide data
- Public getter/setter methods to control access
- Can combine with abstract classes or interfaces to enforce controlled behavior

```java
class Employee {
    private String name;
    private double salary;

    public void setSalary(double salary) {
        if(salary > 0) this.salary = salary;
    }

    public double getSalary() {
        return salary;
    }
}
```

- Private Data: Hidden from outside
- Public Methods: Control access to data
- Internal Implementation: Can change without affecting external code

---

















