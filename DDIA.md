# Designing Data-Intensive Application

## Foundations of Data Systems

### Reliability

- Ensuring systems continue working correctly despite failures (hardware, software, or human errors).
- Faults are inevitable
- Fault-tolerance
  - Hardware faults
    - Hard disk crash, RAM corruption, CPU overheating
  - Software errors
    - Bugs, memory
  - Human errors
    - Configuration errors, incorrect deployments, user errors

### Scalability

- Handling increased load effectively (throughput, response time, and partitioning strategies).
  - Load distribution
  - requests/second to a website
  - read/write ratio in a database
  - hit rate on a cache
- Twitter example: a uer follows many people, and each user is followed by many people
  - Fan-out: when a user posts a tweet, it is delivered to all of their followers
    - posting a tweet simply inserts new tweet to  global collecction of tweets. when a user requests their home timeline (FYP), look up all people user follows, find all tweets for each of followed person, merge and sort by time
    - this is a read-heavy workload
    - maintain a cache for each uer's FYP, like a mailbox of tweets for each recipient user. When user posts a tweet, look up all the people who follow that user and insert the new tweet into each of their FYP caches.
      - this is a write-heavy workload
      - uses a lot of memory
- scaling approaches
  - vertical scaling
    - moving to a more powerful machine
    - more expensive
    - limited
    - easier to manage
  - horizontal scaling
    - distributing load across multiple smaller machines
    - more cost-effective
    - more fault-tolerant
    - more scalable
    - more complex
    - more difficult to manage

### Maintainability

- Designing systems that are operable, simple, and evolvable.
- Easy to understand
- **operability**
  - make it easy for operations teams to keep the system running smoothly
- **simplicity**
  - make it easy for new engineers to understand the system
- **evolvability**
  - make it easy for engineers to make changes to the system in the future

## Data Models and Query Languages

- Relational vs. NoSQL databases
  - Examines structured (SQL) and flexible (document, key-value, columnar, and graph) models.
- Query paradigms
  - Declarative queries (SQL), MapReduce, and graph query languages like Cypher and SPARQL.
- can be expressed in a variety of ways
  - JSON, XML, CSV, binary, etc.
  - relational DB
    - data organized in relations, tables
    - each relation is unordered collection of tuples, rows
  - graph DB
    - awkward translation between OOD and relational DB
      - object-relational mapping (ORM) reduces boilerplate code
  - NoSQL adotption
    - need for greater scalabilityt
    - free / open source software
    - special queries not supported by relational DB
    - schema flexibility

### Relational Model

- One-to-many and many-to-many relationships
  - one-to-many
    - a user has many tweets
    - a tweet has one user
  - many-to-many
    - a user follows many other users
    - a user is followed by many other users
  - makes it easier to search
    - LinkedIn user search in AREA A with SKILL B

### Document Model

- mostly one-to-many relationships
  - a user has many tweets
  - a tweet has one user
- no relationships between records

### MapReduce Query

- programming model for processing large amounts of data in bulk across many machines
  - used by Google, ultimately replaced with Flume
- usage examples
  - counting the number of occurrences of each word in a large collection of documents
  - distributed grep
  - distributed sort

### Graph Query

- used for querying graph-structured data
  - social networks, road networks, network topologies
    - social networks, mutual friends
    - road networks, shortest path
    - network topologies, network reachability

### Cypher Query

- declarative query language for graph databases
  - used by Neo4j

## Storage and Retrieval

### B-Trees (balanced trees)

- typically used in databases
- data is stored in sorted pages
- fast read, expensive write
- updates in place
- four level tree of 4KB with branching factor of 500 can store ~256TB

### LSM-Trees (log structured merge trees)

- great write heavy apps, e.g. logging system, event streaming
- slow reads, but fast writes and are stored in mem buff (MemTable sorted order)
- flushed to disk when full as immutable SSTable

### Column-oriented storage

- Optimized for analytics and compression.

### Data warehouse schemas

- Star and snowflake schemas for analytical workloads.

### Deleting records

- Tombstones and compaction strategies for removing data.
  - deleted keys are not included when log segments are merged

### Crash recovery

