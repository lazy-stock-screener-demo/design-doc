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
  - [Identify User Story in Bounded Context in terms of role/permission based on UseCase](#identify-user-story-in-bounded-context-in-terms-of-role)
- [User Story and Worker](#user-story-and-worker)
  - [User Story](#user-story)
    - [Stock Catalog Context](#stock-catalog-context)
    - [Customer Self Context](#customer-self-context)
    - [Customer Identity Context](#customer-identity-context)
- [User Story and Operation/Transaction](#user-story-and-operation/transaction-mapping)
  - [Customer can search stock based on ticker/name](#Customer-can-search-stock-based-on-ticker)
  - [Customer can login to check their dashboard, User can change his profile and setting and delete his account](#Customer-can-login-to-check-their-dashboard,-User-can-change-his-profile-and-setting-and-delete-his-account)

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

### Identify User Story in Bounded Context in terms of role

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

#### Get Stock Meta by stock ticker

As a Guest/Std/Pro/Elite Requestor, I want to read Stock Meta data by stock ticker/name so that these data can be viewed by customers.

| User Story | ViewStockMeta                                  |
| ---------- | ---------------------------------------------- |
| Given      | Guest/Std/Pro/Elite Requestor                  |
| And        |                                                |
| When       | Guest/Std/Pro/Elite Requestor fetch stock data |
| Then       | a stock data will return.                      |

#### Search worker

As server start, a server will start a worker to build stock search index based on ticker, so that a Requestor can get an auto-complete stock name.
| User Story | createStockIndex |
| ---------- | ------------------------------------------ |
| Given | Authenticated customer |
| And | a validate auth token |
| When | Authenticated customer signout |
| Then | an authenticated customer state is deleted |

### Customer Self Context

- Domain: Identity
- Roles in this context
  - Guest/Std/Pro/Elite customer

#### Register a new account

As a Guest customer, I want to sign up a new account, so that the system can track down my personal preference.

| User Story | CreateNewAccount                    |
| ---------- | ----------------------------------- |
| Given      | Guest customer                      |
| And        |                                     |
| When       | Guest customer signup               |
| Then       | an authenticated customer is added. |

### Customer Identity Context

- Domain: Identity
- Roles in this context
  - Guest customer
  - Authenticated customer

#### Sign in

As a Guest customer, I want to sign in my account, so that the system can track down my personal preference.

| User Story | AuthCustomer                       |
| ---------- | ---------------------------------- |
| Given      | Guest customer                     |
| And        |                                    |
| When       | Guest customer signin              |
| Then       | an authenticated customer is added |

#### Sign out

As an Authenticated Customer, I want to sign in my account, so that the system can track down my personal preference.

| User Story | Signout                                    |
| ---------- | ------------------------------------------ |
| Given      | Authenticated customer                     |
| And        | a validate auth token                      |
| When       | Authenticated customer signout             |
| Then       | an authenticated customer state is deleted |

---

## User Story and Operation/Transaction Mapping

The final step combined the following things together: use-case, user story, corresponding domain and context.

### Non-Transactions - Use Case

It Should be combine with several user story.

#### Customer can search stock based on ticker

- Domain: Stocks
- Context: Stock Catalog Context

| User Story    | Actor / Event that trigger operation            | System Operation                                           | Operation Event                                                |
| ------------- | ----------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------- |
| Search Worker | TickerAdded<br/>TickerUpdated<br/>TickerRemoved | AddSearchIndex<br/>UpdateSearchIndex<br/>RemoveSearchIndex | SearchIndexAdded<br/>SearchIndexUpdated<br/>SearchIndexRemoved |
| ViewStockMeta | Guest/Std/Pro/Elite Requestor                   | GetStockMetaByName<br/>GetStockMetaByTickerId              | SearchMetaAdded                                                |

#### Customer can login to check their dashboard, User can change his profile and setting and delete his account

- Domain: Identity
- Context: Customer-self context and Customer-identity context

| User Story   | Actor / Event that trigger operation | System Operation | Operation Event |
| ------------ | ------------------------------------ | ---------------- | --------------- |
| AuthCustomer | Authenticated customer               | AuthCustomer     | CustomerAuthed  |

### Transactions

"Purchase license saga" is shown here in order to demonstrate how to deal with transaction.

- Domain: Purchasing
- Context: License purchasing

| User Story                                                 | Actor / Event that trigger operation               | System Operation                               | Operation Event                                    | Compensation Operation | Compensation Operation Event |
| ---------------------------------------------------------- | -------------------------------------------------- | ---------------------------------------------- | -------------------------------------------------- | ---------------------- | ---------------------------- |
| AuthCustomer(Txn1)                                         |                                                    | AuthCustomer                                   | CustomerAuthed                                     |                        |
| ViewPruchaseAbleLicenses                                   | Std/Pro Purchaser                                  | GetLicensesList                                |
| PlaceLicenseOrder(Txn2)<br/>PlaceExpenseLicenseOrder(Txn2) | Std/Pro Purchaser                                  | PlaceLicenseOrder<br/>PlaceExpenseLicenseOrder | LicenseOrderCreated<br/>ExpenseLicenseOrderCreated |
| AuthorizePayment Worker(Txn3)                              | LicenseOrderCreated<br/>ExpenseLicenseOrderCreated | AuthorizePayment                               | PaymentAuthorized                                  | RejectPayment          | PaymentRejected              |
| ApproveOrder Worker (Txn4)                                 | PaymentAuthorized                                  | ApproveOrder                                   | OrderApproved                                      | RejectOrder            | OrderRejected                |
| UpdateLicense Worker(Txn5)                                 | OrderApproved                                      | UpgradeLicense                                 | LicenseUpgrade                                     | DegradeLicense         | LicenseDegraded              |
