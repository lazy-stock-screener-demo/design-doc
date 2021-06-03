# Development Process

## What is it?

A document that introducing on how I construct a large scale system in terms of domain-driven design. The fundamental structure of the whole development process is based on domain-driven design (DDD) and [system-design-primer](https://github.com/donnemartin/system-design-primer). DDD provide a organized and clear way to break down problem into actual tactical context, while system-design-primer demonstrate a solid and comprehensive frame and details that including merit and demerit of apply any component into a system and what is the potential alternatives.

## Table of contents

- Check Business Idea
- [Strategic Design](strategic_degisn.md)
  - Problem Space
    - Identify Full end-to-end business use case
    - System Context Diagram
    - Domain Design
  - Solution Space
    - Build Bounded Context based on problem space in domain model
    - Bounded Context Map
    - Define Ubiquitous Language in Bounded Context
    - Identify Use Story through Event Storming
    - Identify User Story in Bounded Context in terms of role/permission based on UseCase
  - User Story and Worker
    - User Story
      - Stock Catalog Context
      - Customer Self Context
      - Customer Identity Context
    - Worker Story
      - Stock Catalog Context
  - User Story and Operation/Transaction
    - Customer can search stock based on ticker/name
    - Customer can login to check their dashboard, User can change his profile and setting and delete his account.
- [Tactical Design](tactical_design.md)
  - infrastructure strategy
  - context details

## Check Business Idea

Check other products' pros and cons in order to understand which direction I can head on and how many rooms of opportunity I can have. Details discussions are not listed here since PM is not my career goal. Nevertheless, I did explore and do some research in advance.

## Strategic Design

![Strategic Design in DDD](https://drive.google.com/uc?export=view&id=19-pno17L98jpnhwYyRZShlt5ZhUlZDgN&sz=w300-h50)

Ref: https://ithelp.ithome.com.tw/m/articles/10216792

Main objective of DDD is to define Bounded contexts, the Ubiquitous Language and the Context Maps together. They help me to identity the relation between problem I have faced and solution I can apply. Not only leveraging from DDD, but I also take advantage of use-case and user story. All the tools I have can really help me to build a large scale system from scratch. To be more specific, from use-case to functional program.

## Tactical Design

![Tactical Design in DDD](https://drive.google.com/uc?id=1jaJUP7Az5St4nsyedx14y4zhHN25uUTN&sz=w300)
Ref: https://ithelp.ithome.com.tw/m/articles/10216792

In addition to simply applying tactical design in DDD, I merge the structure of system-design interview into my own version of "tactical design".

Generally, every decision are accompany with tradeoffs and alternatives. I will list them all in each stage and explain why a choice is the best for me. Since the other options are ready there, I can quickly migrate from current setup to others based on observations that are mainly from system metric from time to time. The topics contain key point of a system, e.g. core component and scale strategy.

## License

GNU GPL
This document should be and only can be used as reference in my future interview only.
