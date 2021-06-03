# Strategic Design

## Index of strategic design

- [Problem Space](#problem-space)
  - [Identify full end-to-end business use case](#identify-full-end-to-end-business-use-case)
  - [System Context Diagram](#system-context-diagram)
  - [Domain Design](#solution-space)
- [Solution Space](#solution-space)
  - [Build Bounded Context based on problem space in domain model](#bounded-context)
  - [Build Bounded Context Map](#bounded-context-map)
  - [Define Ubiquitous Language in Bounded Context](#ubiquitous-language-in-bounded-context)
  - [Identify Use Story through Event Storming](#identify-use-story-through-event-storming)
  - [Identify User Story in Bounded Context in terms of role/permission based on UseCase](#Identify-User-Story-in-Bounded-Context-in-terms-of-role/permission)
- [User Story and Worker](#user-story-and-worker)
  - [User Story](#user-story)
    - Stock Catalog Context
    - Customer Self Context
    - Customer Identity Context
  - Worker Story
    - Stock Catalog Context
- User Story and Operation/Transaction
  - Customer can search stock based on ticker/name
  - Customer can login to check their dashboard, User can change his profile and setting and delete his account.

---

## Problem Space

### Identify full end-to-end business use case

#### Key Use Cases

- Stocks
  - Customer can search stock based on ticker/name
- Identity
  - Customer can login to check their dashboard, Customer can change his profile and setting and delete his account.

### System Context Diagram

![Context Diagram](https://drive.google.com/uc?export=view&id=11aahv1AVkL5KmEKIhOmOG_niSMf9ewPd)

### Domain Design

Identify problem space based on use-case.
![Domain Space](https://drive.google.com/uc?export=view&id=1szCzGOaO-4HMiGlpMG9w3wcPiCl3m7RP)

- Core Domain
  - Stocks
- Support Domain
  - Identity

---

## Solution Space

### Bounded Context

This is where we are.
![Bounded Context](https://drive.google.com/uc?export=view&id=1CQNPOaMt0EatZdqk7f0hSYfFUJtQ01av)

Build Bounded Context based on given problem space
![DDD Strategic Modeling](https://drive.google.com/uc?export=view&id=1d-szfAEt007TA4P8guNXtJH7btEh9g5V)

- Stocks (Core domain)
  - Stock Catalog Context
- Identity (Supporting domain)
  - Customer Identity Context

### Bounded Context Map

This is where we are.
![Bounded Context Map](https://drive.google.com/uc?export=view&id=1Ia7pqXzZqDuesVp5UKYyHXSdrk6RhMqx)

![Bounded Context Map](https://drive.google.com/uc?export=view&id=1depvWascxHivu9x4g5wqLAFWWQVSQ8uc)

### Ubiquitous Language in Bounded Context

Details are shown in the "User Story in Bounded Context" sections.

### Identify Use Story through Event Storming

Step 1. Create Events:

![Event Storming 1](https://drive.google.com/uc?export=view&id=1iZ3YWvimrBxGxtJiAChZkKzfWI3kJsMC)

Step 2. Reorganized orders of commands and events
![Event Storming 2](https://drive.google.com/uc?export=view&id=1EuV8FM4EHdas2aqCi0YqfHAuFiRtBz_L)
![Event Storming 3](https://drive.google.com/uc?export=view&id=1fzI3wXAEqzxSCdvmHYjYnwuiNcSKzMPL)

Step 3. Went through all the events

Step 4. Mark those Questions or Opportunity

Step 5. Add More Events

### Identify User Story in Bounded Context in terms of role/permission

Add role/permission level allow you manage editing/viewing permission for different group of people.

- Stock Catalog Context
  - Frontend Components
    - UI
      - Stock Catalog Page
      - Stock Search Component
    - View Model
      - Stock Search Model
      - Stock Catalog Model
  - Role
    - Guest Requestor
    - Std Requestor
    - Pro Requestor
    - Elite Requestor
  - Worker
    - Search worker
      - can build index in search index file to boost the performance of searching
- Customer Identity Context
  - UI
    - Login Pages
    - Register Pages
  - View Model
    - Login Model
    - Register Model
  - Role
    - Guest customer
      - UI Logic
      - api command permission
        - [can register as new customer](https://hackmd.io/ehhqc9QXTIC_99rqqAzTUg#Register-a-new-account)
        - [can register as new customer by Google](https://hackmd.io/ehhqc9QXTIC_99rqqAzTUg?both#Register-a-new-account-by-google)
        - [can register as new customer by FB](https://hackmd.io/ehhqc9QXTIC_99rqqAzTUg?both#Register-a-new-account-by-fb)
        - [can login/logout as a customer](https://hackmd.io/ehhqc9QXTIC_99rqqAzTUg#Login)
      - api command permission
    - Authenticated customer
      - UI logic
      - api command permission
      - api query permission

---

## User Story

### Stock Catalog Context

- Domain: Stock
- Roles in this context
  - Guest/Std/Pro/Elite Requestor

#### ViewStockMeta

As a Guest/Std/Pro/Elite Requestor, I want to read Stock Meta data by stock ticker/name so that these data can be viewed by customers.

| User Story | ViewStockMeta                                  |
| ---------- | ---------------------------------------------- |
| Given      | Guest/Std/Pro/Elite Requestor                  |
| And        |                                                |
| When       | Guest/Std/Pro/Elite Requestor fetch stock data |
| Then       | a stock data will return.                      |

### Customer Self Context

## Worker Story

### Stock Catalog Context

---

## User Story and Operation/Transaction Mapping

### Non-Transactions - Use Case

It Should be combine with several user story.

- Customer can search stock based on ticker/name
