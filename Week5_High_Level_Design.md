# High Level Design

- during design, requirements are transformed into a format that can be implemented directly: **input** is a list of requirements, and **output** is a set of classes, methods and fields

#### Two broad activities (although highly iterative):  

***high level design:*** identify key architecture, abstractions, and relationships required to build the system
- evaluate different ways the system may have to evolve in the future and evaluate architectural decisions
- guides low level design

***low level design:*** defines concrete public interfaces, classes, and methods needed to build the system

## Introduction to Abstraction

**Abstraction:** technique used by software engineers to be able to manage the complexity of their systems; enables engineers to focus on **key information** for a given task while omitting **unnecessary detail**

### Data abstraction
Process of explicitly separating the **abstract properties** of a data type and its **concrete** implementation
- enables client code to be oblivious of the underlying implementation of a data type, allowing it to be upgraded and improved **without impacting client code**
- Ex. a `Vector` in Java is implemented using an array, but JDK authors can change this implementation without impacting client programs
- Ex. data abstraction from CPSC 210:

```
// Creates a new team
// Requires: A non-empty and unused name.
// Modifies: Team database.
// Effects: Returns whether a team was created.

public createTeam(name: string): boolean {
  ...
}
```
The client code doesn't need to know about the implementation of createTeam

### Control abstraction
Abstraction layer between code and how the machine interprets and executes instructions
- allows engineer to think about what they're doing, rather than how it works
- ie. high-level language (ie. Java) vs Assembly
- ie. promises in JavaScript to allow sequences of async operations to be developed without nested callbacks and mimics synchronous method calls

**Structured programming:** coined by Djikstra; describes **language constructs** that are designed to allow developers to express programs using logical blocks of code that execute in sequence with control statements (ie. if/then/else, while, for) for choosing which blocks execute and subroutines so programs can be decomposed
  - aimed at improving **clarity, quality, and development time** of a program

## Decomposition
Process of taking a complex high-level entity and splitting it into smaller pieces, making **simple tasks simple** and **exceptional tasks possible**.

### Top down decomposition
Broken up into pieces at the top, and working to a greater level of details
- initially means many important **details** represented by **"black boxes"** (can represent systems, modules, or other relevant levels of abstraction) that are filled later

*Pros:* global awareness about full system  
*Cons:* challenging once terminal boxes develop constraints that mean you have to revisit prior decisions

### Bottom up composition
Decisions are made from the **leaf nodes** upward.

*Pros:* great when you have concrete details about the **team implementing the system** because you can make the relevant decisions for the team right away  
*Cons:* lack global overview - leads to **inconsistencies** and extraneous **focussing on details** early on in the design stage

## Information hiding
Separates the parts of the program that are **most likely to change** from parts that are more static.

- first proposed by David Parnas (1972)
- high level motivation for APIs: by describing the expected behaviour of the API, you can **"hide" the implementation**

*Cons:* unnecessary abstractions can **add complexity and difficulty** when trying to understand the system; balancing this complexity against trying to evolve a system lacking abstractions is challenging

## Encapsulation
Typically implemented with **interfaces**, which separate data and behaviour from implementation
- interface describes the **public contract** an object provides
- concrete class describes **implementation of interface** (+ methods and fields)

## Constant change

- Important to recognize the parameters that lead to the abstractions you choose will change over time, and often by orders of magnitude
  - right design will be different at 10x or 100x load
  - Jeff Dean (WSDM 2009 Keynote) advocates **designing for 10x load** with an **expectation** that a rewrite will be needed for 100x load
