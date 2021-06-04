# Stock Context

## Step0: Other similar design

## Step1. Strategy

### Constraints and assumptions

#### Use cases

- Requestor can serach stock based on ticker/name

#### State assumptions

[Guide: State assumptions](https://hackmd.io/oWWAU1yHTBiCUuOiduN-Fg)

- User Operation Profile
  - Serving from cache requires fast lookups
  - Limited memory in cache
    - Need to determine what to keep/remove
    - Need to cache millions of queries
  - Stock data will be updated daily while individual company is going to release report per season.
  - Traffic is not evenly distributed
    - Some popular stock will be fetched more than others. Popular queries should almost always be in the cache
    - Need to determine how to expire/refresh
- Content assumptions
  - ~7000 company
  - 2500 query per month per user
  - 500 hundreds company are popular
  - 200K Users

#### Calculate usage

[Guide: Calculate usage](https://hackmd.io/cz1GW_NhQC6W9VdGXqERdA)

- 200 read request per second
- cache data per month
  - Cache stores of key

### Bounded Context Deployables

![Deployables](https://drive.google.com/uc?export=view&id=18j8rD7JOmfr55tDI9POQTiO2gCg99gOr)

## Step2: Details of System Design

### Cache Flow Design

#### Update strategy

- Options
  - Cache-aside
  - Read-Through
- Decision
  - Read-Through

### State Machine

- Decision
  - Stateless application, thus no need preserve state.

### Operation Function

#### ReadStockByTicker

- Request
  ```
  GET /v1/stock?stockVID=MSFT
  ```
- Headers
  ```
  {}
  ```
- Response
  ```
  {
      catalog:{
          ticker:"MSFT",
          profitDetails:{netIncome:{"2020":"1233","2019","1222",...}}
          safetyDetails:{...}
      }
  }
  ```

### API Sequence Diagram

- Decision
  - ![Sequence Diagram](https://drive.google.com/uc?export=view&id=12zQN9IQy7jc41QOo1080mE7NIIdAI_Ka)

### UI flow or User walkthrough

TODO

### App mockup or UX wireframes

TODO

## Step3: Details of Component Design

### UML Overview

![UML Overview](https://drive.google.com/uc?export=view&id=166RSACDRWzp-IkW3EP2z7e2BBST4ctmg)

### DB design

#### DB choice

- Decision
  - MongoDB

#### ERD Schema Design

- Decision
  - ![ERD Schema](https://drive.google.com/uc?export=view&id=1rUPeObg_7Syw0-MTF6RcqjQqXAl4SchW)

#### SQL/NoSQL Tuning

- Options
  - Schema Design and Indexing
  - Bulk Writes and Reads

### Redis design

#### Pipeline: Construct Stock Catalog

- Options
  TODO
- Decisions
  TODO

#### Set local buffering / caching

- Options
  TODO
- Decisions
  TODO

#### How to choose Redis Datatype

- Options
  TODO
- Decisions
  TODO

#### Reasonable TTL

- Options
  TODO
- Decisions
  TODO

#### Cache Expiration Strategy

- Options
  - Expiration Types
    - Timing deletion
    - Inert deletion
    - Periodic deletion
  - Eviction Policy
    - noeviction
    - allkeys-lru
    - volatile-lru
    - allkeys-random
    - volatile-random
    - volatile-ttl
- Decisions
  - Expiration Types
    - Timing deletion
    - Inert deletion
  - Eviction Policy
    - allkeys-lru

#### Warm-up Cache

- Options
  - Warming inside the app
  - Warming outside the app
- Decisions
  - RDB

#### Persistence options

- Options
  - RDB
  - AOF
- Decision
  - AOF

### Class Diagram

![Aggregate](https://drive.google.com/uc?export=view&id=166RSACDRWzp-IkW3EP2z7e2BBST4ctmg)

## Step4: Test

### Unit Test

- Repo Test
- DB_Connection Test
- UseCase Test

### Intrgreation Test

TODO
