# Liskov's Substitution Principle (LSP)
**LSP states that objects of a child class should be substitutable for objects of the parent class without affecting the correctness of the program or requiring changes in the client code.**

No **SPECIAL TREATMENT** for any child class. *Methods using the parent class should work **correctly with any subclass**.*

### Scenario
We want to create a new class `Penguin` that extends the `Bird` class.  
Problem: Penguins **cannot fly**. This will violates LSP :
- The `Bird` class has a method `fly()` that **all birds are expected to implement**.
- If we substitute `Penguin` wherever a `Bird` is expected, the `fly()` method will behave unexpectedly.

### How We Might Handle It (All Bad Options)

1. **Leave `fly()` method blank**  
   - Violates client expectations; calling `fly()` does nothing.
2. **Throw an exception**  
   - Breaks code that assumes every `Bird` can fly; leads to runtime errors.
3. **Logging a message**  
   - Still requires special handling for `Penguin` in client code.
  
*Treating penguins differently **breaks polymorphism**.*

```java
Bird b = new Penguin();
b.eat();          // ✅ Works fine
b.makeSound();    // ✅ Works fine
b.fly();          // ❌ Problem: Penguins can't fly
```

### LSP-Compliant Design

There can be multiple designs to which can prevent violating LSP.

#### Solution 1
We can remove `fly()` method from Bird class and instead have it in child classes that can fly.
**Problem** : We will not be able to know the birds which can fly. 

#### Solution 2
Create two abstract classes extending Bird:
 - FlyableBird → for birds that can fly
 - NonFlyableBird → for birds that cannot fly
```
Bird
 ├─ FlyableBird
 │    ├─ Crow
 │    └─ Parrot
 └─ NonFlyableBird
      ├─ Penguin
      └─ Ostrich
```
**Problem** : If another behavior (e.g., swim()) is introduced, you may end up with multiple combinations like:
Flyable-Swimming, NonFlyable-Swimming, Flyable-NonSwimming, NonFlyable-NonSwimming. This makes it difficult to maintain and query information (e.g., all birds that can fly).

#### Solution 3 (Ideal Solution)
 Since `fly()` is a one of the behaviour of bird, instead of keep in bird class as abstract method.
 Extract `fly()` into a separate interface `Flyable`. 
 ```
                               ┌───────────────────────────┐
                               │        <<abstract>>       │
                               │           Bird            │
                               ├───────────────────────────┤
                               │ - type                    │
                               │ - size                    │
                               │ - weight                  │
                               │ - beakType                │
                               │ - colour                  │
                               ├───────────────────────────┤
                               │ + eat() : default         │
                               │ + makeSound() : abstract  │
                               └─────────────┬─────────────┘
                                             │
         ┌───────────────────────┬───────────┴───────────┬───────────────────────┐
         │                       │                       │                       │            
 ┌───────────────┐       ┌───────────────┐        ┌───────────────┐       ┌───────────────┐
 │   Sparrow     │       │   Penguin     │        │    Eagle      │       │   Ostrich     │    
 ├───────────────┤       ├───────────────┤        ├───────────────┤       ├───────────────┤
 │ + fly()       │       │  No fly       │        │ + fly()       │       │  No fly       │
 │ + makeSound() │       │ + makeSound() │        │ + makeSound() │       │ + makeSound() │
 └───────────────┘       └───────────────┘        └───────────────┘       └───────────────┘

           ┌───────────────────────────┐
           │        <<interface>>      │    - Only flying birds (Sparrow, Eagle) implement `Flyable`
           │          Flyable          │    - Non-flying birds (Penguin, Ostrich) do not implement `Flyable`
           ├───────────────────────────┤    - Bird class handles **common attributes and behaviors** (eat, makeSound)
           │ + fly()                   │
           └───────────────────────────┘
```
```java
abstract class Bird {
    default void eat(){
      // common eating behavior
    }
    abstract void makeSound();
}

interface Flyable {
    void fly();
}

class Sparrow extends Bird implements Flyable {
    void fly() { ... }
    void makeSound() { ... } 
}

class Penguin extends Bird {
    // No fly() method 
    void makeSound() { ... }
}

Bird b = new Penguin(); // right
Bird b = new Sparrow(); // right
```
 - Any Bird subclass can substitute Bird without breaking code → LSP satisfied.
 - Flying behavior is optional via interface → avoids Penguin “breaking” Bird expectations.

SRP + OCP + LSP are all preserved.








