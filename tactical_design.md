# Tactical Design

## Index of strategic design

- [Infrastructure Strategy](#infrastructure-strategy)
  - [1. Constraints and assumptions](#1.-constraints-and-assumptions)
    - [State assumptions](#state-assumptions)
    - [Calculate Read/Write usage](#calculate-read/Write-usage)
  - [2. System Design: InfraStructure](#2.-system-design:-infraStructure)
    - [Service Mesh Pattern](#service-mesh-pattern)
  - [2.1 Infrastructure - Traffic](#2.1-infrastructure---traffic)
    - [North-South Traffic Management](#north-South-traffic-management)
    - [West-East Traffic Management](#west-East-traffic-management)
  - [2.2 Infrastructure - Security](#2.2-infrastructure---security)
    - [Authentication/Authorization Flow](#Authentication/Authorization-flow)
  - [2.3 Infrastructure - Service-Level (Sidecar)](#2.3-infrastructure---sidecar)
  - [2.4 InfraStructure - Monitoring](#2.4-infraStructure---monitoring)
    - [Metrics](#metrics)
    - [Tracing](#tracing)
    - [Logging](#logging)
  - [3. Scale the design](#3.-scale-the-design)
    - [Infrastructure scale](#infrastructure-scale)
    - [DB scale](#db-scale)
    - [Cache](#cache)
- Contexts
  - [Stocks (Core domain)](/stock_context.md)
    - Step0: Other similar design
    - [Step1. Strategy](stock_context.md#step1.-strategy)
      - [Constraints and assumptions](stock_context.md#constraints-and-assumptions)
        - [Use cases](stock_context.md#use-cases)
        - [State assumptions](stock_context.md#state-assumptions)
        - [Calculate usage](stock_context.md#calculate-usage)
      - [Bounded Context Deployables](stock_context.md#bounded-context-deployables)
    - [Step2: Details of System Design](stock_context.md#step2:-details-of-system-design)
      - [Cache Flow Design](stock_context.md#cache-flow-design)
        - [Update strategy](stock_context.md#update-strategy)
      - [State Machine](stock_context.md#state-machine)
      - [Operation Function](stock_context.md#operation-function)
      - [API Sequence Diagram](stock_context.md#aPI-sequence-diagram)
      - [UI flow or User walkthrough](stock_context.md#UI-flow-or-user-walkthrough)
      - [App mockup or UX wireframes](stock_context.md#App-mockup-or-UX-wireframes)
    - [Step3: Details of Component Design](stock_context.md#Step3:-details-of-component-design)
      - [UML Overview](stock_context.md#UML-overview)
      - [DB design](stock_context.md#DB-design)
        - [DB choice](stock_context.md#DB-choice)
        - [ERD Schema Design](stock_context.md#ERD-schema-design)
        - [SQL/NoSQL Tuning](stock_context.md#SQL/NoSQL-tuning)
      - [Redis design](stock_context.md#Redis-design)
        - [Pipeline: Construct Stock Catalog](stock_context.md#Pipeline:-construct-stock-catalog)
        - [Set local buffering / caching](stock_context.md#Set-local-buffering-/-caching)
        - [How to choose Redis Datatype](stock_context.md#How-to-choose-redis-datatype)
        - [Reasonable TTL](stock_context.md#Reasonable-tTL)
        - [Cache Expiration Strategy](stock_context.md#Cache-expiration-strategy)
        - [Warm-up Cache](stock_context.md#Warm-up-cache)
        - [Persistence options](stock_context.md#Persistence-options)
      - [Class Diagram](stock_context.md#Class-diagram)
    - [Step4: Test](stock_context.md#Step4:-test)
      - [Unit Test](stock_context.md#Unit-test)
      - [Intrgreation Test](stock_context.md#Intrgreation-test)
  - Identity (Supporting domain)

---

## Infrastructure Strategy

### 1. Constraints and assumptions

#### State assumptions

- Traffic
  - Traffic is not evenly distributed
  - Low latency between machines
  - cache requires fast lookups
- How many users?
  - Depends on real case, just list all the options.
- How many queries per month?
  - Depends on real case, just list all the options.

#### Calculate Read/Write usage

See below sections.

### 2. System Design: InfraStructure

#### Service Mesh Pattern

- Options
  - Embedded/In-process Service Mesh
  - [x] Out-of-process service mesh

![Service Mesh](https://drive.google.com/uc?export=view&id=1_UuHNC_lRB8JGF7KgWH3AwvymQ-SoFXT)

### 2.1 Infrastructure - Traffic

#### North-South Traffic Management

![Service Mesh](https://drive.google.com/uc?export=view&id=19cWDWeO8ocrsiM6YYw73y5tvLg-t6n3l)

Ref.: https://ithelp.ithome.com.tw/articles/10224374

- Options
  - K8S ingress resource + e.g. NGINX ingress controller
  - K8S Ingress Resources + ISTIO Gateway
  - ISTIO gateway + ISTIO virtualservice
  - K8S gateway API + API Gateway (L7) + ISTIO virtual Service (Routing VirtualService) (not sure if it is the right combination)

#### West-East Traffic Management

- Options
  - ISTIO Envoy proxy

### 2.2 Infrastructure - Security

#### Authentication/Authorization Flow

TODO

### 2.3 Infrastructure - Sidecar

TODO

### 2.4 InfraStructure - Monitoring

- Ref.
  - [https://dzone.com/articles/metadata-management-in-big-data-systems-a-complete-1](https://dzone.com/articles/metadata-management-in-big-data-systems-a-complete-1)
  - [https://dzone.com/articles/metadata-management-in-big-data-systems-a-complete-1](https://dzone.com/articles/metadata-management-in-big-data-systems-a-complete-1)
  - [https://medium.com/avmconsulting-blog/how-to-deploy-an-efk-stack-to-kubernetes-ebc1b539d063](https://medium.com/avmconsulting-blog/how-to-deploy-an-efk-stack-to-kubernetes-ebc1b539d063)

#### Metrics

- Prometheas
- Grafana

#### Tracing

- Zipkin

#### Logging

- Fluentd
- Elasticsearch
- Kibana

### 3. Scale the design

Ref.: [system-design-primer](https://github.com/donnemartin/system-design-primer/tree/master/solutions/system_design/scaling_aws)

#### Infrastructure scale

- Guide
  - At different traffic level, keep doing these:
    1. Benchmark/Load Test,
    2. Profile for bottlenecks
    3. address bottlenecks while evaluating alternatives and trade-offs
    4. repeat
  - I'll introduce some components to complete the design and to address scalability issues.
- Options
  - Level 0: traffic: 1-2 read request per second
    - Basic principle:
      - Vertical scaling when needed
      - Monitor to determine bottlenecks
      - Start with SQL
    - Components
      - DNS
      - Web Server
      - SQL as database
    - Cons
      - Scaling vertically can get very expensive
      - No redundancy/failover
- Level 1: traffic: 50 average read requests per second
  - Basic principle:
    - Lighten component into single box that allow easily independent scaling.
    - Store content separately
    - Secure the system
  - Components
    - DNS
    - Web Server
    - **SQL as database and Use RDS in AWS or Cloud SQL in GCP (Statefulset in K8S maybe ok?)**
    - **Object store for static content**
  - Cons
    - increase complexity
    - additional security measures must be taken to secure the new components
    - costs could also increase
- Level 2: 200 average read requests per second
  - Assumptions
    - As the service matures, we’d also like to move towards higher availability and redundancy.
  - Basic principle:
    - Use Horizontal Scaling to handle increasing loads and to address single points of failure
    - Horizontal Scaling
      - multiple Web Servers
      - multiple SQL instances across AZ to improve redundancy
    - Move static content to object store and add CDN
  - Components
    - DNS
    - **CDN+Object Store (S3 in AWS or Cloud Storage in GCP)**
    - **Load Balancer**
    - WebServer
    - API Server
    - SQL (Write main-follower)
- Level 3: 3000 average read requests per second
  - Assumption
    - read-heavy (100:1 with writes) and our database is suffering from poor performance from the high read requests.
  - Basic principle
    - Move the following data to a Memory Cache such as Elasticache to reduce load and latency
    - Add SQL Replicas
    - More web servers and application servers
  - Components
    - DNS
    - CDN+Object Store (S3 in AWS or Cloud Storage in GCP)
    - Load Balancer
    - WebServer
    - API Server
    - **Memory Cache**
    - **SQL Read Replicas, Write Main-Follower**
- Level 4: 10000 average read requests per second
  - Assumption
    - spikes during regular business hours in the U.S. and drop significantly when users leave the office.
    - costs by automatically spinning up and down
  - Basic principle
    - Leverage Autoscaling
    - Add service mesh
  - Components
    - Like Level 3, but everything become autoscale
- Level 5: 40000 average read requests per second
  - Assumption
    - iteratively run Benchmarks/Load Tests and Profiling to uncover and address new bottlenecks.
    - handing unevenly distributed traffic and traffic spilkes
  - Basic principle
    - considering storing a limited time period of data in the database, while store the rest data in other places, e.g. data warehouse
    - separate popular content out and address them by scaling the memory cache
    - Need additional scaling techniques
  - Components
    - For handling write
      - Add async write API
      - Add Message queue
      - Add background worker
      - Add NoSQL
    - For handing read
      - SQL sharding
      - SQL federation

#### DB scale

- Ref.
  - [What is the difference between replication, partitioning, clustering, and sharding?](https://www.quora.com/What-is-the-difference-between-replication-partitioning-clustering-and-sharding)
- Options
- Level 3
  - Replication
- Level 4
  - Replication
  - Partitioning/Sharding
- Level 5
  - Replication
  - Partitioning/Sharding
  - Shard replicated across more than one node

#### Cache

- Ref.
  - [Introduction to Caches](https://docs.oracle.com/cd/E18686_01/coh.37/e18677/cache_intro.htm#COHDG5049)
- Guide
  - Reading 1 MB sequentially from memory takes about 250 microseconds, while reading from SSD takes 4x and from disk takes 80x longer. Adding cache to improve system efficiency.
- Options
  - Replicated Cache
    - Pros
      - best read performance
      - fault-tolerant
      - linear performance scalability for reads
    - Cons
      - poor write performance
      - additional network load
      - poor and limited scalability for writes
      - memory consumption
  - Distributed Cache
    - Pros
      - linear performance scalability for reads and writes
      - fault-tolerant
    - Cons
      - increased latency of reads(due to network round-trip and serialization/deserialization expenses)
  - Remote Cache
  - Near Cache
    - Pros
      - best used for read only data
    - Cons
      - increases memory usage since the near cache items need to be stored in the memory of the member
      - reduces consistency

---

## Contexts

### Define Service by applying the decompose by business capability pattern

- [Stocks (Core domain)](/stock_context.md)
- Identity (Supporting domain)

#### Obstacles to decomposing an application into services

On the surface, the strategy of creating a micro-service architecture by defining services corresponding to business capabilities or subdomains looks straightforward. You may, however, encounter several obstacles:

- Network latency
- Reduced availability due to synchronous communication
- Maintaining data consistency across services
- Obtaining a consistent view of the data
- God classes preventing decomposition
