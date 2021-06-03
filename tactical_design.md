# Tactical Design

## Index of strategic design

- Infrastructure Strategy
  - 1.Constraints and assumptions
  - 2.System Design: InfraStructure
    - Service Mesh Pattern
  - 2.1 Infrastructure - Traffic
  - 2.2 Infrastructure - Security
  - 2.3 Infrastructure - Service-Level (Sidecar)
  - 2.4 InfraStructure - Monitoring
  - 3.Scale the design
- Contexts
  - Stocks (Core domain)
  - Identity (Supporting domain)

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

### 2.3 Infrastructure - Service-Level (Sidecar)

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

- Stocks (Core domain)
- Identity (Supporting domain)

#### Obstacles to decomposing an application into services

On the surface, the strategy of creating a micro-service architecture by defining services corresponding to business capabilities or subdomains looks straightforward. You may, however, encounter several obstacles:

- Network latency
- Reduced availability due to synchronous communication
- Maintaining data consistency across services
- Obtaining a consistent view of the data
- God classes preventing decomposition
