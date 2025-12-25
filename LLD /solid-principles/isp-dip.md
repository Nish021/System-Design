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

 *In short → high-level modules should not depend on low-level modules. Both should depend on abstractions.*

 ### What does "Inversion" in DIP mean?

Normally in traditional code: 
```
High-level class → depends on → Low-level class
```

Example: Pigeon (low-level bird class) → depends on → PigeonSparrowFly concrete class ❌ (concrete depends on concrete as shown in example below) ❌
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
   Pigeon(FlyBehaviour flyBehaviour){  
        this.flyBehaviour = flyBehaviour; 
    }  // depends on abstraction ✔
}

class Crow extends Bird {
    FlyBehaviour flyBehaviour;
    Crow (FlyBehaviour flyBehaviour){  
        this.flyBehaviour = flyBehaviour; // Constructor Injection : Concrete implementation is injected from outside
    } 
}
```
if we want to changes the behaviour of Pigeon to `CEFly`, we have to instantiate the object with `CEFly` class

```java
Bird pigeon = new Pigeon(new CEFly());
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

## Dependency Injection
Dependency Injection is a design technique where an object's required dependencies are provided ("injected") from outside rather than the object creating them itself.
It is a way to follow **DIP** in practice.


```java
class Pigeon extends Bird {
     FlyBehaviour flyBehaviour = new PSFly(); // Violates DIP, Tightly Coupled
}

class Pigeon extends Bird {
     FlyBehaviour flyBehaviour; 
     Pigeon () {
      flyBehaviour = new PSFly(); // Violates DIP, Tightly Coupled
     }
}

//Both still violate DIP because the class itself decides which implementation to use, not the external world.
```

```java
class Pigeon extends Bird {
    Pigeon(FlyBehaviour flyBehaviour){  
        this.flyBehaviour = flyBehaviour; // Constructor Injection : Concrete implementation is injected from outside
    } 
}

//while Creating object
Bird pigeon = new Pigeon(new PSFly());  // Inject behaviour at runtime
Bird sparrow = new Sparrow(new PSFly());
Bird eagle = new Eagle(new CEFly());

```
```java
class Pigeon extends Bird {
    FlyBehaviour flyBehaviour;      // depends on abstraction

    // Setter Injection
    void setFlyBehaviour(FlyBehaviour flyBehaviour){
        this.flyBehaviour = flyBehaviour;
    }
}

Pigeon pigeon = new Pigeon();
pigeon.setFlyBehaviour(new PSFly());    // behaviour injected later ✔
```
Setter Injection allows changing FlyBehaviour even after the object is created.  
Useful when behaviours need to switch dynamically (runtime strategy change).

| Use Setter Injection when                  | Use Constructor Injection when             |
| ------------------------------------------ | ------------------------------------------ |
| Behaviour may change after object creation | Behaviour is mandatory for object creation |
| You want runtime swap flexibility          | You want immutability once set             |
| Optional dependency                        | Required dependency                        |



