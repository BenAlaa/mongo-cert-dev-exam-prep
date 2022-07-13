![Certified Developer](./Assets/university-badges-dev-lockup.png)
![MDB University](./Assets/mdb-university.png)
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






---


# M001 - MongoDB Basics <a name="m001"></a>
Learn the fundamentals of MongoDB.
## Chapter 1: What is MongoDB? <a id="m001-ch1"></a>
-----------------------------
- NoSQL Document DB
- DB ...> Collections ...> Documents ...> Fields and values.

- Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.

- Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

- Cluster - group of servers that store your data.

## Chapter 2: Importing, Exporting and Querying Data <a id="m001-ch2"></a>
---

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
---
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
---
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
---

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