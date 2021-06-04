# Development Process

## What is it?

A document that introducing on how I construct a large scale system in terms of domain-driven design. The fundamental structure of the whole development process is based on domain-driven design (DDD) and [system-design-primer](https://github.com/donnemartin/system-design-primer). DDD provide a organized and clear way to break down problem into actual tactical context, while system-design-primer demonstrate a solid and comprehensive frame and details that including merit and demerit of apply any component into a system and what is the potential alternatives.

## Table of contents

- Check Business Idea
- [Strategic Design](strategic_degisn.md)

  - [Problem Space](strategic_degisn.md#problem-space)
    - [Identify full end-to-end business use case](strategic_degisn.md#identify-full-end-to-end-business-use-case)
    - [System Context Diagram](strategic_degisn.md#system-context-diagram)
    - [Domain Design](strategic_degisn.md#solution-space)
  - [Solution Space](strategic_degisn.md#solution-space)
    - [Build Bounded Context based on problem space in domain model](strategic_degisn.md#bounded-context)
    - [Build Bounded Context Map](strategic_degisn.md#bounded-context-map)
    - [Define Ubiquitous Language in Bounded Context](strategic_degisn.md#ubiquitous-language-in-bounded-context)
    - [Identify Use Story through Event Storming](strategic_degisn.md#identify-use-story-through-event-storming)
    - [Identify User Story in Bounded Context in terms of role/permission based on UseCase](strategic_degisn.md#identify-user-story-in-bounded-context-in-terms-of-role)
  - [User Story and Worker](strategic_degisn.md#user-story-and-worker)
    - [User Story](strategic_degisn.md#user-story)
      - [Stock Catalog Context](strategic_degisn.md#stock-catalog-context)
      - [Customer Self Context](strategic_degisn.md#customer-self-context)
      - [Customer Identity Context](strategic_degisn.md#customer-identity-context)
  - [User Story and Operation/Transaction](strategic_degisn.md#user-story-and-operation/transaction-mapping)
    - [Customer can search stock based on ticker/name](strategic_degisn.md#Customer-can-search-stock-based-on-ticker)
    - [Customer can login to check their dashboard, User can change his profile and setting and delete his account](strategic_degisn.md#Customer-can-login-to-check-their-dashboard,-User-can-change-his-profile-and-setting-and-delete-his-account)

- [Tactical Design](tactical_design.md)
  - [Infrastructure Strategy](tactical_design.md#infrastructure-strategy)
    - [1. Constraints and assumptions](tactical_design.md#1.-constraints-and-assumptions)
      - [State assumptions](tactical_design.md#state-assumptions)
      - [Calculate Read/Write usage](tactical_design.md#calculate-read/Write-usage)
    - [2. System Design: InfraStructure](tactical_design.md#2.-system-design:-infraStructure)
      - [Service Mesh Pattern](tactical_design.md#service-mesh-pattern)
    - [2.1 Infrastructure - Traffic](tactical_design.md#2.1-infrastructure---traffic)
      - [North-South Traffic Management](tactical_design.md#north-South-traffic-management)
      - [West-East Traffic Management](tactical_design.md#west-East-traffic-management)
    - [2.2 Infrastructure - Security](tactical_design.md#2.2-infrastructure---security)
      - [Authentication/Authorization Flow](tactical_design.md#Authentication/Authorization-flow)
    - [2.3 Infrastructure - Service-Level (Sidecar)](tactical_design.md#2.3-infrastructure---sidecar)
    - [2.4 InfraStructure - Monitoring](tactical_design.md#2.4-infraStructure---monitoring)
      - [Metrics](tactical_design.md#metrics)
      - [Tracing](tactical_design.md#tracing)
      - [Logging](tactical_design.md#logging)
    - [3. Scale the design](tactical_design.md#3.-scale-the-design)
      - [Infrastructure scale](tactical_design.md#infrastructure-scale)
      - [DB scale](tactical_design.md#db-scale)
      - [Cache](tactical_design.md#cache)
  - Contexts
    - [Stocks (Core domain)](/stock_context.md)
    - Identity (Supporting domain)

## Check Business Idea

Check other products' pros and cons in order to understand which direction I can head on and how many rooms of opportunity I can have. Details discussions are not listed here since PM is not my career goal. Nevertheless, I did explore and do some research in advance.

## Strategic Design

![Strategic Design in DDD](https://drive.google.com/uc?export=view&id=19-pno17L98jpnhwYyRZShlt5ZhUlZDgN&sz=w300-h50)

Ref: https://ithelp.ithome.com.tw/m/articles/10216792

Main objective of DDD is to define Bounded contexts, the Ubiquitous Language and the Context Maps together. They help me to identity the relation between problem I have faced and solution I can apply. Not only leveraging from DDD, but I also take advantage of use-case and user story. All the tools I have can really help me to build a large scale system from scratch. To be more specific, from use-case to functional program.

Check more details in [Strategic Design](strategic_degisn.md)

## Tactical Design

![Tactical Design in DDD](https://drive.google.com/uc?id=1jaJUP7Az5St4nsyedx14y4zhHN25uUTN&sz=w300)
Ref: https://ithelp.ithome.com.tw/m/articles/10216792

In addition to simply applying tactical design in DDD, I merge the structure of system-design interview into my own version of "tactical design".

Generally, every decision are accompany with tradeoffs and alternatives. I will list them all in each stage and explain why a choice is the best for me. Since the other options are ready there, I can quickly migrate from current setup to others based on observations that are mainly from system metric from time to time. The topics contain key point of a system, e.g. core component and scale strategy.

Check more details in [Tactical Design](tactical_design.md)

## License

GNU GPL
This document should be used as reference in my future interview and this purpose only.
