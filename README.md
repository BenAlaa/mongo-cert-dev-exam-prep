![Certified Developer](./Assets/university-badges-dev-lockup.png)
![MDB University](./Assets/mdb-university.png)
![Certificate](./Assets/DEVELOPER_Certificate.jpeg)
In this repo I tried to collect sharded pieces about importent MongoDB topics from many resources that took from me around 2 months to collect them in such a simple way that I hope it will help any one how are preparing to be a **Certified MongoDB Developer** or for any one how searching for an organized source for the most important mongoDB topics that will help you to build a solid db applications and have the required knowledge to scale and maintain your databases.
# Table of Content
1. [M001 - MongoDB Basics](#m001)
    1. [Chapter 1: What is MongoDB?](#m001-ch1)
    2. [Chapter 2: Importing, Exporting and Querying Data](#m001-ch2)
        - [JSON vs BSON](#m001-json-vs-bson)
        - [Importing & exporting](#m001-importing-and-exporting)
        - [Data Explorer](#m001-data-explorer)
        - [Queries](#m001-queries)
    3. [Chapter 3: Creating and Manipulating Documents](#m001-ch3)
        - [Updating Documents using MQL- mongo shell](#m001-updating-documents-using-mql)
        - [Deleting Documents and Collections](#m001-deleting-documents-and-collections)
    4. [Chapter 4: Advanced CRUD Operations](#m001-ch4)
        - [Comparison Operators](#m001-comparison-operators)
        - [Logical Operators](#m001-logical-operators)
        - [Expressive Query Operator](#m001-expressive-query-operator)
        - [Array Operators](#m001-array-operators)
        - [Projection](#m001-projection)
            - [Examples](#m001-examples)
        - [Query a Sub-Document](#m001-query-subdocument)
    5. [Chapter 5: Indexing and Aggregation Pipeline](#m001-ch5)
        - [Aggregation Framework](#m001-aggregation-framework)
        - [sort() and limit()](#m001-sort-and-limit)
        - [Introduction to Indexes](#m001-intro-to-indexes)
        - [Introduction to Data Modeling](#m001-intro-to-data-modeling)
        - [Upsert - Update or Insert?](#m001-upsert)
2. [M103: Basic Cluster Administration](#103)
    1. [Chapter 1: The Mongod](#m103-ch1)
        - [The Mongod Introduction](#m103-mongod-intro)
        - [Configuration File](#m103-conf-file)
        - [File Structure (WiredTiger)](#m103-file-structure)
        - [Basic Commands](#m103-basic-commands)
        - [Logging Basics](#m103-loging-basics)
        - [Profiling the Database](#m103-profiling-db)
        - [Basic Security](#m103-basic-security)
            - [Authentication Mechanisms](#m103-auth-mechanisms)
            - [Authorization: Role Based Access Control](#m103-role-acl)
        - [Roles](#m103-rols)
        - [Server Tools Overview](#m103-server-tools)
    2. [Chapter 2: Replication](#m103-ch2)
        - [What is Replication?](#m103-what-is-replication)
        - [MongoDB Replica Set](#m103-mongo-rs)
        - [Setting Up a Replica Set](#m103-setting-rs)
        - [Replication Configuration Document](#m103-repl-conf)
        - [Replication Commands](#m103-repl-commands)
        - [Local DB](#m103-local-db)
        - [Reconfiguring a Running Replica Set](#m103-re-conf-rs)
        - [Reads and Writes on a Replica Set](#m103-rs-read-write)
        - [Failover and Elections](#m103-failover-election)
        - [Write Concerns](#m103-write-concerns)
        - [Read Concerns](#m103-read-concerns)
    3. [Chapter 3: Sharding](#m103-ch3)
        - [What is Sharding?](#m103-what-is-sh)
        - [Sharding Architecture](#m103-sh-arch)
        - [Setting Up a Sharded Cluster](#m103-setting-sh-cluster)
        - [Config DB](#m103-conf-db)
        - [Shard Keys](#m103-shard-keys)
        - [Picking a Good Shard Key](#m103-good-shard-key)
        - [Hashed shard key](#m103-hashed-sh-key)
        - [Chunks](#m103-chunks)
        - [Balancing](#m103-balancing)
        - [Queries in a Sharded Cluster](#m103-query-sh)
        - [Targeted Queries vs Scatter Gather](#m103-target-queries)
4. [M201: MongoDB Performance](#m201)
   1. [Chapter 1: Introduction](#m201-ch1)
      - [Hardware Considerations & Configurations](#m201-hardware)
         - [Memory](#m201-hardware-memory)
         - [CPU](#m201-hardware-cpu)
         - [I/O](#m201-hardware-io)
   2. [Chapter 2: MongoDB Indexes](#m201-ch2)
      - [Introduction to Indexes](#m201-indexes-intro)
      - [How Data is Stored on Disk](#m201-data-on-disk)
      - [Single Field Indexes](#m201-single-field-index)
      - [Understanding Explain](#m201-explain)
      - [Sorting with Indexes](#m201-index-sorting)
      - [Querying on Compound Indexes](#m201-compound-index)
      - [When you can sort with compound Indexes ?](#m201-sorting-compound-index)
      - [Multikey Indexes](#m201-multikey-index)
      - [Partial Indexes](#m201-partial-index)
      - [Text Indexes](#m201-text-index)
      - [Collations](#m201-collations)
      - [Wildcard Index Type](#m201-wildcard-index)
   3. [Chapter 3: Index Operations](#m201-ch3)
      - [Building Indexes](#m201-building-indexes)
      - [Query Plans](#m201-query-plans)
      - [Forcing Indexes with Hint](#m201-indexes-hint)
      - [Resource Allocation for Indexes](#m201-resource-allocation)
      - [Basic Benchmarking](#m201-benchmarking)
   4. [Chapter 4: CRUD Optimization](#m201-ch4)
      - [Optimizing your CRUD Operations](#m201-optimizing-crud)
      - [Covered Queries](#m201-coverd-queries)
      - [Regex Performance](#m201-regex-performance)
      - [Aggregation Performance](#m201-agg-performance)
   5. [Chapter 5: Performance on Clusters](#m201-ch5)
      - [Performance Considerations in Distributed Systems](#m201-dist-sys-performance)
      - [Increasing Write Performance with Sharding](#m201-sharding)
      - [Reading from Secondaries](#m201-reading-secondaries)
      - [Replica Sets with Differing Indexes](#m201-replset)
      - [Aggregation Pipeline on a Sharded Cluster](#m201-agg-sharded-cluster)





---


# M001 - MongoDB Basics <a id="m001"></a>
Learn the fundamentals of MongoDB.
## Chapter 1: What is MongoDB? <a id="m001-ch1"></a>
- NoSQL Document DB
- DB ...> Collections ...> Documents ...> Fields and values.

- Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.

- Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

- Cluster - group of servers that store your data.

## Chapter 2: Importing, Exporting and Querying Data <a id="m001-ch2"></a>

### JSON vs BSON
- BSON simply stands for “Binary JSON,”
- BSON have more data types like date, and more performante and small size
- JSON easier to read, but havint all data types, take more space and has worse performance in parsing text
- Mongodb stores data with BSON internally and over network.
- JSON can be natively stored and retrived in mongo.
- BSON provide addetional features like speed and flexibility.


### Importing & exporting
- SRV connection string - a specific format used to establish a secure connection between your application and a MongoDB instance
- Docs Links:
    - [mongoimport](https://www.mongodb.com/docs/database-tools/mongoimport)
    - [mongoexport](https://www.mongodb.com/docs/database-tools/mongoexport/)
    - [mongodump](https://www.mongodb.com/docs/database-tools/mongodump/)
    - [mongorestore](https://www.mongodb.com/docs/database-tools/mongorestore/)

- mongo(export|import) ---> in json
- mongo(dump|restore) ---> in bson

- mongoexport --uri "" --collection="<collection name>" --out="<filename>".json
- mongoimport --uri "" --drop <json_file>

- mongodump --uri "<Atlas cluster uri>"
- mongorestore --uri --drop <dump_Folder>

- we using --drop flag to remove the existing file or folder before restore


Example Commands:

    mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json
    
    mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

    mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

    mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json

### Data Explorer

- Namespace: The concatenation of the database name and collection name is called a namespace.

- We looked at the sample_training.zips collection and issued the following queries:

        {"state": "NY"}
        {"state": "NY", "city": "ALBANY"}

### Queries

- admin is the default database that has adminstrative data like traking users of databases
- show dbs -> showing list of the dbs in the cluster
- use <dbName> -> set one db to query on and operate with
- show collections -> showing list of available collections in the current db
- it : iterates through the cursor.
- a curser: is a pointer to result set of a query.
- a pointer: is a direct address of the memory location
    ```
    db.<collection name>.find(<query>).count()
    ```

- count: is cursor method that reture the number of documents that matches the query.
- pretty: is a method or directive the formate the query result in an organized way.
- find query examples:
    ```
    db.zips.find({"state": "NY"}).count()
    ```
    ```
    db.zips.find({"state": "NY", "city": "ALBANY"})
    ```
    ```
    db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()
    ```





## Chapter 3: Creating and Manipulating Documents <a id="m001-ch3"></a>
- Every document must have a unique _id value
- "_id" required in every document
- ObjectId() is the default value for the "_id" key unless it's specified.
- If you insert a document with an existed _id key you will get Duplicate Key Error.
- Docs link about [ObjectId](https://www.mongodb.com/docs/manual/reference/method/ObjectId/)
- MongoDB has schema validation functionality allows you to enforce document structure.
- More about [schema validation](https://www.mongodb.com/docs/manual/core/schema-validation/)

- We can insert many documents in one time using insertMany([]) or insert([]) method .
    ```
    db.inspections.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])
    ```

- If one document failed to be inserted the insert method will terminate by default for example in this command we try to insert documents with duplicate _id so the first document will be inserted successfully and then terminate when the error happend:
    ```
    db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                        { "_id": 3, "test": 3 }])
    ```
- This behaviour because the default is to insert the documents in order.
- to change this behavior you can add option {"ordered": false}
    ```
    db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },
                        { "_id": 3, "test": 3 }],{ "ordered": false })
    ```
- in this case the first and third documents will be inserted.
- if you insert documents in a collection that not existed yet, by default mongo will create collection for you and insert the documents in it.

### Updating Documents using MQL- mongo shell

- to learn more about [update operators](https://www.mongodb.com/docs/manual/reference/operator/update)

- there is to method:
    - updateOne -> to update the first document that matchs the query
    - updateMany -> to update all documents that matches the query.
- Some update operators examples:
    - $inc -> increments field value by a specific amount.
        ```
        {"$inc: {"pop": 10, <field2>: <inc value>, ...}}
        ```
    - $set -> set field value to a new specific value.
        ```
        {"$set: {"pop":12, <field2>: <new value>, ...}}
        ```
    - $push -> adds an element to an array field.
        ```
        {"$push":{<field1>: <value1>, ...}}

- Examples:
    - Update all documents in the zips collection where the city field is equal to "HUDSON" by adding 10 to the current value of the "pop" field.
        ```
        db.zips.updateMany({ "city": "HUDSON" }, { "$inc": { "pop": 10 } })
        ```
    - Update a single document in the zips collection where the zip field is equal to "12534" by setting the value of the "pop" field to 17630.
        ```
        db.zips.updateOne({ "zip": "12534" }, { "$set": { "pop": 17630 } })
        ```
    - Update a single document in the zips collection where the zip field is equal to "12534" by setting the value of the "population" field to 17630.
        ```
        db.zips.updateOne({ "zip": "12534" }, { "$set": { "population": 17630 } })
        ```
    - Find all documents in the grades collection where the student_id field is 151 , and the class_id field is 339.
        ```
        db.grades.find({ "student_id": 151, "class_id": 339 }).pretty()
        ```
    - Find all documents in the grades collection where the student_id field is 250 , and the class_id field is 339.
        ```
        db.grades.find({ "student_id": 250, "class_id": 339 }).pretty()
        ```
    - Update one document in the grades collection where the student_id is ``250`` *, and the class_id field is 339 , by adding a document element to the "scores" array.
        ```
        db.grades.updateOne({ "student_id": 250, "class_id": 339 },
                    { 
                        "$push": { 
                            "scores": { 
                                "type": "extra credit",
                                "score": 100
                            }
                        }
                    }
                )
        ```


### Deleting Documents and Collections

- deleteOne -> to delete the first document that matches the given query.
- deleteOne -> to delete all documents that match the given query.
- to drop the whole collection we can use the following command: 
    ```
    db.<collectionName>.drop()
    ```



## Chapter 4: Advanced CRUD Operations <a id="m001-ch4"></a>

- $ has multi usage:
    - Precedes MQL operators.
    - Precedes Aggregations pipeline stages.
    - Allow access to field value.

### Comparison Operators
- Comparison Operators provide additionl way to locate data in the database like:
    - > $eq = Equal to
    - > $ne != Not Equal to
    - > $gt > Greater Than
    - > $lt < Less Than
    - > $gte >= Greater than or equal
    - > $lte <= Less Than or Equal
- When comparison operator is not specified $eq operator will be used by default.
- We can use Comparison operator by this way:
    ```
    {<field>: {<operator>: <value>}}
    ```
- Some examples:
    - Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was not Subscriber
        ```
        db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()
        ```
    - Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using a redundant equality operator
        ```
        db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()
        ```
    - Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using the implicit equality operator
        ```
        db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": "Customer" }).pretty()
        ```


### Logical Operators

-
    - > $and - Match all specified query clauses
    - > $or - At least one of the query clauses is matched
    - > $nor - Fail to match both given clauses
    - > $not - Negates the query requirements
- $and, $or, $nor syntax is
    ```
    {<operator>: [{statement1}, {statement2}, ...]}
- with $not the array is not required:
    ```
    {$not: {statement}}
    ```
- $and allways in the query by default implicily used by default when an operator is not specified.
    thats mean this
    ```
    {"sector": "Mobile", "result": "warning"}
    ```
    is the same as:
    ```
    {$and: [{"sector": "Mobile"}, {"result": "warning"}]}
    ```

    Also another place to find implicit and is when you apply multiple conditions on the same field
    For example find which students ids are > 25 and < 100 in the sample_training.grades collection
    ```
    {"$and": [{"student_id": {"$gt": 25}}, {"student_id": {"$lt": 100}}]}
    ```
    is the same as
    ```
    {"student_id": {"$gt": 25, "$lt": 100}}
    ```

- You will need to use Expicit $and when you need to use the same operator more than one time in the same query

    - for exmaple: Find all documents where airplanes CR2 or A81 left or landed in the KZN airport:

        ```
        db.routes.find({ 
            "$and": [ 
                { "$or" :
                    [ 
                        { "dst_airport": "KZN" },
                        { "src_airport": "KZN" }
                    ]
                },
                { "$or" :
                    [ 
                        { "airplane": "CR2" },
                        { "airplane": "A81" } 
                    ]
                }
            ]
        }).pretty()
        ```




### Expressive Query Operator

- > $expr allows to use of aggregations expressions  within the query language

- this is the syntax
    ```
    {"$expr": { <expression> }}
    ```
- > $expr allows us to use varialbles and conditional statments
- We can compare fields within the same document to each other
- Examples: 
    - Find all documents where the trip started and ended at the same station:
        ```
        db.trips.find({ 
            "$expr": { 
                "$eq": [ "$end station id", "$start station id"] 
            }
        }).count()
        ```
    - Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station:
        ```
        db.trips.find({ 
            "$expr": { 
                "$and": [ 
                    { "$gt": [ "$tripduration", 1200 ]},
                    { "$eq": [ "$end station id", "$start station id" ]}
                ]
            }
        }).count()
        ```

- usages of $
    - denotes the use of an operator
    - address the field value


### Array Operators
When quering an array field using an array will returns only exact array matches.

1. $push
    1. Allow us to add an element to an array field.
    2. Turns a field into an array field if it was previously a different type

2. $all

    This operator returns a cursor with all documents that has array field contains at least this array of items regardless of there order in the array
        ```
        db.collName.find({fieldArray: {$all: ["item1", "item2"]}})
        ```

3. $size

    This operator returns a cursor with all documents where the specified field array with given length length
        ```
        db.collname.find({fieldArray: {$size: 10}})
        ```

4. $elemMatch

    - Matched documents that contain an array field with at least one element that matches the specified query criteria.
        ```
        {<field>: {"$elemMatch": {<field>:<value>}}}
        ```

    - Can be used with projection to specifies which fields should or should not be included in the result cursor
        ```
        db.<collection>.find({<query>}, {<projection>})
        ```

### Projection
- Only include the fields you want in the cursor result
- By default all the fields will be returened.
- You can apply projection by adding a second argument to the query
    ```
    db.<collection>.find({<query>}, {field1: 1, field2: 1})
    ```
- 1- include the field -> just will returns the fields specified
- 0- exclude the field -> will return all the fields except the specified ones
- Use only 1s or only 0s
- You can only use both 1 and 0 with _id field because it will be included by default
    ```
    db.<collection>.find({<query>}, {<field>:1, _id:0})

#### Examples:
- Find all documents with exactly 20 amenities which include all the amenities listed in the query array, and display their price and address:

    ```
    db.listingsAndReviews.find({ "amenities":
    { "$size": 20, "$all": [ "Internet", "Wifi",  "Kitchen", "Heating",
                                "Family/kid friendly", "Washer", "Dryer",
                                "Essentials", "Shampoo", "Hangers",
                                "Hair dryer", "Iron",
                                "Laptop friendly workspace" ] } },
                        {"price": 1, "address": 1}).pretty()
    ```

- Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor:

    ```
    db.listingsAndReviews.find({ "amenities": "Wifi" },
                        { "price": 1, "address": 1, "_id": 0 }).pretty()
    ```

- Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor, also exclude ``"maximum_nights"``. **This will be an error:*

    ```
    db.listingsAndReviews.find({ "amenities": "Wifi" },
                        { "price": 1, "address": 1,
                            "_id": 0, "maximum_nights":0 }).pretty()
    ```

- Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment:

    ```
    db.grades.find({ "class_id": 431 },
            { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
            }).pretty()
    ```

- Find all documents where the student had an extra credit score:

    ```
    db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
            }).pretty()
    ```



### Query a Sub-Document

We using dot notation to access sub-document fields in our query.
For array sub document we can access array elements with it's index.
```
    db.companies.find(
        {"relationships.0.person.last_name": "Zuckerberg"},
        {"name": 1}
    ).pretty()
```
> 0: position of the first array element
> person: field name with a nested object as value
> last_name: field name within the person sub-document
> "Zuckerberg": value that we are looking for
> {"name":1}: projection to just include the company name in the resulting cursor




## Chapter 5: Indexing and Aggregation Pipeline <a id="m001-ch5"></a>


### Aggregation Framework

In it's simplest form, ianother way to query data in MongoDB.

Let's find all documents that have wifi as one of the amenities only include preice and address in the resulting cursor
```
db.listingAndReviews.find(
    {"amenities": "Wifi"},
    {"price": 1, "address":1, _id:0}
).pretty()
```

```
db.listingAndReviews.aggregate([
    {$match: {"amenities": "Wifi"}},
    {$project: {"price": 1, "address":1, _id:0}}
])
```

aggregation pipeline is array off stages the the documents go through one by one and the order is matter as the output of one stage is the input of the next stage.
documents -> match -> project -> final result

> $group is An operator or stage that take the incoming stream of data and siphon it into multiple distinct reservoirs.

to find which countries are listed in the sample_airbnb.listingsAndReviews collection?
```
{
    $group:
        {
            _id: "$address.country", // Group By Expression
            "count": {"$sum": 1} // Accumelator
        }
}
```


### sort() and limit()
They are cursor methods they applied to resulting cursor
- Sort: 1 -> for increasing, -1 -> for decreasing
- you should use sort first before limit

```
use sample_training

db.zips.find().sort({ "pop": 1 }).limit(1)

db.zips.find({ "pop": 0 }).count()

db.zips.find().sort({ "pop": -1 }).limit(1)

db.zips.find().sort({ "pop": -1 }).limit(10)

db.zips.find().sort({ "pop": 1, "city": -1 })
```


### Introduction to Indexes

- Using indexing like in the book is way more faster to find what you searching for in the book instead of going from the first page.
- in Database index is a special data structure that stores a small portion of the collection's data set in an easy to traverse form
- When we using indexes
    1. Support your query.
    2. Avoid sorting as the index itself is sorted.
- Types of index
    1. single field index.
    2. compund index.


```
use sample_training

db.trips.find({ "birth year": 1989 })

db.trips.find({ "start station id": 476 }).sort( { "birth year": 1 } )

db.trips.createIndex({ "birth year": 1 })

db.trips.createIndex({ "start station id": 1, "birth year": 1 })
```


### Introduction to Data Modeling
Data modeling - a way to organize fields in a document to support your application performance and querying capabilities.
1. Data is stored in the way that it is used. (What we will store? and How it will be queried?)
2. Data that is used together should be stored together.
3. Evolving application implies an evolving data model.

To learn more about data modeling with MongoDB, take our [Data Modeling Course](https://university.mongodb.com/courses/M320/about)! Check out our [documentation](https://docs.mongodb.com/manual/core/data-modeling-introduction/) and [blog](https://www.mongodb.com/blog/post/building-with-patterns-a-summary).


### Upsert - Update or Insert?

Everthing in MQL that is used to locate a document in a collection can also be used to modify this document.

```
db.collection.updateOne({<query>}, {<update>})
```

Upsert is a hybrid of update and insert, it should only be used when it is needed.

```
db.collection.updateOne({<query>},{<update>},{"upsert": true})
```

- If upsert is true:
    - is there a match? -> then update the matched document
    - is not ther a match? -> then insert a new document



---



# M103: Basic Cluster Administration <a id="m103"></a>
Learn the essentials of database administration in MongoDB.
## Chapter 1: The Mongod <a id="m103-ch1"></a>

### The Mongod Introduction <a id="m103-mongod-intro"></a>
- **daemon** is a program or process that's meant to be run but not interacted with directly.
- **mongod** is the main daemon for mongodb, is the core server of the database to handle connections, requests and process data.
- has the all main configurations options to make your data secure and consistent.
- each server has an instance of mongod
- if we have a cluster each server will run one mongod instance.
- can't be interacted with
- mongo client is what we interact with
- mongo client can communicate with the daemon
- We issue database commands like, insert and update to the client and the client take care to communicate with mongod to execute those commands.
- to start up a mongod process we need to run this command in the terminal "mongod"
- we can configure mongod by providing a configurations file or specifing flags
- There is  some defaults configurations when launch mongod without any options:
    - port 27017                                   ``--port <port>``
    - dbpath: /data/db                             ``--dbpath <path>`` where the data is stored like (dbs-collectoins-indexes-journaling info)
    - bind_ip: localhost                           ``--bind_ip <....ip1 , ....ip2 , ....>``
    - auth: disabled                               ``--auth`` auth enables authentication to control which users can access the database. When auth is specified, all database clients who want to connect to mongod first need to authenticate. 
    Before any database users have been configured, a Mongo shell running on localhost will have access to the database. We can then configure users and their permission levels using the shell. Once one or more users have been configured, the shell will no longer have default access
- will use mongo shell client to communicate with mongod by running this command "mongo"
- once mongo shell is connected to mongod you can issue db commands
- to shutting down the daemon we using the following commands:
    ```
    use admin
    db.shutdownServer()
    ```
- Other mongodb clients:
    - MongoDB Compass
    - Drivers (Node, swift, java, ...)


### Configuration File <a id="m103-conf-file"></a>
#### command line options and their respective config commands

- ``--dbpath``   -->  ``storage.dbPath``
- ``--logpath``  -->  ``systemLog.path`` and systemLog.destination  ***must be provided if using fork***
- ``--bind_ip``  -->  ``net.bindIp``
- ``--replSet``  -->  ``replication.replSetName``
- ``--keyFile``  -->  ``security.keyFile``
- ``--sslPEMKey`` --> ``net.ssl.sslPEMKey``
- ``--sslCAKey``  --> ``net.ssl.sslCAKey``
- ``--sslMode``   --> ``net.sslMode``
- ``--fork``      --> ``processManagement.fork`` (to tell mongod to run as a daemon)  must be used with logpath or syslog
- ``--port``      --> ``net.port``

Commands Examples:
- Launch mongod with specified --dbpath and --logpath:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log
    ```
- Launch mongod and fork the process:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log --fork
    ```
- Launch mongod with many configuration options:
    ```
    mongod --dbpath /data/db --logpath /data/log/mongod.log --fork --replSet "M103" --keyFile /data/keyfile --bind_ip "127.0.0.1,192.168.103.100" --tlsMode requireTLS --tlsCAFile "/etc/tls/TLSCA.pem" --tlsCertificateKeyFile "/etc/tls/tls.pem"
    ```

All these options works fine but we need to rewrite all the options every time we need to startup mongod, instead we can use configurations file to store all the required options:

Config file is a YAML file (**Y**et **A**nother **M**arkup **L**anguage)

Example configuration file, with the same configuration options as above:
```
storage:
  dbPath: "/data/db"
systemLog:
  path: "/data/log/mongod.log"
  destination: "file"
replication:
# This is the name of the replica set for M103
  replSetName: M103
net:
  bindIp : "127.0.0.1,192.168.103.100"
tls:
  mode: "requireTLS"
  certificateKeyFile: "/etc/tls/tls.pem"
  CAFile: "/etc/tls/TLSCA.pem"
security:
  keyFile: "/data/keyfile"
processManagement:
  fork: true
```

To use config file there are to ways
- ```mongod --config "path_to_config_file"``` "/etc/mongod.conf"
- ```mongod -f "path_to_config_file"``` "/etc/mongod.conf"

For all available config file options you call check this:
```
systemLog:
   destination: file
   path: "/var/log/mongodb/mongod.log"
   logAppend: true
storage:
   dbPath: "/data/db"
   journal:
      enabled: true
processManagement:
   fork: true
net:
   bindIp: 127.0.0.1
   port: 27017
setParameter:
   enableLocalhostAuthBypass: false
...
##############################################################################################################
systemLog:
   verbosity: <int>
   quiet: <boolean>
   traceAllExceptions: <boolean>
   syslogFacility: <string>
   path: <string>
   logAppend: <boolean>
   logRotate: <string>
   destination: <string>
   timeStampFormat: <string>
   component:
      accessControl:
         verbosity: <int>
      command:
         verbosity: <int>

cloud:
   monitoring:
      free:
         state: <string>
         tags: <string>

net:
   port: <int>
   bindIp: <string>
   bindIpAll: <boolean>
   maxIncomingConnections: <int>
   wireObjectCheck: <boolean>
   ipv6: <boolean>
   unixDomainSocket:
      enabled: <boolean>
      pathPrefix: <string>
      filePermissions: <int>
   ssl:                            # deprecated since 4.2
      sslOnNormalPorts: <boolean>  # deprecated since 2.6
      mode: <string>
      PEMKeyFile: <string>
      PEMKeyPassword: <string>
      certificateSelector: <string>
      clusterCertificateSelector: <string>
      clusterFile: <string>
      clusterPassword: <string>
      CAFile: <string>
      clusterCAFile: <string>
      CRLFile: <string>
      allowConnectionsWithoutCertificates: <boolean>
      allowInvalidCertificates: <boolean>
      allowInvalidHostnames: <boolean>
      disabledProtocols: <string>
      FIPSMode: <boolean>
   tls:
      certificateSelector: <string>
      clusterCertificateSelector: <string>
      mode: <string>
      certificateKeyFile: <string>
      certificateKeyFilePassword: <string>
      clusterFile: <string>
      clusterPassword: <string>
      CAFile: <string>
      clusterCAFile: <string>
      CRLFile: <string>
      allowConnectionsWithoutCertificates: <boolean>
      allowInvalidCertificates: <boolean>
      allowInvalidHostnames: <boolean>
      disabledProtocols: <string>
      FIPSMode: <boolean>
      logVersions: <string>
   compression:
      compressors: <string>

security:
   keyFile: <string>
   clusterAuthMode: <string>
   authorization: <string>
   transitionToAuth: <boolean>
   javascriptEnabled:  <boolean>
   redactClientLogData: <boolean>
   clusterIpSourceAllowlist:
     - <string>
   sasl:
      hostName: <string>
      serviceName: <string>
      saslauthdSocketPath: <string>
   enableEncryption: <boolean>
   encryptionCipherMode: <string>
   encryptionKeyFile: <string>
   kmip:
      keyIdentifier: <string>
      rotateMasterKey: <boolean>
      serverName: <string>
      port: <string>
      clientCertificateFile: <string>
      clientCertificatePassword: <string>
      clientCertificateSelector: <string>
      serverCAFile: <string>
      connectRetries: <int>
      connectTimeoutMS: <int>
   ldap:
      servers: <string>
      bind:
         method: <string>
         saslMechanisms: <string>
         queryUser: <string>
         queryPassword: <string>
         useOSDefaults: <boolean>
      transportSecurity: <string>
      timeoutMS: <int>
      userToDNMapping: <string>
      authz:
         queryTemplate: <string>
      validateLDAPServerConfig: <boolean>

setParameter:
   <parameter1>: <value1>
   <parameter2>: <value2>

storage:
   dbPath: <string>
   journal:
      enabled: <boolean>
      commitIntervalMs: <num>
   directoryPerDB: <boolean>
   syncPeriodSecs: <int>
   engine: <string>
   wiredTiger:
      engineConfig:
         cacheSizeGB: <number>
         journalCompressor: <string>
         directoryForIndexes: <boolean>
         maxCacheOverflowFileSizeGB: <number> // deprecated in MongoDB 4.4
      collectionConfig:
         blockCompressor: <string>
      indexConfig:
         prefixCompression: <boolean>
   inMemory:
      engineConfig:
         inMemorySizeGB: <number>
   oplogMinRetentionHours: <double>

operationProfiling:
   mode: <string>
   slowOpThresholdMs: <int>
   slowOpSampleRate: <double>
   filter: <string>

replication:
   oplogSizeMB: <int>
   replSetName: <string>
   enableMajorityReadConcern: <boolean>

sharding:
   clusterRole: <string>
   archiveMovedChunks: <boolean>

auditLog:
   destination: <string>
   format: <string>
   path: <string>
   filter: <string>

snmp:
   disabled: <boolean>
   subagent: <boolean>
   master: <boolean>
```

- For all command line options check this doc [reference](https://www.mongodb.com/docs/manual/reference/program/mongod/#options)
- For all configurations file options check this doc [reference](https://www.mongodb.com/docs/manual/reference/configuration-options/)



**Note**
To create admin user for the lab use this example
```
mongo admin --host localhost:27000 --eval '
  db.createUser({
    user: "m103-admin",
    pwd: "m103-pass",
    roles: [
      {role: "root", db: "admin"}
    ]
  })
'
```


### File Structure (WiredTiger)  <a id="m103-file-structure"></a>
You typically don't need to interact with this data folder to be modified may be to be read only.
These files isn't designed for user modefications and if you modefied them you may face crashes or data lose.

- To List --dbpath directory:
    > ls -l /data/db
    ```
    WiredTiger      
    WiredTige.wt  
    WiredTiger.lock 
    WiredTiger.turtle  
    WiredTigerLAS.wt   
    _mdb_catalog.wt
    mongod.lock
    sizeStorer.lock
    collection-n.wt
    index-n.wt
    diagnostic.data
    journal
    storage.bson
    ```

**wiredTiger files**: 
this group of files is related to how wiredTiger storage engine keep track of info like cluster metadata and wiredTiger specific configurations options
  - wiredTiger.lock: act as a safty that prevent another mongodb process to point the same data folder


**Collections and Index files .wt**:
- The next group is the files end with .wt these files is the collection and index data itself.
- each collection and index has it's own file.
- These files are designed to interact with through the Mongodb server process rathera third party tool.
- Modeifying these can lead to data lose and crashes

**diagnostic.data folder**: 
  - to collect the dignostic data which is captured by Full Time Data Capture, or FTDC module.
  - These data is only use for diagnostic purpose by mongodb support engineers
  - It collects data from the following commands:
      ```
      serverStatus: db.serverStatus({tcmalloc: true})
      replSetGetStatus: rs.status()
      collStats for local.oplog.rs: db.getSiblingDB('local').oplog.rs.status()
      getCmdLineOpts: db.adminCommand({getCmdLineOpts: true})
      buildInfo: db.adminCommand({buildInfo: true})
      hostInfo: db.adminCommand(hostInfo: true)
      ```

**journal files**:
  - write operations are buffered in memory and flushed every 60 seconds or when the journal files reach 2 Gbytes
  - write ahead logging system to on-disk journal file. It's first buffered in memory and synced to disk every 50ms
  - journal file max 100 mega bytes in size
  - wiredTiger can use journal files to recover data in case of failure

**mongod.lock** is like wiredTiger.lock file

**mongod.log** file saves log files!

**mongodb-27017.sock** file in /tmp folder: socket file use to create socket connection to the specified port


### Basic Commands  <a id="m103-basic-commands"></a>
- Shell helpers ---> wraps db commands
   ```
   db.<method>() --> database commands
   rs.<method>() --> replica set commands
   sh.<method>() --> sharding commands
   ```
- > db.<'Collection'>.<'method'>
- user management:
   ```
   - db.createUser()
   - db.dropUser()
   ```

- collection management:
   ```
   - db.renameCollection()
   - db.collection.createIndex()
   - db.collection.drop()
   ```

- Database management:
   ```
   - db.dropDatabase()
   - db.createCollection()
   ```

- Database status:
   ```
   - db.serverStatus()
   ```

- Database commands: under the hood
   ```
   - db.runCommand({ <'Command'> })
   - db.commandHelp("command")
   ```

- Creating index with Database Command:
   ```
   db.runCommand({
      "createIndexes":"<collection_name>",
         "indexes":[
            {
               "key":{ "product": 1 },
               "name": "name_index"
            }
         ]
      }
   )
   ```

- Creating index with Shell Helper:
   ```
   db.<collection>.createIndex(
      { "product": 1 },
      { "name": "name_index" }
   )
   ```


### Logging Basics  <a id="m103-loging-basics"></a>
MongoDB provide to logging facilities to tracking activites on your database:
1. **Process Log**: collectes the activites into one of the following components: 
   - ACCESS - messages related to access control, such as authentication
   - COMMAND - messages related to database commands
   - CONTROL - messages related to control activities such as initialization
   - FTDC - messages related to the diagnostic data collection mechanism
   - GEO - messages related to parsing geo-spatial shapes
   - INDEX - messages related to indexing operations
   - NETWORK - messages related to network activities such as  accepting connections
   - QUERY - messages related to query planner and other query activities
   - REPL - messages related to replica set, such as initial sync or hearbeats
   - REPL_HB - messages related to  replica set heartbeats  nested under replication
   - ROLLBACK - messages related to replica set ROLLBACK  nested under replication
   - SHARDING - messages related to sharding operations
   - STORAGE - messages related to storage activities
   - JOURNAL - messages related to journaling activities
   - WRITE - messages related to write operations, such as update commands

to retrive a log commponents from db I can use this command:
   ```db.getLogComponents()```
#### Log verbosity Levels
  - verbosity: parent verbosity on the object level  ranges (0 -> 5) the higher the more verbose
  - component.verbosity: for each individual component if set to -1 it will inherit the parent verbosity
  - -1 : inherit from parent
  - 0: Default verposity to include informational message
  - 1-5: increase  the verbosity level to include debug messages

To look to logs we can use one of the following:
   1. ```db.adminCommand({"getLog": "global"})```
   2. ```tail -f 100 /path/to/log/file```

To change the verbosity level of one component use the following command:
   ```db.setLogLevel(<level>: "<component>")```


#### Severity levels: 
> F - fatal
E - error
W - warning
I - info (Verbosity Level 0)
D - debug (Verbosity Level 1-5)


#### Log Message sections:
``` 
<timestamp> 
<severity> 
<component> 
[<connection>] 
action <action>  ex command admin.$cmd  $ indicates db command
appName: <clientThatTriggeredOperation> ex mongoShell 
command: <the command itself> 
<metadata>
<operationTime>   10ms
```



### Profiling the Database  <a id="m103-profiling-db"></a>

- Profiling stores more detailed info than logging
- not all actions are captured on profiler
- Profiler is enabled on database level for each db separatly
- if enabled, stores all operations on db in a new collection called ```system.profile```

**Events Captured by the Profiler:**
   - CRUD
   - Adminstrative operations
   - Configuration operations events are captured by profiler

**Profiler Settings:**
   - 0 -> Profiler is off and does not collect any data, this is the default.
   - 1 -> Profiler collects data for the operations that take longer than the value of ```ms```
   - 2 -> Profiler collect all data for all operations

**slow ops**: by default any operation that takes longer than 100ms  and can be adjusted by setting the slowms variable

- To get the Profiling Level:
   ```db.getProfilingLevel()```
- To set the Profiling Level:
   ```db.setProfilingLevel(<level>, {slowms: <Number>})```


### Basic Security  <a id="m103-basic-security"></a>

#### Authentication Mechanisms:  <a id="m103-auth-mechanisms"></a>
1. SCRAM(**S**alted **C**halleng **R**esponse **A**uthentication **M**echanism): is the default and the most basic form of client authentication (**Community Edition**)

2. X.509: this form using X.509 certificate for authentication. this is more secure and complex(**Community Edition**)

3. LDAP(**L**ightweight **D**irectory **A**ccess **P**rotocol)(**Enterprise Only**)

4. KERBEROS:(**Enterprise Only**)



#### Authorization: Role Based Access Control  <a id="m103-role-acl"></a>

- Each user has one or more roles.
- Each role has one or more privileges.
- A privilege represents a group of actions and the resources to those actions apply to.
- Roles support a high level of responsibility isolation for operational tasks.

**Note**
> when the mongodb is run for the first time, no users exist in the db
to connect you must connect from the local machine on which the server is run
after creating the first user, the local host execution is closed

Localhost Exceptions:
- Allows you to access a MongoDB server that enforces authentiaction but doesn't yet have a configured user for you to authenticate with.
- Must run the mongo shell from the same host
- The local exception closes after you created the first user
- Always create a user with adminstrative privileges first


Create new user with the root role (also, named root):
```
use admin
db.createUser({
  user: "root",
  pwd: "root123",
  roles : [ "root" ]
})
```

Connect to mongod and authenticate as root:
```
mongo --username root --password root123 --authenticationDatabase admin
```


### Roles  <a id="m103-rols"></a>
- Database users are granted roles
- **Custom Roles** : tailored roles to attend  specific needs of sets of users
- **Built-In Roles**: Pre-packed MongoDB Roles
- **Role structure**: 
   - set of privileges each privilege defines a set of actions over a resource
   - resources:
      - specific db and specific collection ```{db: "products", collection: "inventory"}```
      - all dbs and all collections ```{db: "", collection: ""}```
      - all dbs and specific collection ```{db:"", collection:"accounts"}```
      - specific db and any collection ```{db:"products", collection: ""}```
      - cluster --> replicasets or shards ```{cluster: true}```
   - Actions allowed over a resource ```{ resource: { cluster: true }, actions: ["shutdown"] }```
   - role can inherit from other roles
   - network auth previliges  (ip whitelist)

**Built-in Roles Sets**:
- Per Database level:
   - Database User
      - read
      - readWrite
   - Database Administration
      - dbAdmin
      - userAdmin
      - dbOwner
   - Cluster Administration
      - clusterAdmin
      - clusterManager
      - clusterMonitor
      - hostManager
   - Backup/Restore
      - backup
      - restore
   - Super user
      - root

- All Databases level:
   - readAnyDatabase
   - readWriteAnyDatabase
   - dbAdminAnyDatabase
   - userAdminAnyDatabase
   - root


**Main Common Roles**:
- **userAdmin role**: 
  - have all actions on user management (create user, change password, grant role, view role .....)
      - changeCustomData
      - changePassword
      - createRole
      - createUser
      - dropRole
      - dropUser
      - grantRole
      - revokeRole
      - setAuthenticationRestriction
      - viewRole
      - viewUser
  - doesn't have any access on data cannot list, read or write any data other than the users
      ```
      use admin
      db.createUser(
         {
            user: "m103-application-user",
            pwd: "m103-application-pass",
            roles: [
               {
                     db: "applicationData",
                     role: "readWrite"
               }
            ]
         }
      );
      ```

- **dbAdmin role**:
  - has access to DDL operations only.
  - Provides the ability to perform administrative tasks such as schema-related tasks, indexing, and gathering statistics. 
  - This role does not grant privileges for user and role management.
       ```
      db.creatUser(
         {
            user: "user_name",
            pwd: "p@ssw0rd",
            roles: [
               {
                     db: "m103",
                     role: "dbAdmin"
               }
            ]
         }
      );
      ```
  
  - system.profile collection:
      - changeStream
      - collStats
      - convertToCapped
      - createCollection
      - dbHash
      - dbStats
      - dropCollection
      - find
      - killCursors
      - listCollections
      - listIndexes
      - planCacheRead
  
  - All non-system collections (i.e. database resource):
      - bypassDocumentValidation
      - collMod
      - collStats
      - compact
      - convertToCapped
      - createCollection
      - createIndex
      - dbStats
      - dropCollection
      - dropDatabase
      - dropIndex
      - enableProfiler
      - listCollections
      - listIndexes
      - planCacheIndexFilter
      - planCacheRead
      - planCacheWrite
      - reIndex
      - renameCollectionSameDB
      - storageDetails
      - validate
   ```
- **dbOwner role**:
   - The database owner can perform any adminstrative action on the database
   - This role combines the privileges granted by  the **readWrite**, **dbAdmin** and **dbUser** roles.


**Note**
- in most cases you create user using admin database.
- we can add roles to user using the foloowing
   ```
   db.grantRolesToUser( "dba",  [ { db: "playground", role: "dbOwner"  } ] )
   ```
- to Show role privileges:
   ```
   db.runCommand( { rolesInfo: { role: "dbOwner", db: "playground" }, showPrivileges: true} )
   ```



### Server Tools Overview  <a id="m103-server-tools"></a>

List mongodb binaries: this will list all tools installed with mongodb
```
find /usr/bin/ -name "mongo*"
```

- Use **mongostat** to get stats on a running mongod process:
   ```
   mongostat --help
   mongostat --port 30000
   ```
- Use **mongodump** to get a BSON dump of a MongoDB collection:
   ```
   mongodump --help
   mongodump --port 30000 --db applicationData --collection products
   ls dump/applicationData/
   cat dump/applicationData/products.metadata.json
   ```
- Use mongorestore to restore a MongoDB collection from a BSON dump:
   ```
   mongorestore --drop --port 30000 dump/
   ```
- Use mongoexport to export a MongoDB collection to JSON or CSV (or stdout!):
   ```
   mongoexport --help
   mongoexport --port 30000 --db applicationData --collection products
   mongoexport --port 30000 --db applicationData --collection products -o products.json
   ```

- Tail the exported JSON file:
   ```
   tail products.json
   ```

- Use mongoimport to create a MongoDB collection from a JSON or CSV file:

   ```
   mongoimport --port 30000 products.json
   ```

**Notes**
- Mongodump can create a data file  and a metadata file, but mongoexport just create a data file only.
- By default, mongoexport send the output to standard output, but mongodump write to a file
- mongoexport is slower because its convert every file to json before export






## Chapter 2: Replication  <a id="m103-ch2"></a>

### What is Replication?  <a id="m103-what-is-replication"></a>
Replication is the concept of manitaining multiple copies of your data.
This because you never assume that all your servers will be all over available.
To make sure at anytime any server is down you can still access your data **Availability**.

### MongoDB Replica Set  <a id="m103-mongo-rs"></a>
- Replicaset is a group of mongod nodes that work on the same data
- consists of one primary node that handles the data and secondary nodes that sync up with the primary
- if the primary fails, a secondary node takes it's place in a process called **failover** where nodes vote for which node will become the primary in a process called **election**
- Default Replication protocol ----> **pv1** which is based on **raft** protocol
- Read more about the [Simple Raft Protocol](http://thesecretlivesofdata.com/raft/) and the [Raft Consensus Algorithm](https://raft.github.io/).
- Operation log **oplog** is statement based log for each node in the Replicaset that keeps track for all write operations.
- every time a write is successfully applied to the primary node will get recorded in the oplog
- Arbiter member in a Replicaset doesn't hold data, cann't be primary and is used only as a tie-breaker in leader-primary node election
- odd number of nodes is prefered for election
- if you used even number make sure the magority are available as you will need to have at least 3 nodes available
- any failer or vote happen is add as s topology change in the replica set configuration which is defined in one node and shared between all the nodes
- the election will not happen if the majority isn't available for example the majority of 4 is 3 so if 2 nodes down the election won't happen
- Avoid using Arbiter
- We can defin a **hidden** node to provide a read only node or have a copy off the data hidden of the application.
- We can set a node as **delayed** for a specific time to work as a backup
- Replicaset can have up to 50 member
- only max of 7 nodes can be voting to minimize election time



### Setting Up a Replica Set  <a id="m103-setting-rs"></a>
We will independtly launching 3 mongod process and try to connect them to replicate data for us.

1. adding keyFile to security section in config file so all members can authentiacte each other using this keyFile
2. This is adition to client auth
3. Create keyFile using openssl   ```openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile```
4. change permission of file to read permission ```chmod 600 /var/mongodb/pki/103-keyfile```
5. add replication.replSetName to config file

   ```
   storage:
      dbPath: /var/mongodb/db/node1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27011
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node1/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```

6. Create the dbPath folder ```mkdir -p  /var/mongodb/db/node1```
7. Start mongod using this config file ```mongod -f node1.conf```
8. create other 2 nodes with the same replSetName and keyfile just edit the dbPath, logPath and port by coping the configurations file
   ```
   cp  node1.conf node2.conf
   cp  node1.conf node3.conf
   ```
   ```
   storage:
      dbPath: /var/mongodb/db/node2
   net:
      bindIp: 192.168.103.100,localhost
      port: 27012
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node2/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
   ```
   storage:
      dbPath: /var/mongodb/db/node3
   net:
      bindIp: 192.168.103.100,localhost
      port: 27013
   security:
      authorization: enabled
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node3/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
9. Create the dbPath folder for node2 ```mkdir -p  /var/mongodb/db/node2```
10. Start mongod using this config file of node2 ```mongod -f node2.conf```
11. Create the dbPath folder for node3 ```mkdir -p  /var/mongodb/db/node3```
12. Start mongod using this config file of node3 ```mongod -f node3.conf```
13. Connecting to node1: ```mongo --port 27011```
14. Initiating the replica set on one like node1 ``` rs.initiate()```
15. Creating a user: to use to connect to the rest node using it
      ```
      use admin
      db.createUser({
         user: "m103-admin",
         pwd: "m103-pass",
         roles: [
            {role: "root", db: "admin"}
         ]
      })
      ```
16. Exiting out of the Mongo shell and connecting to the entire replica set:
      ```
      exit
      mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin"
      -p "m103-pass" --authenticationDatabase "admin"
      ```
17. Getting replica set status: ```rs.status()```
18. Adding other members to replica set:
      ```
      rs.add("m103:27012")
      rs.add("m103:27013")
      ```
19. Getting an overview of the replica set topology: ```rs.isMaster()```
20. Stepping down the current primary: ```rs.stepDown()```
21. Checking replica set overview after election: ```rs.isMaster()```


### Replication Configuration Document  <a id="m103-repl-conf"></a>
- BSON doc that holds the configuration of the replicaset and is shared across all nodes
- JSON Object that define the configuration options of our replica set.
- Can be configured manually from the shell
- There are set of mongo shell  replication helper methods that make it easier to manage :
   ```
   rs.add
   rs.initiate
   res.remove
   rs.reconfig
   rs.config || rs.conf
   ```


```
{
    _id: string,  // the name of the replicaset
    version: int,  // gets incremented every time the configuration changes
    members: [
        {
            _id: string, // can't be changed once set
            host: string,
            arbiterOnly: bool,
            hidden: bool,  // Not visible to app handles specific operations
            priority: number,  // range (0 - 1000) higher priority members tend to be elected more often, 
                               // changing the priority triggers an election
                               // priority 0 is excluded from being a primary  (arbiteronly and hidden should be 0)
            slaveDelay: int  // the riplication delay of the node in seconds
                             // setting this setting implies that the node will be hidden and the priority is 0
        },
        ...
    ]
}
```


### Replication Commands  <a id="m103-repl-commands"></a>
**rs.status()**:
  - reports health of the nodes
  - uses data from heartbeats so it can be seconds out of date
  - optime: the last time the node did an operation from the oplog

**rs.isMaster()**:
  - describes the role of the node

**db.serverStatus()['repl']**:
  - is a section of the server status full output
  - similar to isMaster
  - rbid field doesn't appear on isMaster and it shows the number of times a rollback occured on the node

**rs.printReplicationInfo()**:
  - returns the oplog data relative to the current node
  - contains timestamp to the first and last oplog event in the node



### Local DB  <a id="m103-local-db"></a>

- Display all databases (by default, only admin and local):
   ```
   mongo
   show dbs
   ```
- Display collections from the local database (this displays more collections from a replica set than from a standalone node):
   ```
   use local
   show collections
   ```
- has startup_log collection only in standalone node
- has several collections on replicset:
   >me
   oplog.rs
   replset.electoin
   replset.minvali
   startup_log
   system.replset
   system.rollback.id
   
   **oplog.rs**: 
   - is the center point of the replication mechanism 
   - It keeps track of all statements that is being replicated
   - is a capped collection (has max size)
   - if you run ```var stats = db.oplog.rs.stats()``` then:
    - ```stats.capped --> true``` if a capped collection
    - ```stats.size``` --> Get current size of the oplog
    - ```stats.maxSize ```--> Get size limit of the oplog 
   - by default it has 5% of the free disk space but can be cofigured through oplogSizeMB option under replication in the config file
   - is created after creating replset
   - once the limit is reached, the early operations are overriden
   - the replication window: 
      - the time it takes to fill the oplog completely and start overriding old statements
      - important to determine the time a node can afforded to be down without requireing human intervention to help it recover
      - it's inversly proportional to the system load
   - one operation can reslut in many entries in the oplog such as updateMany
   - data written in local dbs won't be replicated



### Reconfiguring a Running Replica Set  <a id="m103-re-conf-rs"></a>

Let's assume we have a replica set of 3 nodes and we need to add 2 more 1 as Arbiter and 1 as secodary node.
- **node4.conf**:
   ```
   storage:
      dbPath: /var/mongodb/db/node4
   net:
      bindIp: 192.168.103.100,localhost
      port: 27014
   systemLog:
      destination: file
      path: /var/mongodb/db/node4/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```
- **arbiter.conf**:
   ```
   storage:
      dbPath: /var/mongodb/db/arbiter
   net:
      bindIp: 192.168.103.100,localhost
      port: 28000
   systemLog:
      destination: file
      path: /var/mongodb/db/arbiter/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-example
   ```

- Starting up mongod processes for our fourth node and arbiter:
   ```
   mongod -f node4.conf
   mongod -f arbiter.conf
   ```
- From the Mongo shell of the replica set, adding the new secondary and the new arbiter:
   ```
   rs.add("m103:27014")
   rs.addArb("m103:28000")
   ```
- Checking replica set makeup after adding two new nodes:
   ```
   rs.isMaster()
   ```

- Removing the arbiter from our replica set:
   ```
   rs.remove("m103:28000")
   ```
- Assigning the current configuration to a shell variable we can edit, in order to reconfigure the replica set:
   ```
   cfg = rs.conf()
   ```

- Editing our new variable cfg to change topology - specifically, by modifying cfg.members:

   ```
   cfg.members[3].votes = 0
   cfg.members[3].hidden = true
   cfg.members[3].priority = 0
   ```
- Updating our replica set to use the new configuration cfg:
   ```
   rs.reconfig(cfg)
   ```



### Reads and Writes on a Replica Set  <a id="m103-rs-read-write"></a>
- By default read and write operations aren't allowed on secondary nodes
- to enable reading on a secondary node: ```rs.slaveOk()```
- writing is forbidden to replica sets
- if no nodes are secondary the primary will be secondary and we will not be able to write to the replica set


### Failover and Elections  <a id="m103-failover-election"></a>
suppose the following scenario: 
  - replicaset with 3 nodes 1p and 2s
  - to do a rolling upgrade we do:
      1. stop one of the secondary and bring it up with  the new version
      2. do the same to the other secondary node
      3. perform an election by running rs.stepDown() -> this will lead to that the primary node becomes a secondary
      4. do the step 1 to the secondary that was a primary


**Election**: happens when
  - there is a change in topology
  - reconfiguring a replicaset
  - the primary node becomes unavailable    (must elect a new node to be primary)
  - using rs.stepDown()    (must elect a new node to be primary)



  **Election candidates**:
  - if all nodes has the same priority then the one with the latest data will vote for itself and ask other nodes for support

  - if two nodes run for election simaltaneously in case of odd number of nodes ---> the odd node will decide the winner
   - in case of even number of nodes ---> a tie can happen then election will be repeated
  
  - a node with priority of 1 or higher can be elected
  - a node with priority of 0 can't run election and can't be primary, but can vote
  - a higher priority has a higher chance of being a primary
  - **Note** when running ```rs.isMaster()``` nodes that can't be elected show up as passives
  - if a primary can't reach any voting secondary, it wil automatically step down and be a secondary and if no primary in the replset then it won't be reachable


### Write Concerns  <a id="m103-write-concerns"></a>
- write concerns are acknowledgement mechanism to increase durability
- for a write to be durable, majority of the nodes must acknowledge the success of the write
- **Levels**:
  1. 0          ```no wait for acknowledgement means the write might successed or failed```
  2. 1          ```(Default) wait for acknowledgment from primary only```
  3. greater than 1        ```waint for primary and one or more secondary members```
  4. majority   ```wait for the majority of the replicaset```

- **Options** :
  1. ```wtimeout```: (int) the time to wait for write concern before marking the operation as failed
  2. ```j```: (bool) node acknowledge the write and commit it in the journal files before returning an acknowledge. if j is false then the node will report success when the write stored in memory and before waiting for journaling

- **Write Concern Commands**:
   - insert
   - update 
   - delete 
   - findandmodify
- write concern is 1 by default
```
db.collection.insert(
   { }, 
   { writeConcern: { w: "majority", wtimeout: 60 } }
)
```



### Read Concerns  <a id="m103-read-concerns"></a>
- returns the data if it has been saved to a number of nodes
**Levels**: 
  - ```local``` --> (default) most recent data to the cluster on primary only and doesn't guarantee that it is durable
  - ```available``` --> same as local and default against secondary differs in sharded clusters
  - ```majority```  --> returns if in a majority of the nodes not the latest note supported in MMApv1 storage engine
  - ```linearizable``` -->  like majority and read your only write functionality

**Notes**:
- local & available  ->> fast and latest but not safe
- majority  ->> fast and safe not latest
- linearizable  ->> safe and latest - not fast - single doc reads only
- for secondary reads ->> local & available is fast only but not always latest


**READ preference**
- Route read operations to secondary nodes
- is a driver-side setting
modes: 
  - ```primary``` (default) only to primary
  - ```primaryPreferred```  (primary and if not available the secondary)
  - ```secondary```   routes to only secondary
  - ```secondaryPreferred```    if no secondary then primary
  - ```nearest```  the least network latency to the host







## Chapter 3: Sharding  <a id="m103-ch3"></a>
### What is Sharding?  <a id="m103-what-is-sh"></a>
- higher cost of vertical scaling
- scaling horizontaly by divid the data into multiple instances
- impact on operational tasks like (backup)  ---> backing up several 2 TB hard disks in parallel is faster than 20 TB single hard disk
- single threaded operations benefit from distributed environment ex: aggregation framework
- geographically distributed datasets


### Sharding Architecture  <a id="m103-sh-arch"></a>
- we setting up a router process that accept queries from clients this router process called **Mongos**
- We can have any number of Mongos processes 
- Mongos using Metadata stored in config servers that has info about where each piece is stored.
- We need to make sure that the Metadata is highly availabe using replication 
- We deploy a Config Server Replica Set.
- **Primary Shard**
   - every database has a primary shard
   - that holds non-sharded collections
   - merge aggregation data from different shards




### Setting Up a Sharded Cluster  <a id="m103-setting-sh-cluster"></a>
The minumum requirements to have a sharded cluster is to have (mongos process, one shard, CSRS)



1. deploy config server replicaset with mongod config file csrs_1.conf, csrs_2.conf, csrs_3.conf:
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26001
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs1.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs1
   ```
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26002
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs2.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs2
   ```
   ```
   sharding:
      clusterRole: configsvr
   replication:
      replSetName: m103-csrs
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26003
   systemLog:
      destination: file
      path: /var/mongodb/db/csrs3.log
      logAppend: true
   processManagement:
      fork: true
   storage:
      dbPath: /var/mongodb/db/csrs3
   ```
2. Starting the three config servers:
   ```
   mongod -f csrs_1.conf
   mongod -f csrs_2.conf
   mongod -f csrs_3.conf
   ```
3. Connect to one of the config servers:```mongo --port 26001```
4. Initiating the CSRS: ```rs.initiate()```
5. Creating super user on CSRS:
   ```
   use admin
   db.createUser({
      user: "m103-admin",
      pwd: "m103-pass",
      roles: [
         {role: "root", db: "admin"}
      ]
   })
   ```
6. Authenticating as the super user: ```db.auth("m103-admin", "m103-pass")```
7. Add the second and third node to the CSRS:
   ```
   rs.add("192.168.103.100:26002")
   rs.add("192.168.103.100:26003")
   ```
8. prepare mongos config file:
   ```
   sharding:
      configDB: m103-csrs/192.168.103.100:26001,192.168.103.100:26002,192.168.103.100:26003
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   net:
      bindIp: localhost,192.168.103.100
      port: 26000
   systemLog:
      destination: file
      path: /var/mongodb/db/mongos.log
      logAppend: true
   processManagement:
      fork: true
   ```
9. Start the mongos server: ```mongos -f mongos.conf```
10. Connect to mongos:
   ```
   mongo --port 26000 --username m103-admin --password m103-pass --authenticationDatabase admin
   ```
11. Check sharding status: ```sh.status()```
12. Updated configuration for node1.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node1
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27011
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node1/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
13. Updated configuration for node2.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node2
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27012
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node2/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
14. Updated configuration for node3.conf:
   ```
   sharding:
      clusterRole: shardsvr
   storage:
      dbPath: /var/mongodb/db/node3
      wiredTiger:
         engineConfig:
            cacheSizeGB: .1
   net:
      bindIp: 192.168.103.100,localhost
      port: 27013
   security:
      keyFile: /var/mongodb/pki/m103-keyfile
   systemLog:
      destination: file
      path: /var/mongodb/db/node3/mongod.log
      logAppend: true
   processManagement:
      fork: true
   replication:
      replSetName: m103-repl
   ```
15. Connecting directly to secondary node (note that if an election has taken place in your replica set, the specified node may have become primary):
   ```
   mongo --port 27012 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
   ```
16. Shutting down node:
   ```
   use admin
   db.shutdownServer()
   ```
17. Restarting node with new configuration:
   ```
   mongod -f node2.conf
   ```
18. Stepping down current primary:
   ```rs.stepDown()```
19. Adding new shard to cluster from mongos:
   ```sh.addShard("m103-repl/192.168.103.100:27012")```




### Config DB  <a id="m103-conf-db"></a>
Config DB maintained and used internally by mongodb, so generally you should never write any data to it. However it's got some useful information so we are going to read from it.

If you'd like to explore the collections on the config database, you can find the instructions here:
   - Switch to config DB:
      ```
      use config
      ```
   - Show config DB collections
      ```
      show collections
      ---------------------------
      actionlog
      changelog
      chunk
      collections
      databases
      lockpings
      locks
      migrations
      mongos
      shards
      tags
      transactions
      version
      ```
   - Query config.databases: will return each database in our cluster as one document
      ```
      db.databases.find().pretty()
      -------------------------
      {"_id": "m103", "primary": "m103-repl", "partitioned": true}
      ```
   - Query config.collections: gives us info on collections that have been sharded and the shard key used
      ```
      db.collections.find().pretty()
      -----------------------------
      {
         "_id": "config.system.sessions",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": ISODate("------------------------"),
         "dropped": false,
         "key": {
            "_id": 1
         },
         "unique": false,
         "uuid": UUID("f5f4ff-gfggfdgdg45-4455-d4f544f5d5df4")
      },
      {
         "_id": "m103.products",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": ISODate("------------------------"),
         "dropped": false,
         "key": {
            "salePrice": 1
         },
         "unique": false,
         "uuid": UUID("f5f4ff-gfggfdgdg45-4455-d4f544f5d5df4")
      }
      ```
   - Query config.shards: This tell us about the shards in our cluster
      ```
      db.shards.find().pretty()
      -------------------------
      {
         "_id": "m103-repl",
         "host": "m103-repl/192.168.103.100:27011,192.168.103.100:27012,192.168.103.100:27013",
         "state": 1
      },
      {
         "_id": "m103-shard-2",
         "host": "m103-shard-2/192.168.103.100:27014,192.168.103.100:27015,192.168.103.100:27016",
         "state": 1
      }
      ```
   - Query config.chunks: Each chunk for every collection in this database is returned as one document. The enclusive and exclusive maximum define the chunk range of the shard key value. That means that any document in the associated collection who's shard key value falss into this chunks range will end up in this chunk, and this chunk only
      ```
      db.chunks.find().pretty()
      -------------------------
      {
         "_id": "m103.products-salesPrice_MinKey",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,0),
         "ns": "m103.productions",
         "min": {
            "salePrice": {"$minKey": 1}
         },
         "max": {
            "salePrice": 14.99
         },
         "shard": "m103-shard-2"
      },
      {
         "_id": "m103.products-salesPrice_14.99",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,1),
         "ns": "m103.productions",
         "min": {
            "salePrice": 14.99
         },
         "max": {
            "salePrice": 33.99
         },
         "shard": "m103-shard-2"
      },
      {
         "_id": "m103.products-salesPrice_33.99",
         "lastmodEpoch": ObjectId("sdfdgfghh556546fgh6fdgh"),
         "lastmod": Timestamp(2,1),
         "ns": "m103.productions",
         "min": {
            "salePrice": 33.99
         },
         "max": {
            "salePrice": {"$maxKey": 1}
         },
         "shard": "m103-shard-2"
      }
      ```
   - Query config.mongos: holds data about the mongos processes connected to the cluster
      ```
      db.mongos.find().pretty()
      --------------------------
      {
         "_id": "m103:26000",
         "mongoVersion": "3.6.2-rc0",
         "ping": ISODate("------------------------"),
         "up": NumberLong(3892),
         "waiting": true
      }
      ```



### Shard Keys  <a id="m103-shard-keys"></a>
This is the indexed field(s) used to partition collection on shards in our cluster.

**How the shard key is used to distribute your data?**

Consider a collection with some number of documents in them.
MongoDB uses the shard key to divide up these documents into logical groupings that MongoDB then distributes across our sharded cluster.

MongoDB he refers to these groupings as chunks.
The value of the field or fields we choose as our shard key help to define the inclusive lower bound, and the exclusive upper bound of each chunk.

Because the shard key is used to define chunk boundaries, it also defines which chunk a given document is going to belong to.


Every time you write a new document to the collection, the MongoS router checks which shard contains the appropriate chunk for that documents key value, and routes the document to that shard only.




- The shard key must be present on every document in the collection, and every new inserted document.
- The shard key Fields must be indexed and indexes must exists first before you can select the indexed fields for your shard key.
- Shard Keys are immutable:
   - cannot be changed after sharding
   - cannot specify another key or update a value for that key in any document
- Shard Keys are permanent:
   - cannot unshard a collection unless dropped and restored again.

**How To Shard**:
1. Use ```sh.enableSharding("<database>")``` to enable sharding for the specified database.
2. Use ```db.collection.createIndex()``` to create Index on shard key fields
3. Use ```sh.shardCollection("database.collection", {shard key: 1}) to shard the collection





### Picking a Good Shard Key:  <a id="m103-good-shard-key"></a>

**What makes a good Shard Key?**
The goal is a shard key whose values provides good write distribution.
- **Cardinality**: 
   - Higher Cardinality =  many possible unique shard key values
   - The higher the better
- **Frequency**:
   - High Frequency =  low repetition of a given unique shard key value.
   - If we have 90% of document have the same the shard key value that means that they well be distributed to the same shard which is bad.
   - The lower the better.
- **Type of Change**:
   - Avoid shard keys that change monotonically
   - Like a counter, timestamp, objectId ... 
   - This will lead to all documents will be in the shard with max range
   - Should be avoided



**Read Isolation**:
- Which shard has the data that meets our query parameter?
- MongoDb directs tatgeted queries to a single shard
- Without the shard key, MongoDB has to ask every shard
- The key should be used frequently in query operations to direct the router to it (faster reading)




### Hashed shard key:  <a id="m103-hashed-sh-key"></a>
That is a shard key where the underlying index is hashed.
Like hash tables, the key is hashed and the hash is used to distribute the data
**When to Use**:
- monotonic changed keys

**Drawbacks**: 
- Queries on ranges of shard key values are more likely to be scatter-gathered
- Cannot support geographically isolated read operations using zone sharding
- Hashed index must be on a single non-array field
- Hashed index don't support fast sorting


**Sharding using a Hashed Shard Key**:
1. Use ```sh.enableSharding("<database>")``` to rnable sharding for the specified database
2. Use ```db.collection.createIndex({"field": "hashed"})``` to create the index for your shard key fields
3. Use ```sh.shardCollection("<db>.<collection>", {field: "hashed"})``` to shard the collection



### Chunks  <a id="m103-chunks"></a>
Default size 64MB
Min size 1MB
Max size 1024MB
Can be changed at runtime using:
- using config db
- inserting into settings collection: ``` db.settings.save({_id: "chunksize", value: <inMB> (ex: 2)})```
- changes will be applied when importing or saving new data


Shard key value frequency affects the number of chunks:
**Jumbo Chunks**:
   - Larger than the defined chunksize
   - Can't be moved or split
   - Once marked as jumbo the balancer skips these cunks and avoid trying to move them
   - In some cases these will not be able to split
   - this results from poor choice of key



### Balancing  <a id="m103-balancing"></a>
- MongoDB balancer identifies the shard with too many chunks and distribute them to other shards.
- Balancer runs on primary member of config servers starting 3.2 before it ran in mongos.
- When it detects a migration threshold, it starts a balancer round
- The balancer can migrate chunks in parallel but a shard can only participate in a single migration at a time
- Number of chunks to migrate in a round = floor(n / 2)  n = #shards
- Then a nother round takes place until the shards are balaced
- Balancer affects performance


**Balancer Management Methods**:
Start / Stop the Balancer
- ```sh.startBalacer(timeout, interval)``` timeout-> how mins to wait to start or stop the balancer
- ```sh.stopBalancer(timeout, interval)``` interval-> how mins the client wait to check the status of the balancer again
- ```sh.setBalacerState(bool)``` true, flase on/off


> You can read more about scheduling the balancer on the [MongoDB Sharding docs](https://docs.mongodb.com/manual/tutorial/manage-sharded-cluster-balancer/#sharding-schedule-balancing-window).




### Queries in a Sharded Cluster  <a id="m103-query-sh"></a>
- The mongos is responsible for routing queries
- If the shard key is in the query predicate, then it will target a specific shards (very efficient) otherwise it performs a scatter gather on list of shards (all), opens a cursor in each one, performs the query then merges the result from each shard.

**Sort, Limit and Skip in sharded clusters**:
- **sort( )**
   - The mongos pushes the sort to each shard and merge-sorts the results

- **limit( )**
   - The mongos passes the limit to each targeted shard, then re-applies the limitto the merged set of results

- **skip( )**
   - The mongos performs the skip against the merged set of results


> You can read more about routing Aggregation queries in a sharded cluster on the [MongoDB sharding docs](https://www.mongodb.com/docs/manual/core/aggregation-pipeline-sharded-collections/).



### Targeted Queries vs Scatter Gather  <a id="m103-target-queries"></a>
Each mongos keeps local cashed map of the shard chunk relationships that exists on the config server
Shard | Data
--- | --- |
1 | minKey -> 10000000
2 | 10000000 -> 20000000
3 | 30000000 -> maxKey

So when the mongos receives a query whose predicate includes the shard key, the mongos can look at the table and know exactly which shards to direct that query to.

The mongos opens a cursor against only those shards that can satisfy the query predicate.
Because the mongos is targeting the query to a subset of shards in the cluster, these targeted queries are generally faster than having to check in with every shard in the cluster.

If, for example, the mongos can satisfy the entire query by targeting a single shard, then the mongos can even bypass the merge stage and just return the results. This is particularly fast.


When the query predicate does not include the shard key, then the mongos cannot derive exactly which shards satisfythe query.
These scatter gather queries must necessarily ping and wait for the reply of every shard in the cluster, regardless if they have something to contribute towards the execution of the query or not.

Depending on the number of shards in your cluster, the amount of network latency between shards and mongos and a number of other factors, these queries can be slow.

Compound indexes can be used as shard keys in this case, using any index prefix in the pridicate will result in a target query for example:
   ```
   shard key: {a: 1, b: 1, c: 1}
   
   Targeted Queries:
   db.col.find({a: })
   db.col.find({a: , b: })
   db.col.find({a: , b: , c: })

   Scatter-gather Queries:
   db.col.find({b: })
   db.col.find({c: })
   ```

**Note** Using ```db.col.find().explain()```  shows us detailed info about how we got the results







# M201: MongoDB Performance <a id="m201"></a>
Learn how to optimize the performance of your MongoDB deployment.

## Chapter 1: Introduction <a id="m201-ch1"></a>

### Hardware Considerations & Configurations <a id="m201-hardware"></a>
MongoDB is a High Performance Database and to support your requirements it will require good hardware.
### Let's discuss how mongodb uses hardware resources:
#### 1. Memory <a id="m201-hardware-memor"></a>
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

#### 2. CPU <a id="m201-hardware-cpu"></a>
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
#### 3. I/O <a id="m201-hardware-io"></a>
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






## Chapter 2: MongoDB Indexes <a id="m201-ch2"></a>

### Introduction to Indexes <a id="m201-indexes-intro"></a>
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



### How Data is Stored on Disk <a id="m201-data-on-disk"></a>
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


### Single Field Indexes <a id="m201-single-field-index"></a>
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


### Understanding Explain <a id="m201-explain"></a>
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



### Sorting with Indexes <a id="m201-index-sorting"></a>
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





### Querying on Compound Indexes <a id="m201-compound-index"></a>
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



### When you can sort with compound Indexes ? <a id="m201-sorting-compound-index"></a>
- Sorting can utilize index if the filter fields combined with sort fields gives index prefix
    - ```find({a: "", b: ""}).sort({c: 1})```  will work for index ```{a: 1, b: 1, c: 1}```
- Using single prefix ---> both directions will utilize the index
- else, won't use index unless all is in the same direction as the index or all are reversed


> You can learn more about when you can sort with indexes by visiting the [Use Indexes to Sort Query Results](https://www.mongodb.com/docs/manual/tutorial/sort-results-with-indexes/?jmp=university) section of the MongoDB Manual.




### Multikey Indexes <a id="m201-multikey-index"></a>
- Indexing on a field that is an array is a Multikey Index

- for each entry in the array, the server will create a separate index key.
- it can also work in emedded docs inside the array
	- arr: [{a: "", b: ""}, ...] ---> creatIndex({arr.a: 1})
- For each indexed document, we can have at most one index field whose value is an array
- only one array index field is allowed per compound index
- Multikey indexes don't support covered queries

> You can learn more about multikey indexes by visiting the [Multikey Indexes](https://www.mongodb.com/docs/manual/core/index-multikey/?jmp=university) section of the MongoDB Manual.


### Partial Indexes <a id="m201-partial-index"></a>
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




### Text Indexes <a id="m201-text-index"></a>
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



### Collations <a id="m201-collations"></a>

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





### Wildcard Index Type <a id="m201-wildcard-index"></a>
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



## Chapter 3: Index Operations <a id="m201-ch3"></a>

### Building Indexes <a id="m201-building-indexes"></a>
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



### Query Plans <a id="m201-query-plans"></a>
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



### Forcing Indexes with Hint <a id="m201-indexes-hint"></a>
- Overriding default mongodb index selection with hint()
- Sometimes query optimizer uses another index that we want it to use
- ```db.collection.find().hint({ a: 1, b: 1 })``` index shape
- ```db.collection.find().hint("a_b_1")``` index name
- Use it with caution


### Resource Allocation for Indexes <a id="m201-resource-allocation"></a>
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



### Basic Benchmarking <a id="m201-benchmarking"></a>
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






## Chapter 4: CRUD Optimization <a id="m201-ch4"></a>

### Optimizing your CRUD Operations <a id="m201-optimizing-crud"></a>
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



### Covered Queries <a id="m201-coverd-queries"></a>
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




### Regex Performance <a id="m201-regex-performance"></a>
- Creating index on fields that we want to run regex on increases performance but we still have to run the regex against all the keys in the index
- to address this issue try to use /^regex/ to only examine a subset of the keys
- if we use it like this /^.regex/, then it has no effect



### Aggregation Performance <a id="m201-agg-performance"></a>
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







## Chapter 5: Performance on Clusters <a id="m201-ch5"></a>

### Performance Considerations in Distributed Systems <a id="m201-dist-sys-performance"></a>
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





### Increasing Write Performance with Sharding <a id="m201-sharding"></a>
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




### Reading from Secondaries <a id="m201-reading-secondaries"></a>
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




### Replica Sets with Differing Indexes <a id="m201-replset"></a>
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
	


### Aggregation Pipeline on a Sharded Cluster <a id="m201-agg-sharded-cluster"></a>
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