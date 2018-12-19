# Software Processes

## Introduction

Processes enable us to structure the diverse set of activities needed to create and maintain a successful software project
- define the **who, what, when, and how** for enabling a project to succeed

**Stakeholders:** developers, QA, managers, customers, users, dev/ops, supports, VPs/C-suite
- different representations and documentation needed:
  - **specification:** mission statements, business objectives, requirements (managers, users, sales)
  - **development:** system architecture, detailed design, implementation plan (managers, developers, QA)
  - **validation:** test plan, risk assessment (QA)
  - **deployment/evolution:** development/ops plan, maintenance strategy (devops, support)
- not every document is needed for every project
- core role of processes is to **increase transparency** so all stakeholders know what is expected of them and what they can expect from others

## Traditional processes

### Waterfall

"Classic" software development model; each project flows to the next with explicit **stakeholder sign-off** before the next phase

<img align='right' width='400' src='/images/2_proc1.png'>

- limiting as requirements are often imperfectly understood at the outset of a project
- overall value cannot be validated until process has run to completion
- hard to step back and **resistant to change**
- distinct phases - **easy to understand** what's happening
- clear and explicit handoffs between phases

### Spiral

<img align='right' width='200' src='/images/2_proc2.png'>

Increases **responsiveness** of waterfall-based processes
- **planning:** gathering and analyzing requirements
- **risk analysis:** identify sources of risk and mitigation strategies
- **engineering:** system is built and validated
- **evaluation:** validated externally with customers to inform future iterations

~ 1 year process per iteration

*Shortcomings:*
- documenting risk can be overwhelming
- delays customer feedback

## Agile approaches
Emerged in the early 2000s to allow development teams to become **more flexible** and achieve **higher development velocity**

#### Agile Manifesto
12 tenants; 4 main:
- **individuals** and **interactions** over processes and tools
- **working software** over comprehensive documentation
- **customer collaboration** over contract negotiation
- responding to **change** over following a plan

Designed to increase customer **interaction** and **accountability**, focussing on **experimentation**, but keeping the system in a **buildable state**.

### XP (Extreme Programming)
First widely influential agile methodologies; system should **always be buildable**
- developers should be willing to **start small** and adapt and refactor systems as the need arises, rather than investing in upfront engineering efforts
- give key principles:
  - **communication:** enabling open and continual communication between all stakeholders can help projects stay on track and all stakeholders are aware of the schedule impact of decisions
  - **simplicity:** focus on the simplest possible solution to validate before more expensive or challenging solutions
  - **feedback:** ensure the most locally "correct" decisions are being made
  - **courage:** be willing to discard failed experiments - opportunities to learn and improve the overall system
  - **respect:** don't break the build; focus on long-term understandability

### Test-driven development

Ensures that **automated testing** is always a key part of development
- important because agile relies on quickly refactoring code
- linear steps:
  1. Add a test.
  2. Run tests to ensure they fail.
  3. Write code and run tests.
  4. Refactor the code.
- not often used in practice as developers have a hard time writing tests before source code
