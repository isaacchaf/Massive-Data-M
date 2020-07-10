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


## Chapter 2:
### Data Models and Query Languages

#### 1. What is Map Reduce?
MapReduce is a programming model for processing large amounts of data in bulk across many machines, popularized by Google. A limited form of MapReduce is supported by some NoSQL datastores, including MongoDB and CouchDB, as amechanism for performing read-only queries across many documents.

#### 2. What are the drivinG forces to choose NoSQL?
• A need for greater scalability than relational databases can easily achieve, including very large datasets or very high write throughput
• A widespread preference for free and open source software over commercial database products
• Specialized query operations that are not well supported by the relational model
• Frustration with the restrictiveness of relational schemas, and a desire for a more dynamic and expressive data model 

#### 3. In the property graph model, what are the characteristics of the edeges and vertex?
For vertexes:
• A unique identifier
• A set of outgoing edges
• A set of incoming edges
• A collection of properties (key-value pairs)

For the edges
• A unique identifier
• The vertex at which the edge starts (the tail vertex)
The vertex at which the edge ends (the head vertex)
• A label to describe the kind of relationship between the two vertices
• A collection of properties (key-value pairs)

#### 4. What is Cypher?
Cypher is a declarative query language for property graphs, created for the Neo4j graph database.

#### 5. What are the two main types for NoSQL?
1. Document databases target use cases where data comes in self-contained documents and relationships between one document and another are rare.
2. Graph databases go in the opposite direction, targeting use cases where anything is potentially related to everything.

## Chapter 3:
### Storage and Retrieval

#### 1. Why reads are typically slower in LSM-trees?
Because they have to check several different data structures and SSTables at different stages of compaction.

#### 2. Why CSV are not good for log?
It’s faster and simpler to use a binary formatthat first encodes the length of a string in bytes, followed by the raw string (without need for escaping).

#### 3. What are the differences between B-Trees and LMS-trees?
 B-tree implementations are generally more mature than LSM-treeimplementations, LSM-trees are also interesting due to their performance characteristics. As a rule of thumb, LSM-trees are typically faster for writes, whereas B-trees are thought to be faster for reads. Reads are typically slower on LSM-trees because they have to check several different data structures and SSTables at different stages of compaction.
 
#### 4. What is a Data Werehouse?
A Data Werehouse is a separate database that analysts can query to theirhearts’ content, without affecting OLTP operations [48]. The data warehouse contains a read-only copy of the data in all the various OLTP systems in the company.

#### 5. What is OLPT?
OLTP (optimized for transaction processing) systems are typically user-facing, which means that they may see a huge volume of requests. In order to handle the load, applications usually only touch a small number of records in each query. The application requests records using some kind of key, and the storage engine uses an index to find the data for the requested key. Disk seek time is often the bottleneck here.

## Chapter 4:
### Encoding and Evolution 

### 1.	what is a rolling update?

It to deploy the new version to a few nodes at a time, checking whether the new version is running smoothly, and gradually working your way through all the nodes. This allows new versions to be deployed without service downtime, and thus encourages more frequent releases and better evolvability.

### 2.	According to the author, why is backward compatibility  not hard to achieve?

As author of the newer code, you know the format of data written by older code, and so you can explicitly handle it (if necessary, by simply keeping the old code to read the old data).

### 3.	Why forward compatibility can be trickier?

Because it requires older code to ignore additions made by a newer version of the code.

### 4.	What are the two formats for encoding data?

In memory, data is kept in objects, structs, lists, arrays, hash tables, trees, and so on. These data structures are optimized for efficient access and manipulation by the CPU (typically using pointers).

When you want to write data to a file or send it over the network, you have to encode it as some kind of self-contained sequence of bytes (for example, a JSON document). Since a pointer wouldn’t make sense to any other process

### 5.	What are some problems with encoding libraries?

The encoding is often tied to a particular programming language, and reading the data in another language is very difficult.

In order to restore data in the same object types, the decoding process needs to be able to instantiate arbitrary classes. This is frequently a source of security
problems.

CPU time taken to encode or decode, and the size of the encoded Structure

### 6.	Why CSV is a vague format?
CSV does not have any schema, so it is up to the application to define the meaning of each row and column. If an application change adds a new row or column, you have to handle that change manually



## Chapter 5:
### Replication

### 1.	What is the advantage of synchronous replication?

The advantage of synchronous replication is that the follower is guaranteed to have an up-to-date copy of the data that is consistent with the leader. If the leader suddenly fails, we can be sure that the data is still available on the follower.

### 2.	How to choose a new leader from the rest of the replicas, when the original leader fails?
This could be done through an election process (where the leader is chosen by a majority of the remaining replicas), or a new leader could be appointed by a previously elected controller node. The best candidate for leadership is usually the replica with the most up-to-date data changes from the old leader (to minimize any data loss).

### 3.	What is the problem with circular and star topologies? 
if just one node fails, it can interrupt the flow of replication messages between other nodes, causing them to be unable to communicate until the node is fixed. The topology could be reconfigured to work around the failed node, but in most deployments such reconfiguration would have to be done manually.

### 4.	What is a single-leader replication?
Clients send all writes to a single node (the leader), which sends a stream of data change events to the other replicas (followers). Reads can be performed on any replica, but reads from followers might be stale.

### 5.	What is a problem with asynchronous replication?
Although asynchronous replication can be fast when the system is running smoothly, it’s important to figure out what happens when replication lag increases, and servers fail. If a leader fails and you promote an asynchronously updated follower to be the new leader, recently committed data may be lost.

### 6.	How to detect that a leader has failed?
There is no foolproof way of detecting what has gone wrong, so most systems simply use a timeout: nodes frequently bounce messages back and forth between each other, and if a node doesn’t respond for some period of time—say, 30 seconds—it is assumed to be dead.


