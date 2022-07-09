# Chapter 1: Introduction
---
## Hardware Considerations & Configurations
MongoDB is a High Performance Database and to support your requirements it will require good hardware.
### Let's discuss how mongodb uses hardware resources:
#### 1. Memory
- For execution of the operations
- very important
- Database is designed arround the usage of memory
- mongodb engines are either very dependant on ram or has fully in-memory execution for its data management operations.
- A signficant number of operations are rely heavily in RAM like:
    - Aggregations 
    - Index Traversing 
    - Write Operations (are first performed on ram allocated pages) 
    - Query Engine 
    - connections (1MB per connection)
- We can say the more RAM you have, the more performant you get from your mongodb

#### 2. CPU
- Used by all aplications for computational processing.
- MongoDB using CPU with 2 main factors:    
    - **Storage engine**
    - **Concurrency Model** (wired tiger has non locking concurrency control mechanism)
- Mongodb try to use all available CPU cores
- There is others operations that will require availability of CPU cores:
    - page compression
    - data calculation
    - aggergation framework operations
    - map reduce
- Don't forget that not all write or read operations are non-locking operations, for example:
    - Writing or updating to the same document will require each write to block other writes on that same document to comply.
    - In situations like this multible CPUs do not help performance because the Threads cann't do their work in parallel
#### 3. I/O
- For presistance and communications between servers or withen the host services
- Data presistence on disk 
- The higher IOPS (Input/Output Operations per Second)your disk provide, the faster the db operations
- The type of disks will realy affect the overall performance of your MongoDB
- We can used RAID architectures for redundancy of read and write operations
- RAID 10 is the best for mongodb
- RAID 0, 5 or 6 musn't be used
- using several disks benifits mongodb by distributing the IO loads
- the faster the network, the better performance with client apps
- distributed architecture in mongodb means it needs low latency connection between different components to enhance performance
- The type of newtork switches, Load balancers and firewall will affect the latency which need to be taken into consideration when analizing the network architecture of your application.






# Chapter 2: MongoDB Indexes
---
