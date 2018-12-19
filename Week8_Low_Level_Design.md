# Low Level Design

## Introduction

#### Three commonly used low level design principles:
***Encapsulate what varies:*** identify parts of the system prone to future modification, likely to need future extension, or could be used in other contexts; encapsulate them to ease future tasks
- can be challenging to figure out what kinds of future needs you may have
- cannot encompass *all* future needs, since this makes it hard to understand

***Design to interfaces:*** design systems around interfaces so that we can decouple implementation from design
- reuse parts of the system without having to worry about the implementation of the features by using **interface wrappers**
- defines high-level vocabulary - looking at declarations of methods, parameters, and return types, we can understand **collaborations** within the system
- if the amount of code is large, interfaces will be important to ease future reuse

***Favour composition over inheritance:*** makes it easier to add new features as time goes on; composition and delegation is frequently a better choice than inheritance for long-term evolvability
- inheritance-based code is more brittle - sub-types are more tightly coupled to its super-type than class-to-class coupling through composition
- composition: have an object from a parameter or field in class, and invoke methods on that object; drives you towards **smaller classes** that are bound to certain behaviours
- inheritance: extend an existing class

## Design patterns
Provide guidelines to structure systems to avoid future evolutionary problems.
- leverage previous design knowledge and build flexibility into our systems
- also provide shared vocabulary between developers when descussing system design
- when adding design patterns, you are adding an **abstraction**, which can make it difficult to understand in the future

Parts of a design pattern: **name**, **problem** they are tring to solve, and a **solution**.
- each pattern has pros and cons
- can't be mechanically applied - design upfront or carefully refactor it into a system
- language independent - applies for any object oriented language

#### Categories of design patterns:
**Creational:** build objects in an extensible way
**Structural:** structure systems to avoid future evolutionary problems
**Behavioural:** makes it easier to add new behaviours at runtime

## Singleton (creational)

**Problem:** object that we want to use widely, but don't want to pass an instance to each place that we want to use it; often we want only a **single instance** of that object

**Solution:** ensure that only one instance of a particular class is ever created

**Programmatically:** have a `getInstance()` method that retrieves the instance if it is not `null`, and calls the constructor only if it is `null`
- singletons are often used by other patterns, such as the builder or the abstract factory
- caution! each singleton is essentially a **global variable**, which is bad from an evolutionary point of view, bad for understanding, and bad for recognizing the current state of the system
- challenge: rely on static `getInstance()` method; static methods make it hard to evolve, but in this case the behaviour is relatively fixed, so it's generally ok
- spreads **dependencies** throughout the system, which are also hidden because they are not passed as parameters or fields
- hard to test since the objects aren't passed around; hard to mock up or set up stubs

## Observer

**Problem:** when a change is made to a problem, the change should be consistently reflected across the entire system

**Solution:** decouple the state changing actions by leveraging dependency inversion to implement **inversion of control**; provides a means for a library to call into your code

**Programmatically:** subject maintains a list of its dependents, called **observers**, and notifies them automatically of any state changes, usually by calling one of their **methods**
- Observable classes are not coupled with any concrete Observer objects, so new Observer objects can be dynamically added from Observable objects
- still *some* coupling, since the observable objects must know about the Observer interface, and all observers must know about the objects they are observing
  - but do not know about the concrete subtypes of Observer, so new observers can be added, or the system can be extended by adding a new subtype of Observer without changing its model elements

## Strategy

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_strategy-example.png'>

**Problem:** we don't know all variations in advance, so we want a mechanism that encapsulates algorithms to allow future extension

**Solution:** have explicit interfaces (that have a defined structure of all algorithms we want to encapsulate) that can switch concrete algorithms or strategies based on the situation

**Programmatically:** provides strategy interfaces that new classes can implement to add new functionality; context uses composition with a field of type `Strategy` and forwards the request onto the correct strategy

- client code doesn't change nor interact with the concrete instances
- add factory between client and algorithms so that the client does not know about the algorithms
- or there can be a dependency of context on the factory, so the client won't have the dependency on the factory and makes it more simple

## State (behavioural)

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_state-example.png'>

**Problem:** we often want to vary a behaviour of our program based on the internal state within our program; important in an object-oriented context, since the state of objects will change over time

