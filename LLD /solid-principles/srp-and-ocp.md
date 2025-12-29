# Single Responsibility Principle (SRP)

**SRP states that Every Code unit in your codebase should have exactly one well defined responsibility.**
*Exactly **One Reason** to change.*

A responsibility means a **business reason to change**,  not a method, not a feature.

Lets understand this principle by designing a bird.

Basic requirement of a bird can include its type, weight, colour, beakType etc. and behaviour can includes fly, eat, makeSound etc. 

At first glance, this looks sufficient.
But the question is: ***Is this Bird class enough to represent all kinds of birds?***

```
┌───────────────────────────┐    
│           Bird            │    fly() and type is dependent on each other.
├───────────────────────────┤    if type == "Eagle" → fly high
│ - type      : String      │    if type == "Sparrow" → fly short distance
│ - size      : String      │    if type == "Penguin" → cannot fly
│ - weight    : double      │
│ - beakType  : String      │    If the fly() behavior is implemented inside Bird. 
│ - colour    : String      │    Based on bird type, then Bird has multiple reasons to change.
├───────────────────────────┤    Any change in flying logic of a specific bird. Forces modification of the Bird class.
│ + fly()                   │    
│ + eat()                   │    Bird is responsible for:
│ + makeSound()             │    1. Bird identity and properties
└───────────────────────────┘    2. Flying rules of all bird types

```
|Change Scenario |Reason Bird Changes |
|---------------|--------------------|
| New bird type added | Update fly() |
| Flying behavior of a bird changes | Modify fly() |
| A bird becomes non-flying | Modify fly() |
| Bug in one bird’s flying logic | Modify fly() |

## Disadvantages :
- Voilates SRP.
- Bad for readability (if 1000 types of bird are there).
- Testing is difficult
- Not Reusable code
- Difficult for multiple developers to work on this class parallely.

## How to identify the voilation of SRP :
- Multiple if/else used in method. SRP likely voilates.
- Monster methods, methods which are doing a lot more that what their names is suggesting to do. ( *Break into smaller methods* ).
- Utility Classes, these packages end up being junkyard for a lot of unrelated code. ( *Name them better* ).

*Design is subjective. Every design in software has some trade off. Its not practical if we follow all the principle to its core.*

-----

# Open Close Principle (OCP)

**OCP states that A class should be open for extension but closed for modification.**
A well-designed codebase should make it **easy to add new features**.

Adding new features should require **very little or ideally no changes**
to the existing, already written and tested codebase.

New functionality should mostly be introduced by:
- Adding new classes
- Adding new methods
- Extending existing abstractions

and **not by modifying existing code**.

## Benefits of Open–Closed Principle (OCP)
- **No Regression** – Existing stable code remains untouched, reducing chances of breaking already working features.  
- **Easy Testing** – New features or classes can be tested independently without retesting entire existing modules.

## Why Bird Design Violates OCP
In the Bird design, behavior such as `fly()` is implemented inside the `Bird` class and varies based on the bird `type`.
- Adding a **new bird type** requires modifying the existing `fly()` logic  
- Changing the **flying behavior of one bird** forces changes in the same class  
- Existing, stable, and tested code must be edited for every new variation  

This means the `Bird` class is **open for modification instead of extension**.

-----

# SRP and OCP Compilant Design 
*SRP and OCP often go hand in hand.*

Create **Bird as an abstract base class** containing only common attributes and behavior definitions.
- Declare `fly()`, `eat()`, and `makeSound()` as **abstract methods**.
- Create **different subclasses for each type of bird**, and let each class implement its own flying behavior.
- Now, new bird types can be added by **creating new subclasses** without modifying existing ones.

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
                               │ + fly() : abstract        │
                               │ + eat() : default         │
                               │ + makeSound() : abstract  │
                               └─────────────┬─────────────┘
                                             │
         ┌───────────────────────┬───────────┴───────────┬───────────────────────┐
         │                       │                       │                       │
 ┌───────────────┐       ┌───────────────┐        ┌───────────────┐       ┌───────────────┐
 │   Sparrow     │       │   Penguin     │        │    Eagle      │       │   Ostrich     │
 ├───────────────┤       ├───────────────┤        ├───────────────┤       ├───────────────┤
 │ + fly()       │       │ + fly()       │        │ + fly()       │       │ + fly()       │
 │ + makeSound() │       │ + makeSound() │        │ + makeSound() │       │ + makeSound() │
 └───────────────┘       └───────────────┘        └───────────────┘       └───────────────┘

```
### Why This Design Follows SRP & OCP

| Principle | How it's satisfied |
|----------|-------------------|
| **SRP**       | The abstract `Bird` class has a single responsibility: holding common attributes. Each subclass (e.g., Sparrow, Penguin, Eagle, Ostrich) has one responsibility: implementing behavior specific to that bird type. |
| **OCP** | New bird types are added by creating new classes extending Bird class, not modifying Bird itself. |

*We are using abstract class instead of an interface because an abstract class represents the **Entity** (Bird) which has common attributes and optionally common behavior like `eat()`.
Interfaces represent **behavior** only (like `fly()` or `makeSound()`), which can be implemented by different bird types.*




