# M320: Data Modeling
Learn everything you need to know about data modeling for MongoDB.

## Chapter 1: Introduction to Data Modeling
---
### Data Modeling in MongoDB
- One of the most misconception about mongodb is that modeling is Schemaless means that it doesn't really matter which field documents have or how different the documents can be from one another, or how many collection you might have  per database.
- Even that mongodb give you this flexibility, this is not practical in reality.
- MongoDB has very flexible data model.
- But most importantly all data as some sort of structure and there for a Schema.
- MongoDB just happens to make it easier for you to deal with that later rather than sooner.
- Before building ERD or UML it tends to be preferable to start building your application and finding out from that particular experience what the data structure should look like.
- However if you do know:
    - Usage pattern.
    - How your data is accessed.
    - Which queries are critical to your application,
    - Ratios between reads and writes
you will be able to extract a good model, even before rewriting the full application to make it scale with mongodb.
- Being flexible means that your application  changes. and it's not unreasonable to think that that will not be the case.
- With mongo you will be able to accomodate those changes without experiencing a painful migration process like in traditional relational databases.
- When have a good idea about the structure of your documents you will be able to enforce those rules in MongoDB by using **Document Validation** 
- Another misconception is that all information regardless of how data should be manipilated can be stored in one single document, but the reality is that this is not the way applications in general use data.
- Keep the amount of the iformation stored per individual document to the data that your application uses and having different models to deal with historical data or other types of data that are not always accessed
- We can berform data joining process using $lookup


### The Document Model in MongoDB
- Database -> Collection -> Document
- Document stored in BSON
- Instead of but your related data in multible table you can put the related data nestead in the same document and put it all down in a single query
- You can have multiple version of your document schema and they can be coexists in the same collection

> You can read more about [Document Structure](https://www.mongodb.com/docs/manual/core/data-modeling-introduction/#document-structure) and [BSON Data Types](https://www.mongodb.com/docs/upcoming/reference/bson-types/)



### Constraints in Data Modeling
1. Hardware:
	- RAM
	- SSD, HDD
2. Data:
	- Size
	- Security, sovereignty
3. Application:
	- Network latency
4. DB Server:
	- MongoDB has limitaion on doc size 16MB

- **Working Set**: Is the data that the Application uses in normal operations

- **Tips**:
	- Keep the frequently used documents in RAM
	- Keep the indexes in RAM
	- Prefer SSD to HDD
	- Infrequently accessed data in hdd



> To know more about Transactions with MongoDB, please consult the MongoDB [Documentation on Transactions](https://www.mongodb.com/docs/manual/core/transactions/) and some [videos explaining their implementation](https://www.mongodb.com/transactions) .



### The Data Modeling Methodology
![The Data Modeling Methodology](./Assets/M320/methodology.png)
**Phases**:
1. Describing workloads:
    - Requirement document
    - Business domain analysis
    - Production logs and stats if migrating existing db

2. Indentifying relationships between entities:
    - pieces of info that can be grouped together (entity)
    - Indentifying the relationships between entities
    - decide either keeping as embedded doc or in a new collection

3. Applying design patterns to address performance requirements


### Model for Simplicity or Performance
The main tradoff you will face is Simplicity vs Performance or try to find the balance between them

- **Modeling for Simplicity**:
	- limitied expectations
	- low resources requiremens cpu ram disk
	- fewer collections and embedding documents
	- less disk access
![Modeling for Simplicity](./Assets/M320/modeling_for_simplicity.png)

- **Modeling for Performance**:
	- need more resources
	- Sharding
	- fast read, writes
	- larger teams needed
	- must adhere to the phases of methodology
![Modeling for Performance](./Assets/M320/modeling_for_performance.png)

- **Modeling for a Mix of Simplicity and Performance**
![Modeling for a Mix of Simplicity and Performance](./Assets/M320/modeling_for_a_mix.png)

- **Summary of Modeling Approaches**:
![Summary of Modeling Approaches](./Assets/M320/flexible_methodology.png)



## Chapter 2: Relationships
---
### Introduction to Relationships
- Even MongoDB is classified as non-relationa database but the pieces of data inside will have relations.
- Mainly these relations is done by embeding or referencing.
- The relationships represent all the entities are related to each other.
- For example:
	- customer and customer_id is One-to-One relationship
	- Customer and invoices is One-to-Many relationship
	- Invoices and Products is Many-to-Many relationship
	
