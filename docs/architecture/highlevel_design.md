# High-level Design
System design


---
## Orthagonal Architecture
- decoupling (you can change the UI or database without affecting the other)
    - two different user interfaces should be able to be supported by the same underlying code base
- design components that are self-contained, independent, and with a single, well-defined purpose
    - when components are isolated then one can change wihtout affecting the others
    - allow components to communicate based on external interfaces that are consistent
- module design - if one module is comprimised it will not affect the others
- temporal decoupling - allow for concurrency to reduce time-based dependencies of a system
- gain in productivity and reduces risk

---
## Iterative Design




---
## The Twelve-Factor App
[Website](https://12factor.net/)

1. Codebase: One codebase tracked in revision control, many deploys
    -
2. Dependencies: Explicitly declare and isolate dependencies
    -
3. Config: Store config in the environment
    -
4. Backing services: Treat backing services as attached resources
    -
5. Build, release, run: Strictly separate build and run stages
    -
6. Process: Execute the app as one or more stateless processes
    -
7. Port binding: Export services via port binding
    -
8. Concurrency: Scale out via the process model
    -
9. Disposability: Maximize robustness with fast startup and graceful shutdown
    -
10. Dev/prod parity: Keep development, staging, and production as similar as possible
    -
11. Logs: Treat logs as event streams
    -
12. Admin processes: Run admin/management tasks as one-off processes
    -

---
## Tiers vs. Layers
- tiers involve physical separation of components in a system (UI, backend server, database, cache, message queues, load balancers, search, data processing, shared services)
- layers of an application represent the organization of the code (user interface, business logic, service, or data access layer)


---
## Distributed Systems


---
## Scalability
- the ability of an application to handle & withstand increased workload without sacrificing latency
- if the app takes x seconds to respond to a user reqiest, it should take the same x seconds to respond to each of the million concurrent user requests on the app

#### Vertical
- adding more compute power
- does not require code refactoring

#### Horizontal
- adding more nodes/hardware to existing hardware resource pool
- requires code refactoring to utilize distributed resources
- higher availability

---
#### Monolith
Should be the default choice unless there exists a very good reason to incorporate a microservices architecture - (Sam Newman & Martin Fowler goto; talk)[https://www.youtube.com/watch?v=GBTdnfD6s5Q]  

- Simpler to build, test and deploy
- Weaknesses:
    - small code changes require an entire redeployment of the application
    - requires thorough regression testing
    - introduces single points of failure
    - flexibility and scalability are harder to achieve
    - limits the use of the application using heterogenous technologies
    - tend to be stateful and therefore making it more difficult to be cloud-native and distributed

#### Microservice
Can be incorporated incrementally by migrating small parts of the monolith application to microservices like a dial not an on/off switch - (Sam Newman & Martin Fowler goto; talk)[https://www.youtube.com/watch?v=GBTdnfD6s5Q]   

- Fault isolation
- Easier management
- Isolated development and testing
- Ease of adding new features
- Ease of maintenance
- High availability
- Separated responsiblity/concerns (separate teams gain increased productivity and complete ownership)
- Increased scalability options
- Weaknesses:
    - greatly increased complexity in management and monitoring of the system (distributed logging, networking, service discovery, CICD, alerts, tracing, health checks, etc.)
    - requires more components or code to manage and utilize the distributed nodes
    - requires more human resources to manage the more complex system
    - storing consistency is hard to guarentee in a distributed environment (leans on eventual consistency)

---
## Load Balancing


---
## Caching
- reduces latency, stores data in memory instead of accessing disk - O(1) fetches
- avoids over computing and rerunning joins in SQL which greatly reduces latency
- can continue to serve data requests even when the database server goes down
- can be implanted at any application later - OS, network, CDN, database, client-side, cross-module communication between services in a microservice architecture

### Distributed Caching


---
## Data Storage
- use read-only replica databases to reduce read traffic and as a source for business/financial applications (can be promoted for disaster recovery)

### Data Partitioning

### Database Sharding


### Indexes


### Proxies


### Redundancy and Replication


### SQL vs. NoSQL
- RDBs:
    - strong consistency
    - ACID transactions
    - great for handling relationships
    - scalable with caching (especially read-heavy applications)
- NoSQL
    - eventual consistency
    - horizontal scalability
    - great for read/write-heavy applications with little relationships

### CAP Theorem


### Consistent Hashing


---
## Message Queues
- Allows for communication in an environment with heterogeneous technology
- Enables asynchronous behavior in a service to run background processes, tasks, and batch jobs
- Cross-module communication in a microservice architecture
- Exchanges handle message queue logic
    - use binding to deliver messages to queues

### Publisher / Subscriber Model
- one to many (broadcast)
- allow for filtering of events so that components can subscribe to only the events they need

### Point to Point Model
- one to one (entity relationship)

---
## High Availability
- Active-Passive HA mode: set of redundant components in standby node awaiting automatic failover
- Active-Active HA Mode: set of replicated components that share the workload of the system
- Montior: to detect single points of failure and unhealthy components in the system
- Automation: to allow for the system to conduct self-healing or handle increased/decreased workloads

---
## System Testing
- network bandwidth consumption
- throughput
- requests processed within a time range
- latency
- application memory/CPU usage
- end-user experience when the system is under a heavy load

---
## Reducing Bottlenecks
- horizontally scale workloads servers
- distributed databases
- asynchronous processes & modules wherever possible (reduce unecessary sequential steps)
- data compression
- caching (write-through cache) - only hit a database when it is really required
- use a CDN
- load balancers & efficient configuration
- picking the right databases
- code refactoring
- decoupling
- dynamic analysis / profiling (application profiler, code profiler) to see which processes are taking too long and consuming too many resources
- avoid unnecessary client-server requests (group them together if possible)

---
## Infrastructure Changes
- should include
    - impact analysis
    - rollback plan
    - disaster recovery plan
    - code review / discussion


---
## Cloud Cost Hygiene
- naming conventions
- tags (to associate resources to cost centers)
    - group
    - lifecycle
    - person
    - application
- IT Governance rules
- establish user roles and controls
- billing alarms

### Storage Costs Considerations
- storage capacity provisioned/used
- data transfer
- replication
- access patterns

---
## Types of Architecture

### Client-server

### Peer to Peer (P2P)
- blockchain
- node to node connections

### Federated
- extension of decenteralized architecture
- nodes are grouped and connect to pod servers that enable node discovery
- pods connect to each other to form the backbone of the network

### Event-driven
- reactive programming (Tornado, Nodejs, akka, etc.)

---
## Notes