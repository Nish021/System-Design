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


