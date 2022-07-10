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

### Introduction to Indexes
- What is the problem Indexes try to solve?
    - Slow queries as instead of looking to all documents one by one [Collection Scan] O(n) we got the documentID from the index directly like the concept of dictionry index.
- **Collection Scan**: is going through all docs in the collection 1 by 1   O(n) linear
- The data structure used to store indexes **B-tree**
- The index is associate with one or more fields.
- The _id field is automaticlly indexed on all collections
- **Index Overhead**:
    - slow writes ---> the tree will be updated
    - slow updates and deletes ---> the tree will be adjusted if the indexed field changes
    - We don't have too many unneeded indexes which will affect the performance of insert, delete and update operations.
> You can learn more about indexes by visiting the [Indexes Section](https://www.mongodb.com/docs/manual/indexes/?jmp=university) of the MongoDB Manual.



### How Data is Stored on Disk
- The MongoDB use to store data will differ between different storage engines that mongodb support(MMAPv1 - Wired Tiger - Other).
- Each Collection has a ```file.wt```
- Each Index has a ```file.wt```

- **Database Catalog**: ```_mdb_catalog.wt```
	- Database Catalog contains info about collections and indexes that this mongod has.


- **Running mongod with these flags**:
    - ```--directoryperdb``` ---> each db has its own directory
    - ```--wiredTigerDirectoryForIndexes``` ---> directory for indexes and directory for collections inside the db folder
    - **benefits of the previous structure**:
	    - increasing performance: if multiple disks are available, then distributing these files across different disks (using symbolic links) will increase IO parrallization.
        - We can make a disk for collections and disk for indexes.

- mongodb can store data in compressed format which will increase the performance of data presistence but will require more cpu cycles

- writing data from memory to disk will be triggered by two methods:
	- user side: by specifying write concern that syncs operation with other instances like { writeConcern: { w: 3 } } which means at least one primary and 2 secondaries
	- periodical internal process that regulates how data will be flushed and synced (sync periods)

- **Journaling**:
	- journal flushes are performed using group commits in compressed format.
	- all writes are atomic
	- { writeConcern: { j: true } } wont acknowledge a write unless it has been written in the journal
	- will have impact on performance as it will wait until data is written on disk

> You can learn more about how data is stored on disk in MongoDB by visiting the [MongoDB Storage FAQ](https://www.mongodb.com/docs/manual/faq/storage/?jmp=university) in the MongoDB Manual.


### Single Field Indexes
- the simplest index that can be created
- ```db.<collection>.createIndex({ <field>: <direction> })```

- **Key Features**: 
	- Key from only one field
	- Can find a single value for the indexed field
	- Can find a range of values
	- Can use dot notation to index fileds in subdocuments
	- Can be used to finding several distict values in a single query

- ```db.people.find({"ssn": "720-38-5636"}).explain("executionStats")``` ---> to view extra info about the query
    ```
    queryPlanner: {
        namespace: 'myFirstDatabase.people',
        indexFilterSet: false,
        parsedQuery: { ssn: { '$eq': '720-38-5636' } },
        maxIndexedOrSolutionsReached: false,
        maxIndexedAndSolutionsReached: false,
        maxScansToExplodeReached: false,
        winningPlan: {
            stage: 'COLLSCAN',<--------
            filter: { ssn: { '$eq': '720-38-5636' } },
            direction: 'forward'
        },
        rejectedPlans: []
    },
    executionStats: {
        executionSuccess: true,
        nReturned: 1, <----------------|
        executionTimeMillis: 81,       |
        totalKeysExamined: 0,          |
        totalDocsExamined: 50474,<-----|
        executionStages: {
            stage: 'COLLSCAN', <-------
            filter: { ssn: { '$eq': '720-38-5636' } },
            nReturned: 1,
            executionTimeMillisEstimate: 27,
            works: 50476,
            advanced: 1,
            needTime: 50474,
            needYield: 0,
            saveState: 50,
            restoreState: 50,
            isEOF: 1,
            direction: 'forward',
            docsExamined: 50474 <----------
        }
    }
    ```
- We will notice that the winning plan is COLLSCAN that means in order to returned 1 document it made full collection scan for 50474 documents which is not efficient
- Let's create an index on ssn
- ```db.people.createIndex({ ssn: 1 })``` to create index on ssn field
- 1 means that the index is ordered in ASC

- explainable object:
	- ```let exp = db.people.explain("executionStats")```
	- then we can run our queries on the exp object
	- ```exp.find({"ssn": "720-38-5636"})```
-   ```
    queryPlanner: {
        namespace: 'myFirstDatabase.people',
        indexFilterSet: false,
        parsedQuery: { ssn: { '$eq': '720-38-5636' } },
        maxIndexedOrSolutionsReached: false,
        maxIndexedAndSolutionsReached: false,
        maxScansToExplodeReached: false,
        winningPlan: {
            stage: 'FETCH',<-----
            inputStage: {
                stage: 'IXSCAN',<----
                keyPattern: { ssn: 1 },
                indexName: 'ssn_1',
                isMultiKey: false,
                multiKeyPaths: { ssn: [] },
                isUnique: false,
                isSparse: false,
                isPartial: false,
                indexVersion: 2,
                direction: 'forward',
                indexBounds: { ssn: [ '["720-38-5636", "720-38-5636"]' ] }
            }
        },
        rejectedPlans: []
    },
    executionStats: {
        executionSuccess: true,
        nReturned: 1,<-------------------|
        executionTimeMillis: 1,          |
        totalKeysExamined: 1,<-----------|--- the number of used indexed keys
        totalDocsExamined: 1,<-----------|
        executionStages: {
            stage: 'FETCH',
            nReturned: 1,
            executionTimeMillisEstimate: 0,
            works: 2,
            advanced: 1,
            needTime: 0,
            needYield: 0,
            saveState: 0,
            restoreState: 0,
            isEOF: 1,
            docsExamined: 1,
            alreadyHasObj: 0,
            inputStage: {
                stage: 'IXSCAN',
                nReturned: 1,
                executionTimeMillisEstimate: 0,
                works: 2,
                advanced: 1,
                needTime: 0,
                needYield: 0,
                saveState: 0,
                restoreState: 0,
                isEOF: 1,
                keyPattern: { ssn: 1 },
                indexName: 'ssn_1',
                isMultiKey: false,
                multiKeyPaths: { ssn: [] },
                isUnique: false,
                isSparse: false,
                isPartial: false,
                indexVersion: 2,
                direction: 'forward',
                indexBounds: { ssn: [ '["720-38-5636", "720-38-5636"]' ] },
                keysExamined: 1,
                seeks: 1,
                dupsTested: 0,
                dupsDropped: 0
            }
        }
    },
    ```
- **Note**:
    > If more than one fields are queried and one of them is indexed the indexed results will be returned then filtered by the other fields

    - ``` db.examples.insertOne({_id: 0, subdoc: {indexedField: "value", otherField: "value"}})```
    - ``` db.examples.insertOne({_id: 1, subdoc: {indexedField: "wrongValue", otherField: "value"}})```
    - ```db.examples.createIndex({"subdoc.indexedField": 1})```
    - ```db.examples.explain("executionStats").find({"subdoc.indexedField": "value"})```
    - ```
        queryPlanner: {
            namespace: 'myFirstDatabase.examples',
            indexFilterSet: false,
            parsedQuery: { 'subdoc.indexedField': { '$eq': 'value' } },
            maxIndexedOrSolutionsReached: false,
            maxIndexedAndSolutionsReached: false,
            maxScansToExplodeReached: false,
            winningPlan: {
                stage: 'FETCH',
                inputStage: {
                    stage: 'IXSCAN',<-------
                    keyPattern: { 'subdoc.indexedField': 1 },
                    indexName: 'subdoc.indexedField_1',
                    isMultiKey: false,
                    multiKeyPaths: { 'subdoc.indexedField': [] },
                    isUnique: false,
                    isSparse: false,
                    isPartial: false,
                    indexVersion: 2,
                    direction: 'forward',
                    indexBounds: { 'subdoc.indexedField': [ '["value", "value"]' ] }
                }
            },
            rejectedPlans: []
        },
        executionStats: {
            executionSuccess: true,
            nReturned: 1,
            executionTimeMillis: 0,
            totalKeysExamined: 1,
            totalDocsExamined: 1,
            executionStages: {
                stage: 'FETCH',
                nReturned: 1,
                executionTimeMillisEstimate: 0,
                works: 2,
                advanced: 1,
                needTime: 0,
                needYield: 0,
                saveState: 0,
                restoreState: 0,
                isEOF: 1,
                docsExamined: 1,
                alreadyHasObj: 0,
                inputStage: {
                    stage: 'IXSCAN',
                    nReturned: 1,
                    executionTimeMillisEstimate: 0,
                    works: 2,
                    advanced: 1,
                    needTime: 0,
                    needYield: 0,
                    saveState: 0,
                    restoreState: 0,
                    isEOF: 1,
                    keyPattern: { 'subdoc.indexedField': 1 },
                    indexName: 'subdoc.indexedField_1',
                    isMultiKey: false,
                    multiKeyPaths: { 'subdoc.indexedField': [] },
                    isUnique: false,
                    isSparse: false,
                    isPartial: false,
                    indexVersion: 2,
                    direction: 'forward',
                    indexBounds: { 'subdoc.indexedField': [ '["value", "value"]' ] },
                    keysExamined: 1,
                    seeks: 1,
                    dupsTested: 0,
                    dupsDropped: 0
                }
            }
        }```

- We can use Single Field Index to query on range of values
    - ```exp.find({ssn: {$gte: "555-00-0000", $lt: "556-00-0000"}})```
    -   ```
        queryPlanner: {
            namespace: 'myFirstDatabase.people',
            indexFilterSet: false,
            parsedQuery: {
            '$and': [
                { ssn: { '$lt': '556-00-0000' } },
                { ssn: { '$gte': '555-00-0000' } }
            ]
            },
            maxIndexedOrSolutionsReached: false,
            maxIndexedAndSolutionsReached: false,
            maxScansToExplodeReached: false,
            winningPlan: {
            stage: 'FETCH',
            inputStage: {
                stage: 'IXSCAN',
                keyPattern: { ssn: 1 },
                indexName: 'ssn_1',
                isMultiKey: false,
                multiKeyPaths: { ssn: [] },
                isUnique: false,
                isSparse: false,
                isPartial: false,
                indexVersion: 2,
                direction: 'forward',
                indexBounds: { ssn: [ '["555-00-0000", "556-00-0000")' ] }
            }
            },
            rejectedPlans: []
        },
        executionStats: {
            executionSuccess: true,
            nReturned: 49,<-----------
            executionTimeMillis: 8,
            totalKeysExamined: 49,
            totalDocsExamined: 49,<------------
            executionStages: {
                stage: 'FETCH',
                nReturned: 49,
                executionTimeMillisEstimate: 4,
                works: 50,
                advanced: 49,
                needTime: 0,
                needYield: 0,
                saveState: 0,
                restoreState: 0,
                isEOF: 1,
                docsExamined: 49,
                alreadyHasObj: 0,
                inputStage: {
                    stage: 'IXSCAN',
                    nReturned: 49,
                    executionTimeMillisEstimate: 0,
                    works: 50,
                    advanced: 49,
                    needTime: 0,
                    needYield: 0,
                    saveState: 0,
                    restoreState: 0,
                    isEOF: 1,
                    keyPattern: { ssn: 1 },
                    indexName: 'ssn_1',
                    isMultiKey: false,
                    multiKeyPaths: { ssn: [] },
                    isUnique: false,
                    isSparse: false,
                    isPartial: false,
                    indexVersion: 2,
                    direction: 'forward',
                    indexBounds: { ssn: [ '["555-00-0000", "556-00-0000")' ] },
                    keysExamined: 49,
                    seeks: 1,
                    dupsTested: 0,
                    dupsDropped: 0
                }
            }
        },
        ```

    > You can learn more about single field indexes by visiting the [Single Field Indexes](https://www.mongodb.com/docs/manual/core/index-single/?jmp=university) Section of the MongoDB Manual.

- We can use a single field index to query in a set of single values
    - ```exp.find({ssn: {$in: ['001-29-9184', '177-45-0950', '265-67-9973']}})```
    - ```
        queryPlanner: {
            namespace: 'myFirstDatabase.people',
            indexFilterSet: false,
            parsedQuery: { ssn: { '$in': [ '001-29-9184', '177-45-0950', '265-67-9973' ] } },
            maxIndexedOrSolutionsReached: false,
            maxIndexedAndSolutionsReached: false,
            maxScansToExplodeReached: false,
            winningPlan: {
            stage: 'FETCH',
            inputStage: {
                stage: 'IXSCAN',
                keyPattern: { ssn: 1 },
                indexName: 'ssn_1',
                isMultiKey: false,
                multiKeyPaths: { ssn: [] },
                isUnique: false,
                isSparse: false,
                isPartial: false,
                indexVersion: 2,
                direction: 'forward',
                indexBounds: {
                ssn: [
                    '["001-29-9184", "001-29-9184"]',
                    '["177-45-0950", "177-45-0950"]',
                    '["265-67-9973", "265-67-9973"]'
                ]
                }
            }
            },
            rejectedPlans: []
        },
        executionStats: {
            executionSuccess: true,
            nReturned: 3,
            executionTimeMillis: 0,
            totalKeysExamined: 6,
            totalDocsExamined: 3,
            executionStages: {
                stage: 'FETCH',
                nReturned: 3,
                executionTimeMillisEstimate: 0,
                works: 6,
                advanced: 3,
                needTime: 2,
                needYield: 0,
                saveState: 0,
                restoreState: 0,
                isEOF: 1,
                docsExamined: 3,
                alreadyHasObj: 0,
                inputStage: {
                    stage: 'IXSCAN',
                    nReturned: 3,
                    executionTimeMillisEstimate: 0,
                    works: 6,
                    advanced: 3,
                    needTime: 2,
                    needYield: 0,
                    saveState: 0,
                    restoreState: 0,
                    isEOF: 1,
                    keyPattern: { ssn: 1 },
                    indexName: 'ssn_1',
                    isMultiKey: false,
                    multiKeyPaths: { ssn: [] },
                    isUnique: false,
                    isSparse: false,
                    isPartial: false,
                    indexVersion: 2,
                    direction: 'forward',
                    indexBounds: {
                    ssn: [
                        '["001-29-9184", "001-29-9184"]',
                        '["177-45-0950", "177-45-0950"]',
                        '["265-67-9973", "265-67-9973"]'
                    ]
                    },
                    keysExamined: 6,
                    seeks: 3,
                    dupsTested: 0,
                    dupsDropped: 0
                }
            }
        },
      ```


### Understanding Explain
- used to analyse the query

- info provided by explain:
	- indexes used
	- indexes used to provide sort
	- indexes used to provide projections
	- how selective is the index
	- which part of the plan is the most expensive

- to create explainable object: 
	- exp = db.people.explain()
	- exp.find()
	- parameters to pass to explain func:
		- "queryPlanner" --default (won't run the query) ***
		- "executionStats"  (runs and returns specific stats about the execution)
		- "allPlansExecution"  (runs then gives all alternative plans)

- in case of sharded cluster:
	- each shard will use a winning plan
	- then the plans are merged on the mongos

- **Note**
    > the memory limit for execute in memory sort is 33554432 byte around 32MB and if your query exceed this limit then the sort will canceled



### Sorting with Indexes
- There are two ways to execute sorting:
	1. In memory
	2. Using index

- **In memory**:
    - all documents in collections are stored on disk in unknown order
    - the server will return the documents in the order it find them
	- all data will be stored in RAM
	- then sorting algorithm is run
	- max memory usage in sorting is 32MB if exceeded then the server will abort the operation

- **Using indexes**: 
	- docs are ordered while saved in index using the index field
	- the docs returned will be sorted by the index then no need for additional sorting
	- direction will be forward if the sorting order is the same as the index and backward if not (asc - desc)

> You can learn more about sorting with indexes by visiting the [Use Indexes to Sort Query Results](https://www.mongodb.com/docs/manual/tutorial/sort-results-with-indexes/?jmp=university) section of the MongoDB Manual.





### Querying on Compound Indexes
- Compound index is an index on two or more fields
- Even Index is conmpound of more than one field it still on dimentional.
- We can use compound index to query a range of values too.
- The main key is the first key in the index then the subsequent keys are indexing within that main index and so on
	- index {name: 1, age: 1}
	- {name: "a", age: "1"}, {name: "a", age: "2"}
	- {name: "b", age: "3"}, {name: "b", age: "8"}

- **Index Prefixes**:
    - Index prefixes are the beginning subsets of indexed fields.
	- For example consider the following compound index ```{"item": 1, "location": 1, "stock": 1}```
	- The index has the followning index prefixes:
		- ```{item: 1}```
		- ```{item: 1, location: 1}```
	- prefexis will use the index although it doesn't utilize the whole index the remaining will be ignored

> You can learn more about compound indexes by visiting the [Compound Indexes](https://www.mongodb.com/docs/manual/core/index-compound/?jmp=university) and [Create Indexes to Support Your Queries](https://www.mongodb.com/docs/manual/tutorial/create-indexes-to-support-queries/?jmp=university) sections of the MongoDB Manual.



### When you can sort with compound Indexes ?
- Sorting can utilize index if the filter fields combined with sort fields gives index prefix
    - ```find({a: "", b: ""}).sort({c: 1})```  will work for index ```{a: 1, b: 1, c: 1}```
- Using single prefix ---> both directions will utilize the index
- else, won't use index unless all is in the same direction as the index or all are reversed


> You can learn more about when you can sort with indexes by visiting the [Use Indexes to Sort Query Results](https://www.mongodb.com/docs/manual/tutorial/sort-results-with-indexes/?jmp=university) section of the MongoDB Manual.




### Multikey Indexes
- Indexing on a field that is an array is a Multikey Index

- for each entry in the array, the server will create a separate index key.
- it can also work in emedded docs inside the array
	- arr: [{a: "", b: ""}, ...] ---> creatIndex({arr.a: 1})
- For each indexed document, we can have at most one index field whose value is an array
- only one array index field is allowed per compound index
- Multikey indexes don't support covered queries

> You can learn more about multikey indexes by visiting the [Multikey Indexes](https://www.mongodb.com/docs/manual/core/index-multikey/?jmp=university) section of the MongoDB Manual.


### Partial Indexes
- Index only a subset of the collection

- ```
    db.collection.creatIndex(
	    {a: 1, b: 1},
	    {partialFilterExpression: {c: {$gte: 5}}}
    )
    ```
	the above will index docs with c >= 5

- Can be used with multi-key indexes

- **Sparse index**: Is a spectial case of partial index (only index if the fields exist)
	- ```
        db.collection.creatIndex(
		    {a: 1},
		    {sparse: true}
	    )
        ```    
	- the above is equal to
        ```
        db.collection.creatIndex(
            {a: 1},
            {partialFilterExpression: {a: {$exists: true}}}
        )
        ```

- In order to use a partial index, the filter query must be guarateed to match a subset of the documents, specified by the partialFilterExpression 

- **Partial Index Restrections**:
    - You can't specify both the partialFilterExpression and the sparse options
    - _id indexes can't be partial indexed
    - Shard key index can't be partial index


> You can learn more about partial indexes by visiting the [Partial Indexes](https://www.mongodb.com/docs/manual/core/index-partial/?jmp=university) section of the MongoDB Manual.




### Text Indexes
- Querying docs based on words in a certain text field

- ```createIndex({a: "text"})```
- ```db.collection.find({ $text: { $search: "..." } })```
- similar to multi-key indexes will split the text and create index key for each unique word in the string
- Text indexes are case insensitive
- Will be effected if the text is long
    - More keys to examine
    - Increased index size
    - Increased time to build index
    - Decreased write  performance
- One solution for long text index is to use compound index
- text querys uses or between the delimited words
	- "a b" will match a or b

- **textScore**:
	- is the relevance of the result to the text query
	- ```find({}, {score: {$meta: "textScore"}})```