**Solution:** encapsulate states explicitly within state objects, and allow objects to control transitions within states of a program
- isolates state decisions, which makes reasoning about how or why these transitions took place much easier
- simplifies state management and transition code, since there is only a subset of valid other states that the program could transition to
- differs from the Strategy pattern by their **intent**: strategies are **fixed** at the start of execution, whereas State changes during **runtime**

## Facade (structural)

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_facade-example_after.png'>

**Problem:** subsystems can contain a large amount of code that even if well designed can be difficult for a client to learn to use correctly

**Solution:** reduces the inherent complexity of a subsystem by providing a unified set of interfaces for a system, simplifying modules for common tasks

**Programmatically:** provide a high-level interface to a complex subsystem

- **does not prohibit** clients from accessing features within the subsystem directly, but if a client only uses the facade, they are more insulated from **structural changes** within the subsystem
- hide internal details of the subsystem and allows the client to have only a single dependency on the facade
- **decreases coupling** between client and subsystem classes using **composition**
- **violates the Single Responsibility Principle:** access to many different tasks, since their implementations are spread across different types
  - but clients are more easy to reuse and write

## Decorator (structural)

<img align='right' width='400' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_decorator-example_c.png'>

**Problem:** two objects instantiated from the same type need to behave differently at runtime

**Solution:** allows behaviours to be added to an individual object, rather than every instance of a class; all combinations of behaviours do not need to be compiled in advance

**Programmatically:** enable objects to be wrapped in other objects; use composition to treat the wrapped object as if it were a single object

- *object vs class:* class is the structural template from which object instances are created; object is a single instance, and a class can have different instances
- you can't control the order of the wrappers, so they cannot interact with one another directly
- decorators interfere with object identity, so code that relies on checking identity (ie. with `instanceof`) will behave differently
- bare classes can be kept simple focussing on their core responsibilities (**single responsibility**), while additional functionality can be implemented in decorators (**open/close**) - adding new decorators is easy and does not change the base cases
  - example of *composition over inheritance*; most power comes from encapsulated fields

## Composite

<img align='right' width='300' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_composite-example.png'>

**Problem:** systems often start with individual objects, but over time gain the ability to group objects together

**Solution:** provide a mechanism for treating groups of objects as individual objects (often known as **part-whole hierarchies**)

**Programmatically:** define a unified Component interface for both part (Leaf) objects and whole (Composite) objects; individual Leaf objects implement the Component interface directly, while the Composite forwards requests to their child components
- clients work with the Component interface to treat Leaf and Composite objects uniformly
- Leaf objects perform the request **directly**
- Composite objects **forward the request** to child components downward through the **tree structure**
- makes client classes easier to implement, change, test, and reuse

## Visitor

![visitor](https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_visitor-example.png)

**Problem:** given a large set of objects, it is often necessary to perform tasks on them that are not part of their core responsibilities, since this pollutes classes and adds non-essential code

**Solution:** enable operations to be performed on an object hierarchy without modifying the hierarchy itself

**Programmatically:** add new method to every class being traversed, which is called `accept(visitor: Visitor): void`; all future visitors work with this interface

- separates algorithm from an object structure
- creates a visitor class that implements all the appropriate specializations of the virtual function; takes the `instance` reference as input, and implements the goal through **double dispatch**
- adding concrete types to the type hierarchy is hard, since every visitor needs a visit method for every type being traversed
- often challenging to understand how the visitor works

## Model View Controller (MVC)
Widely used **meta-pattern** that employs other design patterns
- first popularized with the Smalltalk-80 language

**Problem:** interfaces change more frequently than their backend data models and business logic, so frontend development should be more decoupled from other development activities, especially if developed by different teams

**Solution:** split responsibilities into three groups: model, view, and controller

<img align='right' width='300' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_mvc.png'>

