# UML(Unified Modeling Language) Diagrams
UML is a blueprint of software before building it.
A **UML diagram** represents different aspects of a system using symbols, shapes, and relationships instead of code.

## Types of UML Diagram
- Structural : Show the static structure of a system (what exists in the system).
- Behavioural : Show how the system behaves over time.

One of the main type of Structural UML Diagram is **Class Diagram** and for Behavioural Diagram is **Sequence Diagram**.

## Class Diagram
A Class Diagram is a structural UML diagram that shows:
- Classes in the system
- Attributes (data)
- Methods (behavior)
- Relationships between classes
👉 It answers: “What are the main building blocks of the system?

```
-------------------
|   Class Name   |
|       CAR      |
-------------------
|  Attributes     |
|  - model        |
|  - year         |
|  - color        |
-------------------
|  Methods        |
|  + start()      |
|  + stop()       |
|  + drive()      |
-------------------
```
| Symbol | Meaning |
|------|--------|
| `+` | Public |
| `-` | Private |
| `#` | Protected |
| `~` | Default |


👉 Supports **encapsulation** by controlling access

## Relation Between Classes
### 1. Inheritance (IS-A Relationship)

- One class inherits from another  
- Represents **generalization**  
- Promotes **reusability**

**Example:**  
- `Account` → parent class  
- `SavingsAccount`, `CurrentAccount` → child classes

👉 *SavingsAccount IS-A Account*

**Inheritance in Class Diagram:**
 - Represented using: Solid line. Hollow (empty) triangle pointing to the parent class

SavingsAccount ─────▷ Account

---

### 2. Association (USES-A Relationship)

- One class uses another class  
- Weak relationship  
- No ownership involved
- Both classes can exist independently

**Example:**  
- A `Doctor` treats a `Patient` (Doctor and Patient can exist without each other)
- A `Teacher` teaches a `Student` (Teacher and Student can exist without each other)

**Association in Class Diagram:**
 
 Shown using a solid line between classes
- **Unidirectional (one-way)** : One class knows about another (Order → Customer)
- **Bidirectional (two-way)** : Both classes know each other (Teacher ↔ Student)

---

### 3. Aggregation (HAS-A – Weak Ownership)

- One class contains another, but does not control its lifecycle. (can exist independently once created.)
- Child object can exist without parent  

**Example:**  
- `Bank` has `Customer`  
- Customers can exist even if the Bank is removed
- `Library` has `Books`
- (Books can exist even if the library is closed)
- `Team` has `Players`
- (Players exist even if the team is dissolved)

👉 This independence makes it aggregation, not composition.

**Aggregation in Class Diagram:**
 - Represented using: Hollow (empty) diamond at the parent side. Solid line connecting to the child.

 Bank ◇──────── Customer

---

### 4. Composition (HAS-A – Strong Ownership)

- Strong ownership relationship
- One class owns another class and controls its lifecycle.
- Child cannot exist without parent. If parent is destroyed, child is destroyed.

**Example:**  
- `Order` has `OrderItem`  
- If `Order` is deleted, `OrderItem` is also deleted
- `House` has `Rooms`
- (`Rooms` do not exist without the `house`)
- `Human` has `Heart`
 (`Heart` cannot exist independently)

👉 This dependency makes it composition, not aggregation.
👉 Preferred over deep inheritance in many designs

**Composition in Class Diagram:**
 - Represented using: Filled (black) diamond at the parent side. Solid line connecting to the child.

 Order ◆──────── OrderItem

---

### 5. Dependency (DEPENDS-ON Relationship)

- Temporary relationship, No Ownership
- One class uses another only for a short time
- Occurs during method execution  

**Example:**  
- `PrinterService` uses `PDFGenerator` to print a report
- `PaymentService` uses `NotificationService` to send alerts
- `ReportGenerator` uses `EmailService` to send reports

👉 Once the task is done, the dependency ends.

**Dependency in Class Diagram:**
 - Represented using: Dashed arrow. Arrow points from dependent class to the class it depends on

ReportGenerator - - - > EmailService

---

## Example: Banking System (Conceptual)

### Classes
- Customer  
- Account  
- Transaction  

### Relationships
- Customer **has** Account → Aggregation (once customer has created account, even if customer info is deleted, but account will still be there for audit purpose etc)
- Account **has** Transaction → Composition ( without account, there can be no transaction, shows strong relationship )
- SavingsAccount **is** Account → Inheritance
- 
---

## How Class Diagram Helps in Software Design

- Identifies system structure early  
- Avoids God classes  
- Helps choose **inheritance vs composition**  
- Improves maintainability  
- Acts as long-term documentation  

---
## Sequence Diagram

A **Sequence Diagram** is a **behavioral UML diagram** that shows **how objects interact with each other over time** to complete a specific use case.

It focuses on:
- Order of method calls
- Message flow between objects
- Runtime behavior of the system
  
> A sequence diagram represents object interactions arranged in time order to show how a process is executed.

---

## Main Elements of a Sequence Diagram

### 1. Actor
- External entity that initiates the interaction  
- Example: User, Admin, External System

### 2. Participant (Object)
- Objects involved in the interaction  
- Shown at the top of the diagram

### 3. Lifeline
- Vertical dashed line below each participant  
- Represents the object’s existence over time

### 4. Messages
- Communication between objects  
- Shown using arrows

Types:
- Synchronous (solid arrow)
- Asynchronous (open arrow)
- Return message (dashed arrow)

### 5. Activation Bar
- Thin rectangle on a lifeline  
- Indicates when an object is executing logic

---

## Example: User Login Flow

### Scenario
A user logs into an application.

### Participants
- User  
- LoginController  
- AuthService  
- UserRepository  

### Step-by-Step Flow
1. User sends login request to LoginController  
2. LoginController forwards request to AuthService  
3. AuthService fetches user data from UserRepository  
4. UserRepository returns user details  
5. AuthService validates credentials  
6. LoginController sends success/failure response to User  

---

## Textual Representation (Conceptual)

```
User -> LoginController : login(username, password)
LoginController -> AuthService : authenticate()
AuthService -> UserRepository : findUser()
UserRepository --> AuthService : user
AuthService --> LoginController : result
LoginController --> User : response
```

---

## Why Sequence Diagrams Are Important

- Visualize runtime behavior
- Help design APIs and microservices
- Identify unnecessary calls and tight coupling
- Improve communication among teams

---

## Sequence Diagram vs Class Diagram

| Aspect | Sequence Diagram | Class Diagram |
|------|------------------|---------------|
| Type | Behavioral | Structural |
| Focus | Flow of execution | Structure |
| Time | Yes | No |
| Usage | Interaction design | System design |

---

## When to Use a Sequence Diagram

- Login or authentication flow
- Payment or transaction flow
- Microservice communication
- Complex business processes
- Interview explanations

---





       
 
