# Possible Questions

## Difference between array and linked list

- Array is a collection of elements of the same type stored in contiguous memory locations. It is fixed in size. Accessing elements is fast at O(1) but inserting and deleting elements is slow at O(n).
- Linked list is a collection of elements of the same type stored in non-contiguous memory locations. It is dynamic in size. Accessing elements is slow at O(n) but inserting and deleting elements is fast at O(1).

## What is a hash table and how does it work?

- it's a data structure that maps keys to values. It uses a hash function to compute an index into an array of buckets or slots. It is fast at O(1) for inserting, deleting, and searching. For string, the hashing is done by getting the length of the string and modulo it by the size of the hash table. We can use separate chaining if there are collisions, i.e. treat the index is a linked list (this turns O(1) into O(n)).

## ## Process vs Thread

- Process is an instance of a program that is being executed. It has its own memory space and resources. It is heavyweight.
- Thread is a lightweight process. It shares the same memory space and resources as the process. It is used for small tasks.

## Binary Search Tree vs Heap

- BST is when left node < parent node and right node > parent node. It is good for searching and sorting. It has O(log n) for searching, inserting, and deleting.
- Heap is when parent node > child node. It is good for priority queues. It has O(log n) for inserting and deleting but O(1) for searching.

## BFS vs DFS

- BFS uses a queue to explore level by level, which is good for finding shortest path.
- DFS uses a stack to explore as far as possible along each branch before backtracking. It is good for cycle detection.
- Both have O(V+E) time complexity, where V is the number of vertices and E is the number of edges.

## Quick Sort vs Merge Sort

- Both sorting algorithms are divide and conquere algorithms but QS picks a pivot and partitions the array around the pivot while MS divides the array into multiple partitions, sorts, and then merges said partitions.

## DP with example

- it's a way of solving problems by breaking them down into subproblems and storing results to avoid recomputation, basically a NoRecursion so to speak if compared to SQL vs NoSQL. For example, Fibonacci sequence is what we implement our first recursion on, but this can be solved using DP by storing the results of the subproblems.

```cpp
int fib(int n){
    std::vector<int> dp(n+1, 0);
    dp[0] = 0;
    dp[1] = 1;
    for(int i = 2; i <= n; i++){
        dp[i] = dp[i-1] + dp[i-2];
    }
    return dp[n];
}
```

## SOLID principles

- SOLID is a set of principles that help us write better code.
- Single Responsibility Principle: a class should have only one reason to change
- Open/Closed Principle: a class should be open for extension but closed for modification
- Liskov Substitution Principle: objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program
- Interface Segregation Principle: a client should not be forced to implement an interface that it doesn't use
- Dependency Inversion Principle: high-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should depend on abstractions.

## Monolith vs Microservices

- Monolith is a single unit that contains all the code for the application. It is easy to develop and deploy but hard to scale and maintain.
- Microservices is a collection of small services that are loosely coupled. It is easy to scale and maintain but hard to develop and deploy.

## DB Sharding

- a way to split a DB into smaller parts based on a key
- it's great way to implement when a DB instance can't handle the load or when we want to reduce contention on a single table

## What are the trade-offs between vertical scaling and horizontal scaling?

- The trade offs are mainly about cost, efficiency, and fault tolerance.
- For vertical scaling,
  - it might be easier to upgrade resources like CPU and RAM on a single machine, but it is costly
  - it is not fault tolerant, if the machine fails, the whole system fails
- For horizontal scaling,
  - it is cheaper to add more machines, but it is more complex to manage
  - it is fault tolerant, if one machine fails, the system can still run

## How would you design a reliable and fault-tolerant system?

- I would keep backups by creating replicas of the data. I would also distribute data across nodes to prevent hot spots. And amongst many others, I would add load balancing algorithms to distribute the load evenly across these nodes.

## What are common causes of failures in distributed systems?

- hardware crashes
- concurrency issues like race conditions
- network failures

## Websockets

- it's persistent bi-directional communication between client and server, used for chat apps, games, maybe stock prices

## CDN (content delivery network)

- caches static content like imgs and videos in servers close to users. I learned about this through using Discord. I downloaded an image and the URL said CDN.discordapp.com
- it reduces latency and server load

## When would you choose a document database over a relational database?

- I would choose document database when tables don't need complicated joins and when the data is hierarchical in nature. Also when read performance is more important than write performance.
- I would choose relational database when the data is structured and when the data needs to be normalized. Also when the data needs to be queried in many different ways, which is good for analytics.

## Explain the advantages of graph databases and give an example use case

- graph DBs are good for storing relationships between data. For example, it's good for social networks where users are connected with each other or to see mutual connections. A good one is LinkedIn. It has 1st, 2nd, and 3rd connections.

## What’s the difference between B-Trees and LSM-Trees? When would you use each?

- B-Trees are fast read, slow write. They are good for databases that need to be updated frequently.
  - would be best used in parts of a social media system where data is read more often than written because users are constantly reading posts
- LSM-Trees are slow read, fast write. They are good for databases that need to be updated infrequently.
  - would be best used in parts of a social media system where data is written more often than read because users are constantly posting new content

## Explain the CAP theorem and how it applies to real-world databases

- it is this theory that only 2/3 can be achieved in a distributed system: Consistency, Availability, and Partition Tolerance. || Consistency means every read receives the most recent write or an error. Availability means every request receives a response, without guarantee that it contains the most recent write. Partition Tolerance means the system continues to operate despite network failures.
- Since most systems need to be partition tolerant, the trade off is between consistency and availability. Most systems choose availability over consistency. But I would argue that availability and consistency can coexist but in different parts of the system. For instance, a ticketing system can be consistent when checking for ticket availability but available when purchasing tickets all while being partition tolerant.
- CP for traditional DB like SQL and AP for NoSQL

## How would you design a distributed cache for a high-traffic website?

- I would include distributed cache so that repeating searches don't have to hit the database. I would use Redis because it is fast and can be used as a cache. I would also use a load balancer to distribute the load evenly across the nodes. I would probably also include some eviction policies to remove old data, mainly to manage the cache size.

## Explain the differences between REST APIs and gRPC

- REST APIs are great for general web services and gRPC is great for microservices that require low-latency. REST APIs use JSON making it slower because it's text-parsing while gRPC is faster because it uses binary serialization.
- REST uses HTTP methods while gRPC uses protobufs

## What is an event-driven architecture, and when would you use it?

- it is a system that responds to events. It is good for systems that need to be scalable and responsive. For example, a chat system where messages are sent and received in real-time. Another example are online games where players need to be updated in real-time.

## Waterfall vs Agile

- Waterfall is a linear approach and sticks to the plan. It is good for small projects with clear requirements. It is not good for large projects because requirements can change.
- Agile is an iterative approach and is good for large projects with changing requirements. It is not good for small projects because it can be too complex.
