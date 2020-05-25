# Data Model for Big Data: Chapter 3

## 1. Why data corruption issues are hard to debug?

Because you have very little context on how the corruption occurred. Typically you’ll only notice there’s a problem when there’s an error downstream in the processing—long after the corrupt data was written. 

## 2. What do serialization frameworks do?
Serialization frameworks generate code for whatever languages you wish to use for reading, writing, and validating objects that match your schema. 

## 3. What does a property contain?
 A property contains a node and a value for the property.The value can be one of many types, 
 
## 4. Why only checking for the present required fields and their expected type is not a limitation for serialization frameworks?
The difficulties of representing nested objects and doing schema migrations with relational databases are non-existent when applying serialization frameworks to represent immutable objects using graph schemas. 

## 5. What is the idea of a schema?
The right way to think about a schema is as a function that takes in a piece of data and returns whether it’s valid or not. 

# Data Storage on the Batch Layer: Chapter 4

## 1. What is the batch layer responsible for?
The batch layer is also responsible for computing functions on the dataset to produce the batch views. This means the batch layer storage system needs to be good at reading lots of data at once. In particular, random access to individual pieces of data is not required. 

## 2. What is the biggest problem with key/value stores?
it has a lot of things you don’t need: random reads, random writes, and all the machinery behind making those work. In fact, most of the implementation of a key/value store is dedicated to these features you don’t need at all.

## 3. What is the advanage of distributed filesystems over regular file sistems?
 Distributed filesystems are designed so that you have fault tolerance when a machine goes down, meaning that if you lose one machine, all your files and data will still be accessible
 
 ## 4. What is the process called vertical partioning?
 The batch storage should allow you to partition your data so that a function only accesses data relevant to its computation, without reading the entire dataset, vertical partitioning enables large performance gains 
 
 ## 5. How distributed filesystems meet the storage requirements ?
 Efficient appends of new data
Scalable storage
Support for parallel processing
Tunable storage and processing costs
Enforceableimmutability
