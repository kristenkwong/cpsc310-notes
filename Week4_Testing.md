# Testing

## Introduction

> *"Testing shows the presence, not the absence of bugs."* – Dijkstra

#### Goals:
1. To ensure our code **implements its specification**.
2. To try to **find defects** in our implementation.
3. To understand **where we can expect** to have found defects.

*Non-goal:* to ensure our system is bug-free  
*Key-constraint:* time  
*Approach:* coverage-guided white box testing

## Coverage

**Coverage**: proportion of the system being tested, usually represented as a percentage `% = (covered/(covered + uncovered)) * 100`

#### Appealing properties of coverage:
- **cheap to compute:** simply needs to track which parts of a program have executed while the test suite is running
- **actionable:** developers can look at a coverage report, find parts that are not currently covered, and decide how to act upon this information in a straightforward way

#### Two high level categories:

1. **Flow Independent**
  - most common: cheap to compute and easy to reason about
  - executes each part of the system and ensures it is capable of executing successfully
  - Kinds:
    - **block:** measures the proportion of blocks in the system that are executed by the test suite; split up program into parts **smaller than methods** but **coarser than lines**
    - **line:** measures proportion of lines in the system that are executed by the test suite
    - **statement:** percentage of statements; compound ifs or loops often contain multiple statements
2. **Flow Dependent**
  - gives insight into stringency of execution
  - Kinds:
    - **branch:** ensure we've covered both sides of a branching statement (ie. true and false branches of an `if` statement)
    - **path:** ensures we've executed all paths; ie. in an if statement, execute all **combinations**
    - **MCC:** extension to path; ensures all statements are also independently evaluated with respect to the outcome of a function; hard to use in practice

Example code snippet:
```
eval (x: number, c1: boolean, c2: boolean): number {
  if (c1)
    x++;
  if (c2)
    x--;
  return x;
}
```

Tests to achieve full test coverage:
```
// 100% statement coverage
eval(0, true, true);

// 100% decision coverage
eval(0, true, true);
eval(0, false, false);

// 100% MCC coverage
eval(0, true, true);
eval(0, false, false);
eval(0, true, false);
eval(0, false, true);
```
- greatest shortcoming of code coverage is that while it shows the suite **executes** the code under test, it does not show the code is **correct**
- start with simple coverage and move to more stringent coverage once they have high coverage with simpler metrics
- high quality coverage > artificially improving coverage

## Testability

#### Goals of testing:
1. Reach the code we are trying to test.
2. Trigger defects.
3. Propagate results to where we can observe it.
4. Observe the fault.
5. Interpret result as a defect.

***Testability:*** building systems so we can validate it more easily  
- in Test Driven Development, writing tests first ensures system is structured in a testable way

***Controllability:*** make system/code under test (SUT/CUT) be **programmatically controlled**, so an automated test can be written for it
- often a tradeoff between what is possible versus efficient to test

***Observability:*** sometimes CUT can be invoked by a test, but its outcome cannot be observed
- Ex. if the defective method mutates some external inaccessible object, but **does not return a value**
- often resolved by returning data to the caller

***Isolateability:*** refactor more complicated segments to smaller, manageable components
- in simulation-based environments, code dependencies are **mocked** or **stubbed**, where they are replaced with developer-created fake components that **take known inputs** and **returns known values**
  - Ex. `MockLoginRejectController` that always returns false


- mocking **increases performance** and makes components less prone to **non-determinism** since the result is fixed and not **dependent on some external computation**

***Automatability:*** suites that run without human intervention are **more likely to be run**
- also likely to be run as a **regression suite**
- automated suites can be run online as changes are made to a system, after every commit, nightly or weekly

## Assertions

Used to **evaluate correctness** within automated unit tests
- jUnit: one of the earlier unit testing frameworks
- many modern frameworks provide features similar to jUnit's assertion features for evaluating program outputs

#### Most common high-level models to consider when structuring tests:
- ***Four-phase test:*** supported by default by jUnit and variants
  1. set up test fixture (`before`, `beforeAll`)
  2. executing code under test
  3. evaluating output of the CUT through assertions
  4. tear down the test environment


- ***Given-When-Then:*** developed by behavioural driven programming (BDD)
  - create "executable specifications" through unit tests that use descriptive strategies for nesting and naming
  - structured from some **given** state, where configuration is understood
  - key action is **when**; often described in plain language
  - **then** involves observing the output to ensure its correctness