### Model
- programatically representation of the application's data
- often contains functionality for persisting (and loading) itself from permanent storage, as well as ability to validate its correctness (and ensure it's not in an inconsistent state)
- use **concrete model objects** makes it easier to understand by allowing **interfaces** for model elements to be well documented
- often **subjects** in the **observer pattern**

### View
- responsible for rendering the Model for presentation to the user
- users interact with and manipulate the View to change the Model
- does not store data or maintain state; updated independently from the Controller or Model
- often declare an **interface** allowing different views to work for the same task (ie. `DesktopView` and `MobileView`)
- are often composites
- acts as the **observer** for the subject models in the **observer pattern**

### Controller
- controls the main application logic, and **binds View to Model**
- mediates how the Model is updated when the user performs actions in the View
- typically tightly bound to the View interface
- all application logic should be placed here to ensure that alternate implementations of the View interfaces have the same **runtime behaviour**
- also enables Controller test effort to be amortized across all views

#### At runtime, MVC works as follows (for a user View update):
1. View notifies Controller of change.
2. Controller modifies Model as required.
3. Model, which is the subject of the observer pattern, notifies its observers that it has changed.
4. The View, which is the observer in the pattern, receives the notify event.
    - The View **queries the Model object** to understand the nature of the change and updates itself.

#### Drawbacks
- Views tend to get out of control
  - easy to put functionality within the view, leading to huge views and anemic controllers
  - heavy views are harder to test than heavy controllers - view logic gets mixed up with display logic, forcing error-prone **UI-based testing** to be used rather than **unit tests** for the controller alone
- every component talks to every other component
  - apparent with the Model - all components need to be updated if it changes
  - negative impact on maintainability and extensibility
- hard to use this for a small system, but useful once it gets large

#### Enables
- collaborative views
- enhanced maintainability since it's easier to add new views (if logic is in the Controller)
- teams split by designers, UX, and frontend teams to concentrate on Views
- enhanced testability by isolating application logic in Controller

## Model View Presenter (MVP)
- popularized by Google; heavily used in GWT framework
- primary difference in MVP from MVC is that Model is isolated from the View; Views are more stringently minimal in MVP than MVC
- enhance testability and avoid heavyweight views

#### Model
- relatively unchanged from MVC
- except it can optionally use an **event bus** to communicate with the rest of the system
  - fine-grained observing
  - easier to debug update notifications

#### View
- significantly simpler in MVP than MVC
- view **never sees Model objects**
- Presenter **must mediate** all View/Model interactions
- View deals with only **primitive types** (ie. `Number`, `String`, and arrays in TypeScript)
  - ensures Presenter is main location for all program logic
  - views become more similar; validated "correct by inspection" since they are rendering primitive types
  - Presenter has to mediate using primitive types, which are easy to `assert` or `expect` with traditional unit testing frameworks

#### Presenter
- actually contains **all of the application logic**
- no dependencies on UI components (to make it easier to test)
- tend to be complex, but easily unit tested
  - with traditional unit testing without requiring a fake UI (ie. `MockView` can be a regular object, and not need access to the DOM)
- View and Presenter are tightly bound - they need to know the public interface of the other to enable communication

<img align='right' width='300' src='https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_mvp.png'>

#### Typical design example:
- ViewPresenter and OutlinePresenter each have one View
- updates to the controller optionally mediated by an EventBus
- AppController responsible for dynamically starting and stopping Presenters and configuring the View each Presenter is bound to

#### Runtime (for user View update):
1. View notifies Presenter of the change; data passed between View and Presenter consists only of method calls with **primitive** parameter types.
2. Presenter modifies Model.
3. Model, which is the subject in the Observer pattern, notifies observers that it has changed.
4. EventBus (or Presenter if EventBus not used), acts as the Observer, receiving the notify event. Often passes the instance of the model so Presenter doesn't need to retrieve again.
5. Presenter determines how the View should be updated and refreshes it, using only **primitive** values.

#### Pros:
- more testable
- minimizes bloated views
- EventBus makes it easier to keep track of updates

#### Cons:
- views tend to contain a lot of boilerplate primitive code that updates UI widgets with values and calls methods with the Presenter

## Model View ViewModel (MVVM)

![mvvm](https://github.com/ubccpsc/310/raw/2018sept/readings/figures/patterns_mvvm.png)

- most recent
- further cements connection between **view and controller** by considering them a **single unit**
- like a specialization of MVP but the View and Presenter are more tightly bound using two-way data binding

#### View
- typically uses **data binding** to allow data values to automatically update UI representations, **decreasing boilerplate code**

#### ViewModel
- acts like the Presenter in MVP
- contains most of the application logic

#### ViewController
- focusses on preparing data in a way that is amenable for data binding (and responding to user-driven events appropriately)