- when crash happens, servers may lose data.
- restarting the server takes time
- it's easier to write data to a disk than to read it after a crash

### Concurrency control

- kwrites are appended to log sequentially
- easier to have one writer thread
- enables parallel reads

## Encoding and Evolution

### Data serialization formats

- **JSON** is human-readable, but inefficient
  - distuingish between 1 and "1" but not between 1 and 1.0
  - no support for binary data
- **CSV** is simple, but lacks schema
  - can't tell between 1 and "1"
- **XML** is verbose and complex
  - can be hard to read
  - can't tell between 1 and "1"
- **Protocol Buffers** is binary, efficient, and has schema evolution support
  - field tags are used to identify fields
  - legacy fields can be marked as deprecated, tags are kept
  - unrecognized fields are ignored
  - fields can be added, removed, or renamed

```protobuf
<!-- next tag: 4 -->
message Person {
required string user_name = 1;
optional int64 favorite_number = 2;
repeated string interests = 3;
}
```

### Schema evolution
- Managing changes in data structure over time.

### Modes of dataflow

#### Databases

  - write -> encode
  - read -> decode
  - UPDATE: read and decode into model object (DB->class object) then encode and write back to DB

#### Service Calls

- client/server communication
  - servers expose API over network
  - clients can connect to the servers to make requests to that API
  - API is called a service
  - one service can call another service, SOA (service-oriented architecture)
- Web services
  - REST
    - uses URLs for identifying resources and using HTTP feats for  cache control, authentication, and content negotiation
    - uses JSON for encoding messages
  - SOAP
    - uses XML for encoding messages
- RPC (remote procedure call) flaws
  - just like calling a function in another process
  - local function call is predictable, either succeeds or fails, but a network request is unpredictable because it might not get a response at all
  - servers and clients can change/deploy independently
  - uses futures (promises) to encapsulate async actions
  - gRPC
    - supports streams, call consists of not just one request and one response, but series of each
    - uses protocol buffers for encoding messages
    - uses HTTP/2 for transport

#### Message-Passing Dataflow

- client's request (RPC-like), called message, is delivered to another process with low latency
- message not set via direct network connection (database-like), goes via message broker/queue, storing message temporarily
- message broker pros
  - acts as buffer if recipient is unavailable, improes system reliability
  - automatically redelivers messages if recipient crashes
  - doesn't need IP address / port number of recipient
- one-way messaging, sender doesn't expect a response
- distributed actor frameworks is for concurrency in a single process

## Distributed Data Systems

### Replication strategies

- high availability
  - keeps system running, even when one machine goes down
- disconnected operatin
  - allows application to continue working when there is network interruption
- latency
  - placing data geographically close to users
- scalability
  - handles higher volume of reads than single machine could handle

#### THREE MAIN APPROACHES TO REPLICATION

- single-leader replication
  - cliends send ALL writes to a single node (leader) which sends a stream of data change events to other replicas (followers). Reads can perform on any replica but reads from followers might be stale
- multi-leader replication
  - cliends send each write to one of the leaders. The leaders send s treams of data change events to each other and to any followers
- leaderless replication
  - cliends send each write to seceral nodes, and read from several nodes in parallel in order to detect and correct nodes with stale data

### Partitioning

- Hash-based and range-based partitioning, rebalancing techniques.

### Transactions

- ACID properties, isolation levels, and distributed transactions (2PC, serializability).

## Challenges in Distributed Systems

- Fault tolerance and consensus
  - CAP theorem, consistency models, and distributed consensus protocols (Raft, Paxos).
- Clock synchronization
  - Dealing with unreliable clocks and time-based ordering issues.

## Derived Data Processing

- Batch processing
  - Hadoop, MapReduce, and Spark for large-scale processing.
- Stream processing
  - Kafka, event sourcing, and real-time data integration.
- Lambda vs. Kappa architectures
  - Hybrid vs. stream-only data processing models.

## Future Trends

- Dataflow-driven architectures
  - Moving away from monolithic databases towards specialized data pipelines.
- Privacy, security, and ethics
  - Balancing user tracking, predictive analytics, and responsible data practices.
