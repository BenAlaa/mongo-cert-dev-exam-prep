## Chapter 1: Introduction
---
### Hardware Considerations & Configurations
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






## Chapter 2: MongoDB Indexes
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



### Collations

- Collations allows users to specify language specific rules for string comparison
    ```
    {
        locale: string,   the ICU supported locale
        caseLevel: boolean,
        caseFirst: string,
        strength: int,
        numericOrder: boolean,
        alternate: string,
        maxVariable: string,
        backwards: boolean
    }
    ```

- Where to specify collation:
	- In collection level ```db.createCollection("name", {collation: {...}})``` 
    - For a specific requests like queries and aggregations:
        - ```db.find().collation({...})``` as a iterator method
        - ```db.aggregate([{}, {collation: {...}}])``` as a stage 

	- For Indexes ```db.createIndex({}, {collation: {...}})```
        - To using this index for quering the collation specified with the index must match the one specified with this key on collection level

- **Benifits of using Collations**:
	- correctness by matching certain locale
	- it has a marginal performance impact
	- allows the use of case-insensitive indexes by using strength: 1 which will ignore case and diacritics 


> You can learn more about collations by visiting the [Collations](https://www.mongodb.com/docs/manual/reference/collation/?jmp=university) section of the MongoDB Manual.





### Wildcard Index Type
- New in 4.2
- Allows a dynamically create indexes on all fields or subset of fields

- how it works:
	- mongodb analysis the document and indexes all fields in the document
	- for arrays, indexes for each value like multi-key
	- for subdocs, indexes for each field in the sub document

- ```db.<collection>.createIndex({'$**': 1})```

- it creates one virtual single field index at execution time
- if filtering by multiple fields, create multiple plans using multiple single indexes then the query planner uses the score to determine which one to use.
	
- To creating wild card index on sub docs only:
	- ```db.<collection>.createIndex({"field.$**": 1})``` only subdoc and fields in the subdoc will be indexed

- **Wild card projection option**:
	- specifies set of fields to include or execlude from the index
	- ```db.<collection>.createIndex({'$**'}, {wildcardProjection: {a: 1}}) ```index only ```a``` and all its subpaths if it's a subdoc
	- ```db.<collection>.createIndex({'$**'}, {wildcardProjection: {a: 0}})``` index all but ```a```

- A **Covered Query** is a query that can be satisfied entirely using an index and does not have to examine any documents.
- An index covers a query when all of the following apply:
	- all the fields in the query are part of an index, and
	- all the fields returned in the results are in the same index.
	- no fields in the query are equal to null (i.e. {"field" : null} or {"field" : {$eq : null}} ).

- wild card index can only cover a query if the query is on a single field


- **Used Cases Examples**: 
	- unpredictable work loads
	- queries with many fields
	- arbitrary query patterns
	- implementing the attribute pattern



## Chapter 3: Index Operations
---
### Building Indexes
- Hyprid index build new in Mongodb 4.2

- **Types of index builds**:
	1. **Foreground** index build:
		- very performant
		- locks the entire db until the build is done
		- ```createIndex({a: 1})``` 
        - prior to 4.2

	2. **Background** index build:
		- doesn't lock the db
		- less performant
		- it uses incremental approach to create the index
		- the index resulting is less efficient than the foreground index
		- ```createIndex({a: 1}, {background: true})```
        - prior to 4.2

	3. Hyprid:
		- This is the only option available since 4.2
        - Using the best of both worlds:
            - No locks on the db like the Background
            - Performant like Forground


> You can learn more about building indexes by visiting the [Index Build Operations](https://www.mongodb.com/docs/manual/core/index-creation/?jmp=university) section of the MongoDB Manual.



### Query Plans
- Are a series of stages that feen into one another to run a query

- For a single query, many plans can be proposed based on the indexes
- Only one winning plan is used

- **How it works**:
	1. Fresh query comes to the db for the first time
	2. The service looks for all indexes on the collection
	3. Choose indexes viable for the query (candidate indexes)
	4. The query optimizer uses candidate indexes to generate candidate plans
	5. The quey planner is emperical query planner --> means all candidate plans will be tried over a short period of time then choose which plan performs best
	6. Then mongodb caches the winning plan for that query shape
	7. The plans will be removed from cache if:
		- The server is restarted
		- Index is rebuilt
		- Index is created or dropped
		- If threshold is reached, that is amount of work done by the first portion of the query exceeds the work done by the winning plan by a factor of 10


> You can learn more about query plans by visiting the [Query Plans](https://www.mongodb.com/docs/manual/core/query-plans/?jmp=university) section of the MongoDB Manual.



### Forcing Indexes with Hint
- Overriding default mongodb index selection with hint()
- Sometimes query optimizer uses another index that we want it to use
- ```db.collection.find().hint({ a: 1, b: 1 })``` index shape
- ```db.collection.find().hint("a_b_1")``` index name
- Use it with caution


### Resource Allocation for Indexes
- Indexes help in:
	- Optimizing queries
	- Decreasing response time

- ```db.stats()```      shows the index sizes
- ```db.col.stats()```  shows the index sizes per collection

- **Disk** 
    - Size isn't a big issue
	- If no space is available, the index won't be created
	- If we use a separate disk to store the indexes, insure it has enough disk space

- **Memory**:
	- we should have enough space to accomodate the indexes
	- if not, then disk access is required to traverse index file and will slow down queries
	- ```db.col.stats({indexDetails: true})```  shows the index sizes per collection

- **Edge Cases**:
	- Occasional reports
		- Indexes used in operational workloads should be in Memory
		- Indexes used in BI tools do not have to be always in memory
		- Usuallly BI tools should only talk to a secondary not the primary node then their related indexes should also be created only on those nodes
	- Right-end-side index incerements
        - Indexes on fields that grow monotonically ex: counts, dates, incremental ids should not always be in Memory
        - The B-Tree would be unbalanced and tend to grow to the right hand side
        - Only the right hand side is needed to be in memory (new added data)



### Basic Benchmarking
- public test suite
- private testing environment

- **Types of Perfromance Benchmarking**:
    1. Low Level Benchmarking:
        - File I/O Performance
        - Scheduler Performance
        - Memory allocation and transfer speed
        - Database server performance
        - Thread performance
        - ...
        Tools used:
            - sysbench 
            - iibench

    2. Database Server Benchmarking:
        - Data set load
        - Writes per second
        - Reads per second
        - Balanced workloads
        - Read / Write ratio
        Tools:
            - YCSB
            - TPC

    3. Distributed Systems Benchmarking:
        - Linearization of reads and writes
        - Serialization of requests
        - Fault tolerance if node fails
        tools:
            - highbench
            - jepsen

- **Benchmarking Conditions**:
	- POCDriver tool to test mongodb workloads
	- benchmarking anti-patterns:
		- Database swap replace (turning tables in sql dbs to collections)
		- Using mongo shell for write and reads requests
		- Using mongoimport to test writes response
		- Local laptop to run tests
		- Using default mongodb parameters






## Chapter 4: CRUD Optimization
---
### Optimizing your CRUD Operations
- Index selectivity ---> minimizing index keys scanned
	- Range queries aren't very selective
	- Equality is very selective
	- ```createIndex({the more selective, the less selective, ...})```
	- To use index for both sorting and filtering, the query predicate should be all equalities and no range
	
	- ```find({a: "", b: {$gt: 1}}).sort(c: 1)```
		- an index with shape ```{a: 1, b: 1, c: 1}``` won't be use in sorting
		that's because the prefix used to reach the sort will be [a, b, c] and b is range, but index with shape ```{a: 1, c: 1, b: 1}``` will be used in sorting because the prefix used to reach sort is [a, c] which has equality only


- **Equality, Sort, Range rule**: best way to define an index would be: {equality_condition, sort_conditions, range_conditions}



> You can learn more about optimizing your CRUD operations by visiting the [Create Indexes to Support Your Queries](https://www.mongodb.com/docs/manual/tutorial/create-indexes-to-support-queries/?jmp=university), [Use Indexes to Sort Query Results](https://www.mongodb.com/docs/manual/tutorial/sort-results-with-indexes/?jmp=university), and [Create Queries that Ensure Selectivity](https://www.mongodb.com/docs/manual/tutorial/create-queries-that-ensure-selectivity/?jmp=university) sections of the MongoDB Manual.



### Covered Queries
- **What are covered queries?**
    - Very performant way to service the queries to our database
    - Satisfied entirely by index
    - 0 docs needs to be examined

- A covered query has:
	- all index fields are the only fields in the predicate
	- projection to return only fields from index fields and the fields must be stated explicitly
	- no fields in the query are equal to null (i.e. ```{"field" : null}``` or ```{"field" : {$eq : null}}``` ).
    - No fetch stage is present in the query plan

- You can't cover a query if:
	- Any of the indexed fields are arrays
	- Any of the indexed fields are embedded docs
	- When run against a mongos if the index does not contain the shard key


> You can learn more about covered queries by visiting the [Query Optimization](https://www.mongodb.com/docs/manual/core/query-optimization/?jmp=university) section of the MongoDB Manual.




### Regex Performance
- Creating index on fields that we want to run regex on increases performance but we still have to run the regex against all the keys in the index
- to address this issue try to use /^regex/ to only examine a subset of the keys
- if we use it like this /^.regex/, then it has no effect



### Aggregation Performance
- **Realtime Processing**:
    - Providing data to applications   
    - Performance is important

- **Batch Processing**:
    - Providing data for analytics
    - Performance is less important

- **Index usage**:
    - if in a certain stage the index can't be used, then it won't be used in the following stages
    - pass ```{ explain: true }``` as option to view the steps of execution
    - operators that use index should be at the start of the pipline
    - match, sort, limit should be in front

- **Memory constraints**:
    - Results are subject to 16MB doc size limit applies
        - Use ```$limit``` and ```$projection``` to reduce the results size
    - 100MB Ram per stage
        - Use indexes
        - We can use disk ```db.orders.aggregate([...], {allowDiskUse: true})```
        - allowDiskUse less performant
        - graphLookup doesn't support allowDiskUse as it doesn't support spilling to disk







## Chapter 5: Performance on Clusters
----
### Performance Considerations in Distributed Systems
- Distributed Systems are Replicaset Cluster and Shard Cluster.
- Consider latency
- Data is spread across different nodes
- Read implications
- Write implications

- **In Replication**:
	- Offloading eventual consistency data to secondaries
	- specific work load to target indexes on secondaries like BI

- **In Sharding**:
	- Shard nodes must be themselves replicasets
	- Sharding is for horizontal scaling
	- You should reach vertical scaling limit before sharding
	- You need to understand how data grows and how your data is accessed to determine a good shard key
    - Sharding works by defining key based  ranges - our shard key
    - It's important to get a good shard key
	- Latency between different cluster elements
	- Putting mongos on the same server as the application server should reduce latency
    - In MongoDB there are two types of read we can perform in a shard cluster:
        - **Scatter Gather**: Where we ping all nodes of our shard cluster for the information corresponding to a given query
        - **Routed Queries**:  Where we ask one signle shard node or a small amount of shard nodes for the data that your application is requesting
        - If we don't use shard key will use Scatter Gather
	    - Routed queries are more performant than scatter gathered queries
	- Sorting limit and skip is performed locally on each shard then merged on the primary shard


- **Note**:
    > In MongoDB 4.2, we can use any of the shards (or mongos) to do final sort, limit and skip steps. For more details, you can refer to [How mongos Handles Query Modifiers](https://www.mongodb.com/docs/manual/core/sharded-cluster-query-router/#how-mongos-handles-query-modifiers) section in the documentation.

    > You can learn more about distributed system performance considerations by visiting the [Distributed Queries](https://www.mongodb.com/docs/manual/core/distributed-queries/?jmp=university) section of the MongoDB Manual.





### Increasing Write Performance with Sharding
- Picking a good shard key
- With a shard key our data are divided up into bite size piece called chunks
- Each chunk has an inclusive lower bound and exclusive upper bound
- By default a Chunk max size is 64MB
- We need to insure that the chunks are distributed evenly across shards
- **Shard Key Factors**:
    - **Cardinality**: 
        - The number of distinct values for a given shard key 
        - High Cardinality is good
        - Cardinality determines the max number of chunks that can exist in our cluster
        - Using Compound shard key insure increase the cardinality
    - **Frequency**:
        - The number of occurance of the same values in our cluster
        - High frequency will limit the equal distribution of chuncks which is bad
        - When the frequency is high then the throughput of our application would be constrained by the shard contains this repeated values and we defined this as a **Hot Shard**
        - Typically, when a chunck is close to its max size, Mongo will split it into two chunks 
        - **Jumbo Chunk** is a chunk with the same lower and upper bound and it will be no longer eligible for spliting and this will reduce the effectiveness of horizontal scaling because we won't be able to move these chuncks between shards
        - We can mitigate the issue of uneven frequency if we create a good compound shard key 
    - **Rate of Change**:
        - How our values change over time.
        - Avoid monotonically increasing or decreasing values in our shard key.
        - The examble of this is ObjectID
        - When using monotonically increasing shard key, all of our writes are going to the same shard, This is the shard that contains the upper bound of max key, which is often refered to as the last shard.
        - When using monotonically decreasing shard key, all of our writes are going to the same shard, This is the shard that contains the lower bound of min key, which is often refered to as the first shard.
        - Monotonically changing shard keys should be avoided unless they are used in compound keys and shouldn't be the first field using them this way increases the cardinality
- High cardinality, low frequency, low rate of change for shard key
- Compound keys have high cardinality and low frequency

- **Bulk Write**:
    - ```
        db.collection.bulkWrite(
            [<operation1>, <operation2>, ...],
            {ordered: <boolean>}
        )
      ```
    - **Ordered** bulkwrites are less performant on sharded cluster because the server will execute thesd operations one after another waiting for the last response to succeed, if an operation fails we immediately stop the bulk insertion and report back to the client 
    - **Unordered** bulkwrites can utilize parallization in sharded cluster, as the server will execute all these operations in parallel
    - 


> You can learn more about increasing write performance with sharding by visiting the [Distributed Write Operations](https://www.mongodb.com/docs/manual/core/distributed-queries/) and [Bulk Write Operations](https://www.mongodb.com/docs/manual/core/bulk-write-operations/?jmp=university) sections of the MongoDB Manual.


> **Note**: A monotonically increasing or decreasing values in our shard key is not desired for a heavy write workload, but may be fine for a heavy read workload.




### Reading from Secondaries
- Read preference by default is set to primary node
- ```db.find().readPref("primary")```
- There are several other read preferences available:
    - Primary ```db.find().readPref("primary")``` all reads will routed to primary node
    - Primary Prefered```db.find().readPref("primaryPrefered")```
    - Secondary ```db.find().readPref("secondary")``` all reads will be routed to one of the secondaries, write however can only be routed to primary node
    - Secondary Prefered```db.find().readPref("secondaryPrefered")``` reads will always be routed to a secondary unless there aren't any available in which case will be routed to primary
    - Nearest ```db.find().readPref("nearest")``` will read from node with the lowest network latency

- Note that when we read data from secondary node there is a possibility to read stale data

- When reading from a Secondary node is a **Good** idea:
	- Analytics queries
		- Are generally resource intensive and long running
	- Local read
		- in geo distributed clusters for low latency


> You can learn more about reading from secondaries by visiting the [Read Preference](https://www.mongodb.com/docs/manual/core/read-preference/?jmp=university) section of the MongoDB Manual.

> **Note** As of MongoDB 3.6 you can read from secondaries on sharded clusters safely.




### Replica Sets with Differing Indexes
- Use secondary with differing indexes for:
	- Analytics
	- Reporting delayed consistency data
	- Text search

- Secondary Node Considerations: it should be
    - Prevent such a secondary from becoming primary
	    - Priority = 0
	    - Hidden node
	    - Delayed secondary
	- They should not be allowed to be primary as they may not have the indexes to handle application queries

- Steps to create index on secondary only:
	1. shut down the server
	2. bring up the secondary in stand alone mode
	3. create the index
	4. bring up the replicaset again
	


### Aggregation Pipeline on a Sharded Cluster
1. If match stage uses shard key
    - all the pipeline will be routed to that shard
    - the results will be returned to mongos

2. No match
    - some stages will be splitted on each shard
    - then the results will be merged together on a single shard
    - it normally happens on random shard unless ($out, $facet, $lookup, $graphLookup) are used
    - in those cased the primary shard will do the merging


**Aggregation Optimizations**
1. if a sort is followed by match, the query optimizer will move the match above to limit the docs sorted
2. if skip is followed by limit, the query optimizer will move the limit up and change the values of each stage 
    to result in correct data ----> {skip: 10, limit: 5} will be coalesce { limit: 5, skip: 10 }
3. some stages can be combined together ---> {limit: 5}, {limt: 10} ---> {limit: 15} or skip or match if possible


> You can learn more about aggregation in a sharded cluster by visiting the [Aggregation Pipeline and Sharded Collections](https://www.mongodb.com/docs/manual/core/aggregation-pipeline-sharded-collections/?jmp=university) section of the MongoDB Manual.