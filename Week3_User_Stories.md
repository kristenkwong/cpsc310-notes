# User Stories

**User stories:** lightweight descriptors of **features** often used to specify software development **tasks**
- used to discuss features, how they can be validated, and their cost
- representations are an easy way to increase cohesion between product owners and engineers

## Format

1. ***Role-Goal-Benefit***
  *"As a `<stakeholder>` I want `<function>` so that `<value>`."*
  - conveys who the feature is for, what it is to do, and what the value to the user is  

2. ***Limitations***
  - scopes down the role-goal-benefit to only what matters for the feature

3. ***Definition of Done***
  - specific description of how the story can be validated by both the developer and product owner to ensure it is completed correctly
  - avoid work on unnecessary functionality the stakeholder might not need

4. ***Engineering Tasks***
  - useful for the development team to keep track of how this feature interacts with system or subsystem

5. ***Effort Estimate***
  - cost of a feature
  - useful for discussing value - cost vs benefit
  - some dev teams specify in days, sometime in points (1/2 to ~40)
    - if engineering days are used, generally 0.5 to 5 days

## INVEST
Used to evaluate user stories.

***Independent:*** self-contained; can be reordered and implemented as needed

***Negotiable:*** choosing which story is performed at the next iteration should be negotiable
  - Allows product owner and technical team to ask more questions that flesh out the story and provide greater detail as needed

***Valuable:*** clear about how it adds meaningful value to a product (role-goal-benefit)

***Estimable:*** should have enough detail to evaluate its feasability and cost

***Small:*** features between half a day and half an iteration
- longer stories should be split up to reduce the chance of a story not making it in an iteration

***Testable:*** all stakeholders can understand how the story will be evaluated

Example:
```
As a prof, I want to be able to create a repository for a 310 team so they can start working on the project.

Notes:
Inputs: team name, initial repo, team member GitHub ids.

Definition of done:
Single command will take params and complete task.
Success should be programmatically verifiable.
Unit tests to check for error handling for org, team name, and members.
Integration test will ensure compatibility with the GitHub API.
Script-based tests will be provided for the command line aspects of the feature.

Engineering notes:
Must integrate with existing GitHubManager
Any constants that would need to be changed must be stored in config.js.
API will be used by user interface in future, keep this in mind when designing API.

Estimate: 1.5 units.
```
