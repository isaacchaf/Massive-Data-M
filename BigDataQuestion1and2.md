# A new paradigm for Big Data

### 1.	What does the Lambda Architecture provide?

The Lambda Architecture provides a general-purpose approach to implementing
an arbitrary function on an arbitrary dataset and having the function return its results
with low latency. That doesn’t mean you’ll always use the exact same technologies
every time you implement a data system. The specific technologies you use might
change depending on your requirements. But the Lambda Architecture defines a consistent approach to choosing those technologies and to wiring them together to meet
your requirements.

### 2.	What are the desired properties of a Big Data system?
Robustness and fault tolerance

Low latency reads and updates: you need to be able to achieve low latency reads and updates without compromising the robustness of the system. 

Scalability: The Lambda Architecture is horizontally scalable across all layers of the system stack: scaling is accomplished by adding more machines. 

Generalization: Because the Lambda Architecture is based on functions of all data, it generalizes to all applications, whether financial management systems, social media analytics, scientific applications, social networking, or anything else.

Extensibility: Extensible systems allow functionality to be added with a minimal development cost. 

Ad hoc queries: Being able to mine a dataset arbitrarily with fully incremental architectures gives opportunities for business optimization and new applications.

Minimal maintenance

Debuggability

### 3.	What are the problems with fully incremental architectures?

Operational complexity:
Reclaiming space as soon as it becomes unused is too expensive, so the space is occasionally reclaimed in bulk in a process called compaction. Compaction is an intensive operation. 

Extreme complexity of achieving eventual consistency: handling eventual consistency in incremental, highly available systems is unintuitive and prone to error. 

Lack of human-fault tolerance: An incremental system is constantly modifying the state it keeps in the database, which means a mistake can also modify the state in the database. Because mistakes are inevitable, the database in a fully incremental architecture is guaranteed to be corrupted. 

### 4.	What are the differences between a fully incremental solution and a Lambda Architecture solution?

The two solutions can be compared on three axes: accuracy, latency, and throughput. The Lambda Architecture solution is significantly better in all respects. Both
must make approximations, but the fully incremental version is forced to use an inferior approximation technique with a 3–5x worse error rate. Performing queries is significantly more expensive in the fully incremental version, affecting both latency and
throughput. But the most striking difference between the two approaches is the fully
incremental version’s need to use special hardware to achieve anywhere close to reasonable throughput. Because the fully incremental version must do many random
access lookups to resolve queries, it’s practically required to use solid state drives to
avoid becoming bottlenecked on disk seeks.
 That a Lambda Architecture can produce solutions with higher performance in
every respect, while also avoiding the complexity that plagues fully incremental architectures.

### 5.	What is the main idea in the Lambda Architecture? 

The main idea of the Lambda Architecture is to build Big Data systems as a series of layers. Each layer satisfies a subset of the properties and builds upon the functionality provided by the layers beneath it. 

### 6.	What is a batch layer? 

The portion of the Lambda Architecture that implements the batch view = function(all data) equation is called the batch layer. The batch layer stores the master copy of the dataset and precomputes batch views on that master dataset. The master dataset can be thought of as a very large list of records.

### 7.	What process are needed for a Batch Layer?

The batch layer needs to be able to do two things: store an immutable, constantly growing master dataset, and compute arbitrary functions on that dataset. This type of processing is best done using batch-processing systems

### 8.	What is the function of a Serving layer?
The serving layer is a specialized distributed database that loads in a batch view and makes it possible to do random reads on it. ). When new batch views are available, the serving layer automatically swaps those in so that more up-to-date results are available. 

### 9.	What are the benefits of the batch and serving layers?

The beauty of the batch and serving layers is that they satisfy almost all the properties you want with a simple and easy-to-understand approach. There are no concurrency issues to deal with, and it scales trivially. The only property missing is low latency updates. The final layer, the speed layer, fixes this problem. 

### 10.	What is a speed layer?

As its name suggests, its goal is to ensure new data is represented in query functions as quickly as needed for the application requirements. It is similar to the batch layer in that it produces views based on data it receives. One big difference is that the speed layer only looks at recent data, whereas the batch layer looks at all the data at once. 


# Data Model for Big Data

### 11.	What is the advantage of raw of data?

Storing raw data is hugely valuable because you rarely know in advance all the questions you want answered. By keeping the rawest data possible, you maximize your ability to obtain new insights, whereas summarizing, overwriting, or deleting information limits what your data can tell you. The trade-off is that rawer data typically entails more of it is sometimes much more. But Big Data technologies are designed to manage petabytes and exabytes of data


### 12.	What are the advantages and disadvantages of immutability of Data?

One important advantage is that data is Human-fault tolerant, with a mutable data model, a mistake can cause data to be lost, because values are actually overridden in the database. With an immutable data model, no data can be lost. If bad data is written, earlier (good) data units still exist.
 The other advantage is the simplicity: mutable data models imply that the data must be indexed in some way so that specific data objects can be retrieved and updated. In contrast, with an immutable data model you only need the ability to append new data units to the master dataset.

One of the trade-offs of the immutable approach is that it uses more storage than a mutable 
schema. First, the user ID is specified for every property, rather than just once per row, as with a mutable approach. 

### 13.	What is a consequence of immutability of Data?

The key consequence of immutability is that each piece of data is true in perpetuity. That is, a piece of data, once true, must always be true.

### 14.	What is a characteristic of facts in big Data?

Facts are atomic because they can’t be subdivided further into meaningful components. are represented as multiple, independent facts. As a consequence of being atomic, there’s no redundancy of information across distinct facts. 

### 15.	What are the benefits of a fact-based model?

Is queryable at any time in its history 
Tolerates human errors 
Handles partial information
Has the advantages of both normalized and denormalized forms 

