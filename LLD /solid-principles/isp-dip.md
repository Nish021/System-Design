# Interface Segregation Principle (ISP)
**ISP states that Clients should not be forced to depend on methods they do not use.** 
*Instead of having one large interface, split it into smaller, specific interfaces based on behavior.*

## Scenario
If we add a new behavior `swim()` and put it in the Bird class, non-swimming birds will be forced to implement it unnecessarily. (ISP Violation)

## ISP Compliant Solution (Split Behaviors)
Create separate interfaces for distinct capabilities:
Now each bird implements only what it needs:

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
│ + fly()       │       │ + Swim()      │        │ + fly()       │       │ + Swim()      │
│ + makeSound() │       │ + makeSound() │        │ + makeSound() │       │ + makeSound() │
└───────────────┘       └───────────────┘        └───────────────┘       └───────────────┘

          ┌───────────────────────────┐    ┌───────────────────────────┐
          │        <<interface>>      │    │        <<interface>>      │ 
          │          Flyable          │    │          Swimmable        │ 
          ├───────────────────────────┤    ├───────────────────────────┤ 
          │ + fly()                   │    │ + Swim()                  │
          └───────────────────────────┘    └───────────────────────────┘
```
``` java
class Sparrow extends Bird implements Flyable {
    public void fly(){ ... }
    public void makeSound(){ ... }
}

class Penguin extends Bird implements Swimmable {
    public void swim(){ ... }
    public void makeSound(){ ... }
}
```
---------

# Dependency Inversion principle (DIP)
 **DIP states that two concrete classes should not depend on each other directly — instead, they should communicate through an abstraction (interface/abstract class) in between them.**.

 *In short → Concrete classes must depend on interfaces, not on each other.*

 ### What does "Inversion" in DIP mean?

Normally in traditional code: 
```
High-level class → depends on → Low-level class
```

Example: Bird → depends on → PigeonSparrowFly (concrete depends on concrete as shown in example below) ❌
This is **normal dependency direction**, but it creates tight coupling.

DIP *inverts* this direction
Instead of high-level depending on low-level, **both depend on an abstraction**, and **low-level classes plug into it**.
```
High-level class → depends on → Abstraction ← implemented by ← Low-level class
```
## Scenario 

 There are some birds that fly in a similar way:

| Birds | Flying Style |
|-------|--------------|
| Pigeon, Sparrow | Same way of flying |
| Crow, Eagle | Same way of flying |

To avoid code duplication, we may try this solution:

```java
class PigeonSparrowFly { void fly(){...} }
class CrowEagleFly { void fly(){...} }

class Pigeon extends Bird {
    PigeonSparrowFly flyer = new PigeonSparrowFly();  // depends on concrete class ❌
}
class Crow extends Bird {
    CrowEagleFly flyer = new CrowEagleFly();  // depends on concrete class ❌
}
```

This design solves duplication but **violates DIP**.
because Pigeon and Crow subclasses depend directly on concrete fly classes.


## DIP Compliant Solution (Use Abstraction)
We introduce an interface `FlyBehaviour` to decouple both classes:

```java
interface FlyBehaviour { void fly(); }

class PSFly implements FlyBehaviour { public void fly(){ ... } }   // Pigeon, Sparrow
class CEFly implements FlyBehaviour { public void fly(){ ... } }   // Crow, Eagle

class Pigeon extends Bird {
    FlyBehaviour flyBehaviour;
    Pigeon(){ flyBehaviour = new PSFly(); } // depends on abstraction ✔
}

class Crow extends Bird {
    FlyBehaviour flyBehaviour;
    Crow(){ flyBehaviour = new CEFly(); } // abstraction ✔
}
```
if we want to changes the behaviour of Pigeon to `CEFly`, we have to instantiate the object with `CEFly` class

```java
class Pigeon extends Bird {
    FlyBehaviour flyBehaviour;
    Pigeon(){ flyBehaviour = new CEFly(); } // depends on abstraction ✔
}
```
Now **Bird no longer knows which flying class is used**, it only knows the interface.

---

### Why is this called "Inversion"?

Because **control of dependency is flipped**:

| Without DIP | With DIP (Inverted) |
|-------------|---------------------|
| Pigeon/Sparrow directly use a concrete fly class | Pigeon/Sparrow depend on FlyBehaviour abstraction |
| Bird subclasses depend on specific implementations | Bird hierarchy depends on behaviour abstraction only |
| Changing flying logic requires modifying bird subclasses | New fly logic can be added without changing bird classes |

```
❌ Before DIP (direct dependency)
Pigeon ----> PigeonSparrowFly (concrete)
Sparrow --> PigeonSparrowFly (concrete)

✔ After DIP (dependency inverted)
Pigeon ----> FlyBehaviour <---- PSFly
Sparrow --> FlyBehaviour <---- PSFly
```


We move the **power from concrete implementation → to abstraction**.
**High-level logic stops depending on details — details now depend on abstraction.**



