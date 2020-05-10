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
## Distributed Systems


---
## Scaling

#### Vertical


#### Horizontal


---
#### Monolith


#### Microservice


---
## Load Balancing


---
## Consistent Hashing


---

## Message Queues


## Publisher / Subscriber
- allow for filtering of events so that components can subscribe to only the events they need

---
## Database Sharding


---
## Distributed Caching


---
## Notes