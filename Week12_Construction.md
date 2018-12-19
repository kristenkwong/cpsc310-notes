# Construction

## Introduction
- many software engineering aspects to consider while constructing your system

## Non-structural properties: readability

#### Readability makes your code:
- easier for others to understand
- easier to make changes to
- reduces chance of introducing bugs

#### Why readable code?
- acts as documentation itself
- documentation can become outdated
- readable code is the **ultimate spec** of what the system is doing

#### How to make code more readable?
- avoid **deep nesting**
- avoid **bad naming** - names of variables and functions should be easy to understand
- avoid **individual style** within the same code base - code should look cohesive and seem to be written by one person or a focussed team; easier to understand and modify
- avoid having **no comments** - not key types and methods
- have code that reads like **natural language**
- code should be separated into **digestible chunks**; break down large complex pieces of code into individual steps (methods)
- code should be **self-explanatory** and tells a **story** - acts as a spec for itself

## Static analysis

- simplest form is the **compiler** - ingest source code and output errors
  - don't give semantic warnings, only validate **syntax**
- **linters** are a class of static analysis tools that provide semantic warnings to help avoid common mistakes
  - applying heuristics that can identify things that could be problematic in the future
  - can also ensure the code adheres to a team's style guide
  - linting tools all publish the heuristics they use
  - rules are just warnings, not errors; some teams may not use all the rules

## Automation
- crucial to modern software because of team size, complexity, and pace of development
- allows building software in a reliable, repeatable, and revertible way

![automation](https://github.com/ubccpsc/310/raw/2018sept/readings/figures/automation_feedback.png)

1. When **code is changed**, we can use automation to get the code and all its **dependencies**; feedback should go back to the changing code
    - use package libraries like npm or maven
2. Once we have the code, we should **build** the project
    - fully automated in most large systems
3. Run **tests** automatically on the project
    - hit one button and run all tests
4. **Deployment** step if all the tests are passed
    - should be automated to reduce chances of human error causing an important failure

- each step has a **feedback loop** to go back to the changing code - includes changing metadata and automation scripts
- we can automate all of the steps except the original code change

#### Goal of automation is to transform building software into a process that is:

**Repeatable:** process of building a product should not vary between different version of the software (except minor improvements)
- unified building process provides a mechanism for building the system the **same way**, with minimal (preferably no) human intervention required

**Reliable:** should be able to trust that the repeatable process will run to its successful completion
- naturally, test failures are to be expected and are not considered a build automation reliability failure, unless the tests themselves are flaky

**Revertible:** should be able to quickly and transparently revert out of any change
- if deployed and found to have an error in production, it should be possible to automatically revert to a prior build in a quick and transparent way
