# Designing Data-Intensive Applications
## Chapter 1:
### Reliable, Scalable and Mantainable Applications 

#### 1. What is the defference between fault and failure?
 A fault is usually defined as one component of the system deviating from its spec, whereas a failure is when the system as a whole stops providing the required service to the user. 
 
#### 2. Why adding redundancy will not prevent hardware problem?
Because when one component dies, the redundant component can take its place while the broken component is replaced, but it does not prevent the problem.

#### 3. What are the cascading failures?
where a small fault in one component triggers a fault in another component, which in turn triggers further faults. 

#### 5. What is monitoring?
Monitoring show us early warning signals and allow us to check whether any assumptions or constraints are being violated. When a problem occurs, metrics can be invaluable in diagnosing the issue.

#### 6. What is an example which you have to choose to sacrifice reliability in order to reduce development cost or  or operational cost ?
when developing a prototype product for an unproven market or for a service with a very narrow profit margin




