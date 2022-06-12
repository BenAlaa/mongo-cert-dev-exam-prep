# Chapter 1: What is MongoDB?
-----------------------------
- NoSQL Document DB
- DB ...> Collections ...> Documents ...> Fields and values.

- Replica Set - a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.

- Instance - a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

- Cluster - group of servers that store your data.

# Chapter 2: Importing, Exporting and Querying Data
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





# Chapter 3: Creating and Manipulating Documents
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



# Chapter 4: Advanced CRUD Operations
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
