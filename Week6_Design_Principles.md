# Design Principles

## Introduction

* high level guidelines that ensure designs are robust in the face of defect fixes and feature additions
* not absolute: sometimes you will violate them, but be careful to do so in a considered manner

## Coupling
**Coupling:** property that indicates the **strength of connections** between different program elements

**Problem:** negatively influences evolvability and maintainability  
- easier for errors in one part of the system to propagate to other unrelated parts of the system  
- increases the degree to which a single bug fix or feature addition is scattered across the codebase  
- harder to reuse independently  
- harder to understand because it can't be considered in isolation

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/coupling_flow.png'>

### Types of coupling

***data coupling:*** classes share data through primitive parameters  
ie. `setSize(width: number, height: number) {}`

***stamp coupling***: classes coupled through public types; works well for small interfaces  
ie. `setState(state: sizeState)`: sizeState is a public type

***control coupling***: caller controls flow within another unit; caller needs to know something about the callee which it shouldn't  
ie. `handler(event:keyboardEvent, status: boolean)`: status is unclear the handler function needs to know its implementation

***global coupling:*** global variables that make it harder to reason about the flow of data. They can be changed in **unexpected ways**, which impacts reusability and decreases testability.

***content coupling:*** internal state of one unit is modified by another. This is a **breakdown of information hiding**.

### Ways to decrease coupling

- minimize **interfaces** between elements  
- minimize **complexity of interfaces**: makes interaction between program elements clearer and easier to reason about and evolve  
- avoid **control flow coupling**

## Cohesion

**Cohesion:** property that indicates how focussed our program elements are on performing a single task  
- easily thought of in terms of classes in object-oriented design - measures how well **elements within a class belong together**  
- **low cohesion** means the class is responsible for a **wide variety of tasks**  
  - hard to reason since they have many competing concerns within implementation that might conflict  
  - defect for one feature might be a design for another
- cohesive classes generally have a **small set of private fields** that makes sense to the majority of the **public methods** within the classes  
- proliferation of classes within a system makes it easier to understand a single class
  - simplifies any future bug fixes or feature additions

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/cohesion_flow.png'>

### Types of cohesion

***functional cohesion:*** all code contributes to a single feature or task

***sequential cohesion:*** output from one step is used as input in the next step  

***communicational cohesion:*** different tasks are grouped together because they work on the same data  

***procedural cohesion:*** follows execution sequence, determined by control flow  

***temporal cohesion:*** functionality grouped only because of execution timing  

***logical cohesion:*** combined for task, regardless of intent

***coincidental cohesion:*** arbitrarily grouped; hard to understand and reuse, and propagate defects

## Design Guidance: SOLID

Design symptoms:  
- ***rigidity:*** resistant to change
  - makes **effort estimation** hard because we can't see propagation
  - also makes **risk analysis** difficult
- ***fragility:*** easy to break
  - makes **assumptions about context** they will be run in
  - lax in **post-/pre-condition checks**
- ***immobility:*** hard to reuse, too many dependencies
  - "clone and own"/pragmatic software **reuse**: copy and pasting code and dependencies
- ***viscocity:*** easy to violate than adhere
  - developers will introduce "hacks"
- ***needless complexity:*** building too many **abstractions**
  - cognitive overhead to future developers; hard to understand
- ***repetition:*** clones from copy and paste
  - often indicates **missing abstraction**
  - you would need to fix defects in all places with clones
- ***opaque:*** hard to understand
  - ie. putting functions into existing abstractions instead of building new classes

### Single responsibility
A software module should do **one thing** and do it well.  
Design patterns:
- **strategy pattern:** modules encapsulate algorithms
- **command pattern:** small modules that only provide features needed for a specific action
- **state pattern:** encapsulates all behaviours for a state in a module

### Open/closed principle
Modules should be **open to extension** but **closed to modification**.
- encourages engineers to design code to be more **amenable to future change**
  - think explicitly about which parts of their systems should enable future feature additions and which should not
- extension points typically **add abstraction** which makes it hard to understand, and call cause **negative performance** or **security implications**
- challenges - knowing when to enable extension
- common **code smell**: `instanceof` or `typeof` checks - if a system is extended, client code will need to be updated so type checking checks for the new features
- sometimes supertypes have private fields, so subtypes are better insulated from changes in the supertype

### Liskov substitution
Any object can be **interchanged** with any other object that has the **same parent type**.

### Interface segregation
Clients should **not** be forced to **depend on interfaces they do not use**.
- have smaller, *cohesive* interfaces
- compose a larger interface with smaller interfaces

### Dependency inversion
Classes should **depend on abstractions**, not **implementations**.
- helps to design implementations that are as **decoupled** from one another as possible
  - by injecting an interface, and have the classes take dependencies on the **interface** instead
  - if you wanted to reuse one of the classes, you would have to reuse the interface rather than the concrete class and its dependencies
- when we refactor to encourage **extensability**, we do it through dependency inversion
  - introduce a new interface and make the existing code implement the interface
