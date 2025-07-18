<!-- markdownlint-disable MD012 MD026 MD001 MD022 MD032 MD029 MD019 MD034 MD031 MD047 MD040 MD009 MD058 MD024 MD036 MD003 MD025 MD028  -->

# Questions About MongoDB & Mongoose

## 🔹What is MongoDB, and How Does It Differ from Traditional SQL Databases?

MongoDB is a NoSQL database that uses a document-oriented data model, allowing for flexible and scalable data storage. Unlike traditional SQL databases, which use structured tables and predefined schemas, MongoDB stores data in JSON-like documents with dynamic schemas. This means that fields can vary between documents, making it easier to handle unstructured data and adapt to changing requirements.

Some key differences between MongoDB and traditional SQL databases include:

1. **Data Model**: MongoDB uses a document-based model, while SQL databases use a table-based model.
2. **Schema Flexibility**: MongoDB allows for dynamic schemas, whereas SQL databases require a fixed schema.
3. **Scalability**: MongoDB is designed for horizontal scalability, making it easier to distribute data across multiple servers.
4. **Query Language**: MongoDB uses a rich query language that supports ad-hoc queries, while SQL databases use structured query language (SQL) with predefined queries.

Overall, MongoDB is well-suited for applications that require high scalability, flexibility, and the ability to handle diverse data types.

#### 🔸 Key Differences Between MongoDB and SQL Databases
| Feature                | MongoDB                          | SQL Databases                    |
|------------------------|----------------------------------|----------------------------------|
| Data Model             | Document-based                    | Table-based                      |
| Schema Flexibility      | Dynamic schemas                   | Fixed schemas                    |
| Scalability            | Horizontal scaling                | Vertical scaling                  |
| Query Language         | Rich query language               | Structured Query Language (SQL)  |
| Transactions           | Multi-document transactions       | ACID transactions                |
| Relationships          | Embedded documents and references | Foreign keys and joins           |
| Indexing               | Supports various indexing types  | Supports B-tree indexing         |
| Use Cases              | Real-time analytics, IoT, etc.   | Traditional business applications |
| Performance            | Optimized for large datasets      | Optimized for complex queries    |
| Data Storage           | BSON (Binary JSON)                | Rows and columns in tables       |
| Data Retrieval         | Flexible queries with aggregation | Structured queries with joins    |
| Data Integrity         | Eventual consistency              | Strong consistency               |
| Data Types             | Supports various data types       | Limited to predefined types      |
| Deployment             | Cloud, on-premises, or hybrid    | Primarily on-premises            |
| Community Support      | Active open-source community      | Established vendor support       |
| Learning Curve         | Easier for developers familiar with JSON | Requires knowledge of SQL syntax |


#### ❓Collections And Databases In MongoDB
- **Database**: A container for collections, equivalent to a database in SQL.
- **Collection**: A group of documents, similar to tables in SQL, but schema-less.
> For example, a users collection can be part of the mydatabase database.

#### ❓MongoDB Ensure High Availability and Scalability
- MongoDB ensures high **availability** and **scalability** through its features like **replica sets** and sharding.
- **Replica sets** provide redundancy and failover capabilities by ensuring that data is always available.
- Sharding distributes data across multiple servers, enabling horizontal scalability to handle large volumes of data and high traffic loads.

#### ❓Replica Sets in MongoDB
- A replica set in MongoDB is a group of mongod instances that maintain the same data set.
- A replica set consists of a primary node and multiple secondary nodes.
- The primary node receives all write operations while secondary nodes replicate the primary's data and can serve read operations.
- If the primary node fails, an automatic election process selects a new primary to maintain high availability.

#### ❓Create a New Database and Collection in MongoDB

#### 🧱 1. Using MongoDB Shell (`mongosh`)
#### ✅ Step 1: Switch to (or create) a new database
```javascript
use myNewDatabase
```
> If `myNewDatabase` doesn't exist, MongoDB will create it when you insert the first document.
#### ✅ Step 2: Create a collection
```javascript
db.createCollection("myCollection")
```
> This creates an empty collection named myCollection in myNewDatabase.
#### ✅ Step 3: Insert a document (auto-creates collection if needed)
```javascript
db.myCollection.insertOne({ name: "John", age: 30 })
```

#### 💻 2. Using MongoDB with Node.js (Example)
```javascript
const { MongoClient } = require("mongodb");

async function run() {
  const uri = "mongodb://localhost:27017";
  const client = new MongoClient(uri);
  
  try {
    await client.connect();
    const db = client.db("myNewDatabase");           // Create/use database
    const collection = db.collection("myCollection"); // Create/use collection

    await collection.insertOne({ name: "Alice", age: 25 }); // Insert document
    console.log("Document inserted");
  } finally {
    await client.close();
  }
}

run().catch(console.dir);
```

#### ❓Basic Syntax of MongoDB CRUD Operations
#### 🔁 CRUD Operations
#### 🟢 CREATE
```javascript
  //Insert One Document
  db.myCollection.insertOne({
  name: "Alice",
  age: 25,
  city: "New York"
})

  //Insert Multiple Documents
  db.myCollection.insertMany([
  { name: "Bob", age: 30, city: "London" },
  { name: "Charlie", age: 35, city: "Paris" }
])
```
#### 🔵 READ
```javascript
  //Find All Documents
  db.myCollection.find()

  //Find with Filter
  db.myCollection.find({ age: { $gt: 30 } })

  //Find One Document
  db.myCollection.findOne({ name: "Alice" })

  //Find with Projection
  db.myCollection.find(
  { city: "London" },
  { name: 1, _id: 0 }
  )
```

#### 🟡 UPDATE
```javascript
  //Update One Document
  db.myCollection.updateOne(
    { name: "Alice" },
    { $set: { age: 26 } }
  )

  //Update Multiple Documents
  db.myCollection.updateMany(
    { city: "London" },
    { $inc: { age: 1 } }
  )
```

#### 🔴 DELETE
```javascript
  //Delete One Document
  db.myCollection.deleteOne({ name: "Charlie" })

  //Delete Multiple Documents
  db.myCollection.deleteMany({ age: { $gt: 30 } })
```

#### ❓An Index in MongoDB
In MongoDB, an index is a special data structure that improves the speed of queries on a collection. Without indexes, MongoDB must perform a collection scan, which is inefficient for large datasets.

**An Index**
- An index stores a small portion of the data set in an easy-to-search form.
- It is similar to an index in a book – it allows you to quickly find the information you're looking for.
- MongoDB automatically creates an index on `_id` for every collection.

#### Example: Creating and Using an Index
```javascript
// Insert sample data
db.users.insertMany([
  { name: "Alice", age: 25 },
  { name: "Bob", age: 30 },
  { name: "Charlie", age: 35 }
])

// Create an index on age
db.users.createIndex({ age: 1 })

// Query using the index
db.users.find({ age: { $gt: 26 } })
```

#### ❓TTL Indexes in MongoDB
TTL (Time To Live) Indexes in MongoDB are special indexes that automatically remove documents from a collection after a certain period. They are commonly used for data that needs to expire after a specific time, such as session information, logs, or temporary data.
```javascript
// Insert a document with current timestamp
db.tempData.insertOne({
  name: "Temporary Data",
  createdAt: new Date()
})

// Set TTL index to expire after 10 seconds
db.tempData.createIndex(
  { createdAt: 1 },
  { expireAfterSeconds: 10 }
)

// After ~10 seconds, MongoDB will automatically delete this document
```

#### ❓Handle Schema Design and Data Modeling in MongoDB

Schema design and data modeling in MongoDB involve defining how data is organized and stored in a document-oriented database. Unlike SQL databases, MongoDB offers flexible schema design, which can be both an advantage and a challenge. Key considerations for schema design include:

1. Embedding vs. Referencing: Deciding whether to embed related data within a single document or use references between documents.

##### Embedding (Denormalization): 

- Store related data inside a single document.
- Best for one-to-few or read-heavy relationships.
```javascript
  {
  "_id": 1,
  "name": "Alice",
  "orders": [
    { "item": "Book", "price": 20 },
    { "item": "Pen", "price": 5 }
  ]
  }
```
##### ✅ Pros:
- Faster reads (fewer queries).
- Atomic updates for nested fields.
##### ❌ Cons:
- Document size limit (16 MB).
- Risk of data duplication.

##### Referencing (Normalization)
- Store related data in separate collections and link via IDs.
- Best for one-to-many or many-to-many relationships.
```javascript
// User document
{ "_id": 1, "name": "Alice" }

// Orders collection
{ "_id": 101, "userId": 1, "item": "Book", "price": 20 }
```
##### ✅ Pros:
- Data reuse and consistency.
- Smaller documents.

##### ❌ Cons:
- Requires joins using $lookup, which are slower.

**Document Structure**: Designing documents that align with application query patterns for efficient read and write operations.
**Indexing**: Creating indexes to support query performance.
**Data Duplication**: Accepting some level of data duplication to optimize for read performance.
**Sharding**: Designing the schema to support sharding if horizontal scaling is required.

#### ❓MongoDB Compass Tool and Its Functionalities
MongoDB Compass is a `graphical user interface (GUI)` tool for MongoDB that provides an easy way to visualize, explore, and manipulate your data. It offers features such as:

- **Schema Visualization**: View and analyze your data schema, including field types and distributions.
- **Query Building**: Build and execute queries using a visual interface.
- **Aggregation Pipeline**: Construct and run aggregation pipelines.
- **Index Management**: Create and manage indexes to optimize query performance.
- **Performance Monitoring**: Monitor database performance, including slow queries and resource utilization.
- **Data Validation**: Define and enforce schema validation rules to ensure data integrity.
- **Data Import/Export**: Easily import and export data between MongoDB and JSON/CSV files.

#### ❓MongoDB Atlas
MongoDB Atlas is a fully managed cloud database service provided by MongoDB. It offers automated deployment, scaling, and management of MongoDB clusters across various cloud providers (`AWS`, `Azure`, `Google Cloud`). Key differences from self-hosted MongoDB include:

- **Managed Service**: Atlas handles infrastructure management, backups, monitoring, and upgrades.
- **Scalability**: Easily scale clusters up or down based on demand.
- **Security**: Built-in security features such as encryption, access controls, and compliance certifications.
- **Global Distribution**: Deploy clusters across multiple regions for low-latency access and high availability.
- **Integrations**: Seamless integration with other cloud services and MongoDB tools.

#### ❓Capped Collections in MongoDB
A Capped Collection in MongoDB is a fixed-size, high-performance, circular collection that automatically overwrites the oldest documents when the allocated space is full.

Think of it like a circular buffer—when the cap is reached, new data replaces the old data in insertion order.

##### ✅ Good for:
- Logging (e.g., system logs, access logs)
- Real-time stream data (e.g., IoT)
- Caching most recent values
- Maintaining a fixed-length history

#### 🧪 Example
```javascript
db.createCollection("eventLogs", {
  capped: true,
  size: 512000, // 500 KB
  max: 500      // Keep only 500 latest logs
})

// Insert some log entries
db.eventLogs.insertOne({ message: "Server started", timestamp: new Date() })

// Read logs in insertion order
db.eventLogs.find().sort({ $natural: 1 })
```

#### ❓Change Streams in MongoDB
Change Streams in MongoDB allow real-time access to changes (inserts, updates, deletes, etc.) that occur in your collections, databases, or the entire deployment — without polling.

They’re like a live feed of database events you can listen to, making them perfect for event-driven applications, notifications, replication, or caching systems.

#### Example
```javascript
const changeStream = db.collection.watch()

changeStream.forEach(change => {
  printjson(change)
})
```
##### Output
```javascript
{
  "_id": { ... },
  "operationType": "insert",
  "fullDocument": {
    "_id": ObjectId("..."),
    "name": "Alice"
  },
  "ns": {
    "db": "testDB",
    "coll": "users"
  },
  "documentKey": { "_id": ObjectId("...") }
}
```

#### ❓Optimize MongoDB Queries for Performance
Optimizing MongoDB queries involves several strategies:

- **Indexes**: Create appropriate indexes to support query patterns.
- **Query Projections**: Use projections to return only necessary fields.
- **Index Hinting**: Use index hints to force the query optimizer to use a specific index.
- **Query Analysis**: Use the explain() method to analyze query execution plans and identify bottlenecks.
- **Aggregation Pipeline**: Optimize the aggregation pipeline stages to minimize data processing and improve efficiency.

#### ❓Role of Journaling in MongoDB
Journaling in MongoDB ensures data durability and crash recovery by recording changes to the data in a journal file before applying them to the database files. This mechanism allows MongoDB to recover from unexpected shutdowns or crashes by replaying the journal. While journaling provides data safety, it can impact performance due to the additional I/O operations required to write to the journal file.

#### ❓Process of Migrating Data from a Relational Database to MongoDB
Migrating data from a relational database to MongoDB involves several steps:

- **Schema Design**: Redesign the relational schema to fit MongoDB's document-oriented model. Decide on embedding vs. referencing, and plan for indexes and collections.
- **Data Export**: Export data from the relational database in a format suitable for MongoDB (e.g., CSV, JSON).
- **Data Transformation**: Transform the data to match the MongoDB schema. This can involve converting data types, restructuring documents, and handling relationships.
- **Data Import**: Import the transformed data into MongoDB using tools like mongoimport or custom scripts.
Validation: Validate the imported data to ensure consistency and completeness.
- **Application Changes**: Update the application code to interact with MongoDB instead of the relational database.
- **Testing**: Thoroughly test the application and the database to ensure everything works as expected.
- **Go Live**: Deploy the MongoDB database in production and monitor the transition.

#### ✅ When to Use MongoDB
- When you need to handle large volumes of unstructured or semi-structured data.
- When your application requires high availability and horizontal scalability.
- When you need to iterate quickly and adapt your data model without downtime.
- When you want to leverage the flexibility of a document-based data model.
- When you need to support real-time analytics and complex queries on large datasets.

---
---
---


## 🔹What is BSON?
BSON (Binary JSON) is a binary-encoded serialization format that extends JSON (JavaScript Object Notation) to support additional data types, such as Date and Binary. It is the data storage and network transfer format used by MongoDB.

#### Key Features of BSON
- **Binary Format**: BSON is a binary format, which makes it more efficient for storage and transmission compared to plain JSON.
- **Rich Data Types**: BSON supports a wider range of data types than JSON, including:
  - **Date**: BSON has a specific date type for storing dates and times.
  - **Binary**: BSON can store binary data, such as images or files.
  - **ObjectId**: BSON includes a unique identifier type for documents.
- **Document Structure**: Like JSON, BSON represents data as a hierarchical structure of key-value pairs, making it easy to work with complex data models.
- **Size Efficiency**: BSON is designed to be space-efficient, allowing for faster read and write operations in MongoDB.

#### BSON vs. JSON
While BSON is based on JSON, there are some key differences:
- **Encoding**: JSON is a text format, while BSON is a binary format.
- **Data Types**: BSON supports more data types than JSON, making it more versatile for certain applications.
- **Performance**: BSON is optimized for performance in MongoDB, allowing for faster data access and manipulation.

In summary, BSON is a powerful data format that enhances the capabilities of JSON, making it ideal for use in MongoDB and other applications that require efficient data storage and retrieval.
#### Example of BSON Document
```json
{
  "_id": ObjectId("60c72b2f9b1e8c001c8e4d3a"),
  "name": "John Doe",
  "age": 30,
  "email": "john.doe@example.com",
  "address": {
    "street": "123 Main St",
    "city": "Any town",
    "state": "CA",
    "zip": "12345"
  },
  "createdAt": ISODate("2023-10-01T12:00:00Z"),
  "profilePicture": BinData(0, "base64encodeddata"),
  "skills": [
    {
      "name": "JavaScript",
      "level": "Advanced"
    },
    {
      "name": "Python",
      "level": "Intermediate"
    }
  ],
}
```
#### Example of BSON Data Types
```json
{
  "stringField": "Hello, World!", // String
  "intField": 42,                  // Integer
  "doubleField": 3.14,             // Double
  "boolField": true,                // Boolean
  "dateField": ISODate("2023-10-01T12:00:00Z"), // Date
  "binaryField": BinData(0, "base64encodeddata"), // Binary
  "objectIdField": ObjectId("60c72b2f9b1e8c001c8e4d3a") // ObjectId
}
```

---
---
---

## 🔹What is an operator in MongoDB?

In MongoDB, operators are special symbols or keywords used in queries to perform specific operations on the data, such as comparisons, logical evaluations, or updates. Operators are used in queries, projections, updates, aggregations, etc.

#### Types of Operators in MongoDB

1. **Comparison Operators**: Used to compare values.
   - `$eq`: Matches values that are equal to a specified value.
   - `$ne`: Matches all values that are not equal to a specified value.
   - `$gt`: Matches values that are greater than a specified value.
   - `$gte`: Matches values that are greater than or equal to a specified value.
   - `$lt`: Matches values that are less than a specified value.
   - `$lte`: Matches values that are less than or equal to a specified value.
   - `$in`: Matches any of the values specified in an array.
   - `$nin`: Matches none of the values specified in an array.

#### Examples of Comparison Operators

```javascript
// Find documents where age is equal to 30
db.collection.find({ age: { $eq: 30 } })
// Find documents where age is not equal to 25
db.collection.find({ age: { $ne: 25 } })
// Find documents where age is greater than 20
db.collection.find({ age: { $gt: 20 } })
// Find documents where age is greater than or equal to 18
db.collection.find({ age: { $gte: 18 } })
// Find documents where age is less than 40
db.collection.find({ age: { $lt: 40 } })
// Find documents where age is less than or equal to 35
db.collection.find({ age: { $lte: 35 } })
// Find documents where status is either "active" or "pending"
db.collection.find({ status: { $in: ["active", "pending"] } })
// Find documents where status is neither "inactive" nor "archived"
db.collection.find({ status: { $nin: ["inactive", "archived"] } })
```


2. **Logical Operators**: Used to combine multiple conditions.

   - `$and`: Joins query clauses with a logical AND returns all documents that match the conditions of both clauses.
   - `$or`: Joins query clauses with a logical OR returns all documents that match the conditions of either clause.
   - `$not`: Inverts the effect of a query predicate and returns documents that do not match the query predicate.
   - `$nor`: Joins query clauses with a logical NOR returns all documents that fail to match both clauses.

#### Examples of Logical Operators

   ```javascript
  // Find documents where age is greater than 20 AND status is "active"
  db.collection.find({ $and: [{ age: { $gt: 20 } }, { status: "active" }] })
  // Find documents where age is less than 30 OR status is "pending"
  db.collection.find({ $or: [{ age: { $lt: 30 } }, { status: "pending" }] })
  // Find documents where status is NOT "inactive"
  db.collection.find({ status: { $not: { $eq: "inactive" } } })
  // Find documents where status is neither "archived" nor "deleted"
  db.collection.find({ status: { $nor: [{ $eq: "archived" }, { $eq: "deleted" }] } })
  ```

3. **Element Operators**: Used to query the existence or type of fields.

   - `$exists`: Matches documents that have the specified field.
   - `$type`: Selects documents if a field is of the specified type.

#### Examples of Element Operators

```javascript
// Find documents where the "email" field exists
db.collection.find({ email: { $exists: true } })
// Find documents where the "createdAt" field is of type Date
db.collection.find({ createdAt: { $type: "date" } })
// Find documents where the "skills" field is of type Array
db.collection.find({ skills: { $type: "array" } })
// Find documents where the "tags" field is of type Object
db.collection.find({ tags: { $type: "object" } })
// Find documents where the "preferences" field is of type ObjectId
db.collection.find({ preferences: { $type: "objectId" } })
// Find documents where the "location" field is of type GeoJSON
db.collection.find({ location: { $type: "geoJson" } })
// Find documents where the "metadata" field is of type Null
db.collection.find({ metadata: { $type: "null" } })
```

4. **Array Operators**: Used to query and manipulate arrays.

   - `$all`: Matches all elements in an array
   - `$elemMatch`: Matches documents with at least one element in an array that matches the specified criteria
   - `$size`: Matches arrays with a specific size

#### Examples of Array Operators

```javascript
// Find documents where the "tags" array contains both "mongodb" and "database"
db.collection.find({ tags: { $all: ["mongodb", "database"] } })
// Find documents where the "skills" array contains at least one element with name "JavaScript" and level "Advanced"
db.collection.find({ skills: { $elemMatch: { name: "JavaScript", level: "Advanced" } } })
// Find documents where the "tags" array has exactly 3 elements
db.collection.find({ tags: { $size: 3 } })
// Find documents where the "skills" array has at least 2 elements
db.collection.find({ skills: { $size: { $gte: 2 } } })
// Find documents where the "preferences" array has at most 5 elements
db.collection.find({ preferences: { $size: { $lte: 5 } } })
// Find documents where the "hobbies" array has exactly 0 elements
db.collection.find({ hobbies: { $size: 0 } })
// Find documents where the "tags" array contains at least one element that starts with "mongo"
db.collection.find({ tags: { $elemMatch: { $regex: /^mongo/ } } })
// Find documents where the "skills" array contains at least one element with level "Intermediate"
db.collection.find({ skills: { $elemMatch: { level: "Intermediate" } } })
// Find documents where the "preferences" array contains at least one element that is a string
db.collection.find({ preferences: { $elemMatch: { $type: "string" } } })
// Find documents where the "hobbies" array contains at least one element that is a number
db.collection.find({ hobbies: { $elemMatch: { $type: "number" } } } })
// Find documents where the "tags" array contains at least one element that is an object
db.collection.find({ tags: { $elemMatch: { $type: "object" } } } })
// Find documents where the "skills" array contains at least one element that is a boolean
db.collection.find({ skills: { $elemMatch: { $type: "bool" } } })
```

5. **Update Operators**: Used to modify existing documents.

   - `$set`: Sets the value of a field
   - `$unset`: Removes the specified field from a document.
   - `$inc`: Increments the value of the field by the specified amount.
   - `$push`: Adds an element to an array
   - `$pull`: Removes all array elements that match a specified query.
   - `$addToSet`: Adds elements to an array only if they do not already exist in the set.
   - `$pop`: Removes the first or last element from an array
   - `$pullAll`: Removes all matching values from an array.
   - `$rename`: Renames a field
   - `$currentDate`: Sets the value of a field to current date, either as a Date or a Timestamp.
   - `$min`: Only updates the field if the specified value is less than the existing field value.
   - `$max`: Only updates the field if the specified value is greater than the existing field value.
   - `$mul`: Multiplies the value of the field by the specified amount.
   - `$setOnInsert`: Sets the value of a field if an update results in an insert of a document. Has no effect on update operations that modify existing documents.
   - `$`: Acts as a placeholder to update the first element that matches the query condition.
   - `$[]`: Acts as a placeholder to update all elements in an array for the documents that match the query condition.
   - `$[<identifier>]`: Acts as a placeholder to update all elements that match the `arrayFilters` condition for the documents that match the query condition.
   - `$each`: Modifies the `$push` and `$addToSet` operators to append multiple items for array updates.
   - `$position`: Modifies the `$push` operator to specify the position in the array to add elements.
   - `$slice`: Modifies the `$push` operator to limit the size of updated arrays.
   - `$sort`: Modifies the `$push` operator to reorder documents stored in an array.


#### Examples of Update Operators

```javascript
// Update the "age" field to 31 for documents where name is "John Doe"
db.collection.updateOne({ name: "John Doe" }, { $set: { age: 31 } })
// Remove the "email" field from documents where status is "inactive"
db.collection.updateMany({ status: "inactive" }, { $unset: { email: "" } })
// Increment the "views" field by 1 for documents where status is "active"
db.collection.updateMany({ status: "active" }, { $inc: { views: 1 } })
// Add "JavaScript" to the "skills" array for documents where name is "Jane Smith"
db.collection.updateOne({ name: "Jane Smith" }, { $push: { skills: "JavaScript" } })
// Remove "Python" from the "skills" array for documents where name is "John Doe"
db.collection.updateOne({ name: "John Doe" }, { $pull: { skills: "Python" } })
// Set the "status" field to "active" for documents where age is greater than 25
db.collection.updateMany({ age: { $gt: 25 } }, { $set: { status: "active" } })
// Unset the "preferences" field for documents where status is "archived"
db.collection.updateMany({ status: "archived" }, { $unset: { preferences: "" } })
// Increment the "likes" field by 10 for documents where category is "technology"
db.collection.updateMany({ category: "technology" }, { $inc: { likes: 10 } })
// Push "React" to the "frameworks" array for documents where name is "Alice Johnson"
db.collection.updateOne({ name: "Alice Johnson" }, { $push: { frameworks: "React" } })
// Pull "Angular" from the "frameworks" array for documents where name is "Bob Smith"
db.collection.updateOne({ name: "Bob Smith" }, { $pull: { frameworks: "Angular" } })
// Set the "lastLogin" field to the current date for documents where status is "active"
db.collection.updateMany({ status: "active" }, { $set: { lastLogin: new Date() } })
// Unset the "profilePicture" field for documents where status is "inactive"
db.collection.updateMany({ status: "inactive" }, { $unset: { profilePicture: "" } })
// Add "Node.js" to the "skills" array only if it does not already exist for documents where name is "Charlie Brown"
db.collection.updateOne({ name: "Charlie Brown" }, { $addToSet: { skills: "Node.js" } })
// Pop the last element from the "comments" array for documents where name is "David Wilson"
db.collection.updateOne({ name: "David Wilson" }, { $pop: { comments: 1 } }) 
// Rename the "username" field to "user_name" for documents where name is "Emily Davis"
db.collection.updateMany({ name: "Emily Davis" }, { $rename: { username: "user_name" } })
// Multiply the "salary" field by 1.1 for documents where department is "HR"
db.collection.updateMany({ department: "HR" }, { $mul: { salary: 1.1 } })
// Set "score" to 80 only if it's currently greater than 80
db.collection.updateOne({ name: "John Doe" }, { $min: { score: 80 } })
// Set "score" to 90 only if it's currently less than 90
db.collection.updateOne({ name: "John Doe" }, { $max: { score: 90 } })
// Set the "lastModified" field to the current date
db.collection.updateOne({ name: "John Doe" }, { $currentDate: { lastModified: true } })
// Set "createdAt" only during an upsert (insert if not exists)
db.collection.updateOne(
  { email: "john@example.com" },
  {
    $set: { name: "John Doe" },
    $setOnInsert: { createdAt: new Date() }
  },
  { upsert: true }
)
// Add multiple hobbies at once
db.collection.updateOne(
  { name: "John Doe" },
  { $push: { hobbies: { $each: ["hiking", "biking"] } } }
)
// Keep only the last 3 elements in the "logs" array
db.collection.updateOne(
  { userId: 123 },
  { $push: { logs: { $each: [newLog], $slice: -3 } } }
)
// Insert "jogging" at position 1 in the "hobbies" array
db.collection.updateOne(
  { name: "John Doe" },
  { $push: { hobbies: { $each: ["jogging"], $position: 1 } } }
)

```

6. **Bitwise Operators**: Used for bitwise operations on numeric values.

   - `$bitsAllClear`: Checks if all bits are clear
   - `$bitsAllSet`: Checks if all bits are set
   - `$bitsAnyClear`: Checks if any bits are clear
   - `$bitsAnySet`: Checks if any bits are set

#### Examples of Bitwise Operators

```javascript
// Find documents where the "flags" field has all bits clear (0b0000)
db.collection.find({ flags: { $bitsAllClear: 0b0000 } })
// Find documents where the "flags" field has all bits set (0b1111)
db.collection.find({ flags: { $bitsAllSet: 0b1111 } })
// Find documents where the "flags" field has any bits clear (0b0001)
db.collection.find({ flags: { $bitsAnyClear: 0b0001 } })
// Find documents where the "flags" field has any bits set (0b0001)
db.collection.find({ flags: { $bitsAnySet: 0b0001 } })
```
7. **Geospatial Operators**: Used for querying geospatial data.
   - `$geoWithin`: Matches documents within a specified geometry
   - `$near`: Finds documents near a specified point
   - `$geoIntersects`: Matches documents that intersect with a specified geometry
#### Examples of Geospatial Operators
```javascript
// Find documents within a specified polygon
db.collection.find({
  location: {
    $geoWithin: {
      $geometry: {
        type: "Polygon",
        coordinates: [
          [
            [-73.97, 40.77],
            [-73.98, 40.78],
            [-73.99, 40.77],
            [-73.97, 40.77]
          ]
        ]
      }
    }
  }
})
// Find documents near a specified point
db.collection.find({
  location: {
    $near: {
      $geometry: {
        type: "Point",
        coordinates: [-73.97, 40.77]
      },
      $maxDistance: 1000 // in meters
    }
  }
})
// Find documents that intersect with a specified geometry
db.collection.find({
  location: {
    $geoIntersects: {
      $geometry: {
        type: "LineString",
        coordinates: [
          [-73.97, 40.77],
          [-73.98, 40.78]
        ]
      }
    }
  }
})
``` 


8. **Aggregation Operators**: Used in aggregation pipelines to perform data transformations and calculations.

#### 📌 8.1 - Expression Operators

Used in stages like `$project`, `$group`, and `$addFields`.

#### 🔢 Arithmetic Operators

| Operator      | Description           | Example                                  |
|---------------|------------------------|-------------------------------------------|
| `$add`        | Adds numbers           | `{ $add: [5, "$price"] }`                 |
| `$subtract`   | Subtracts numbers      | `{ $subtract: ["$price", "$discount"] }`  |
| `$multiply`   | Multiplies numbers     | `{ $multiply: ["$price", "$quantity"] }`  |
| `$divide`     | Divides numbers        | `{ $divide: ["$total", "$quantity"] }`    |
| `$mod`        | Modulus (remainder)    | `{ $mod: ["$score", 2] }`                 |


#### 📌 8.2 - Array Operators

Work with array fields in documents.

| Operator        | Description               | Example |
|-----------------|---------------------------|---------|
| `$size`         | Returns size of array     | `{ $size: "$tags" }` |
| `$arrayElemAt`  | Get element by index      | `{ $arrayElemAt: ["$items", 0] }` |
| `$concatArrays` | Concatenates arrays       | `{ $concatArrays: ["$arr1", "$arr2"] }` |
| `$filter`       | Filters array items       | `{ $filter: { input: "$scores", as: "score", cond: { $gt: ["$$score", 80] } } }` |
| `$in`           | Checks if value in array  | `{ $in: ["apple", "$fruits"] }` |


#### 📌 8.3 - String Operators

Manipulate and evaluate string values.

| Operator       | Description             | Example |
|----------------|--------------------------|---------|
| `$concat`      | Concatenate strings      | `{ $concat: ["$firstName", " ", "$lastName"] }` |
| `$substrBytes` | Substring by bytes       | `{ $substrBytes: ["$desc", 0, 5] }` |
| `$toLower`     | Lowercase string         | `{ $toLower: "$name" }` |
| `$toUpper`     | Uppercase string         | `{ $toUpper: "$name" }` |
| `$trim`        | Trim chars from string   | `{ $trim: { input: "$name", chars: " " } }` |


#### 📌 8.4 - Comparison Operators

Used in conditions to compare values.

| Operator | Description     | Example |
|----------|------------------|---------|
| `$eq`    | Equal to         | `{ $eq: ["$status", "active"] }` |
| `$ne`    | Not equal to     | `{ $ne: ["$status", "inactive"] }` |
| `$gt`    | Greater than     | `{ $gt: ["$price", 100] }` |
| `$gte`   | Greater or equal | `{ $gte: ["$price", 100] }` |
| `$lt`    | Less than        | `{ $lt: ["$price", 50] }` |
| `$lte`   | Less or equal    | `{ $lte: ["$price", 50] }` |
| `$cmp`   | Compare values   | `{ $cmp: ["$a", "$b"] }` |


#### 📌 8.5 - Boolean Operators

Logical operators for building conditions.

| Operator | Description    | Example |
|----------|----------------|---------|
| `$and`   | Logical AND    | `{ $and: [{ $gt: ["$qty", 10] }, { $lt: ["$qty", 50] }] }` |
| `$or`    | Logical OR     | `{ $or: [{ $eq: ["$status", "A"] }, { $eq: ["$status", "B"] }] }` |
| `$not`   | Logical NOT    | `{ $not: [{ $eq: ["$status", "A"] }] }` |



#### 📌 8.6 - Conditional Operators

Evaluate conditional logic.

| Operator  | Description     | Example |
|-----------|------------------|---------|
| `$cond`   | If-then-else     | `{ $cond: { if: { $gt: ["$qty", 100] }, then: "High", else: "Low" } }` |
| `$ifNull` | Default if null  | `{ $ifNull: ["$discount", 0] }` |
| `$switch` | Multiple cases   | `{ $switch: { branches: [{ case: { $eq: ["$grade", "A"] }, then: "Excellent" }], default: "Average" } }` |



#### 📌 8.7 - Date Operators

Manipulate and format dates.

| Operator         | Description             | Example |
|------------------|--------------------------|---------|
| `$year`          | Extract year             | `{ $year: "$createdAt" }` |
| `$month`         | Extract month            | `{ $month: "$createdAt" }` |
| `$dayOfWeek`     | Extract weekday          | `{ $dayOfWeek: "$createdAt" }` |
| `$dateToString`  | Format date as string    | `{ $dateToString: { format: "%Y-%m-%d", date: "$createdAt" } }` |
| `$dateAdd`       | Add to date              | `{ $dateAdd: { startDate: "$createdAt", unit: "day", amount: 7 } }` |



#### 📌 8.8 - Set Operators

Operate on sets (arrays with unique values).

| Operator         | Description             | Example |
|------------------|--------------------------|---------|
| `$setEquals`     | Check set equality       | `{ $setEquals: [["$a", "$b"]] }` |
| `$setIntersection` | Common elements        | `{ $setIntersection: [["$tags1", "$tags2"]] }` |
| `$setUnion`      | Combine unique values    | `{ $setUnion: [["$a", "$b"]] }` |
| `$setDifference` | Items in A not in B      | `{ $setDifference: [["$a", "$b"]] }` |



#### 📌 8.9 - Accumulators (for `$group` stage)

Aggregate values from multiple documents.

| Operator     | Description       | Example |
|--------------|-------------------|---------|
| `$sum`       | Sum of values     | `{ $sum: "$amount" }` |
| `$avg`       | Average            | `{ $avg: "$score" }` |
| `$min` / `$max` | Min/Max         | `{ $min: "$price" }` |
| `$push`      | Add to array       | `{ $push: "$name" }` |
| `$addToSet`  | Unique values      | `{ $addToSet: "$tag" }` |
| `$first`     | First document     | `{ $first: "$score" }` |
| `$last`      | Last document      | `{ $last: "$score" }` |



#### 📌 8.10 - Type Conversion Operators

Convert data types.

| Operator     | Description        | Example |
|--------------|---------------------|---------|
| `$toString`  | Converts to string  | `{ $toString: "$price" }` |
| `$toInt`     | Converts to int     | `{ $toInt: "$price" }` |
| `$toDouble`  | Converts to float   | `{ $toDouble: "$price" }` |
| `$convert`   | Generic converter   | `{ $convert: { input: "$price", to: "int", onError: 0 } }` |



#### 📌 8.11 - Math Operators

Math and trigonometry functions.

| Operator   | Description     | Example |
|------------|------------------|---------|
| `$abs`     | Absolute value   | `{ $abs: "$value" }` |
| `$ceil`    | Round up         | `{ $ceil: "$value" }` |
| `$floor`   | Round down       | `{ $floor: "$value" }` |
| `$round`   | Round to places  | `{ $round: ["$value", 2] }` |
| `$sqrt`    | Square root      | `{ $sqrt: "$value" }` |
| `$sin` / `$cos` / `$tan` | Trigonometry | `{ $sin: "$angle" }` |


#### Examples of Aggregation Operators
```javascript
// Aggregate documents to calculate the average age by status
db.collection.aggregate([
  {
    $group: {
      _id: "$status",
      averageAge: { $avg: "$age" }
    }
  }
])
// Sort documents by age in ascending order
db.collection.aggregate([
  {
    $sort: { age: 1 }
  }
])
// Project documents to include only name and age fields
db.collection.aggregate([
  {
    $project: {
      name: 1,
      age: 1
    }
  }
])
// Match documents where status is "active" and group by name to count occurrences
db.collection.aggregate([
  {
    $match: { status: "active" }
  },
  {
    $group: {
      _id: "$name",
      count: { $sum: 1 }
    }
  }
])
// Match documents where gender is "Male" and age is less than 30
db.test.aggregate([
    // stage - 1
    { $match: { gender: "Male", age: { $lt: 30 } } },
    // stage - 2: Select only the name, age, and gender fields
    { $project: { name: 1, age: 1, gender: 1 } }
])
// Add fields and export the output to a new collection
db.test.aggregate([
    // stage - 1: Match only male users
    { $match: { gender: "Male" } },
    // stage - 2: Add new fields to each document
    { $addFields: { course: "level-2", eduTech: "Programming Hero" } },
    // stage - 3: Show only the added fields
    { $project: { course: 1, eduTech: 1 } },
    // stage - 4: Export the pipeline output to a new collection called "course-student"
    { $out: "course-student" }
])
// Merge the results back into the same collection
db.test.aggregate([
    // stage - 1: Filter only male documents
    { $match: { gender: "Male" } },
    // stage - 2: Add fields to the matching documents
    { $addFields: { course: "level-2", eduTech: "Programming Hero" } },
    // stage - 3: Show only course and eduTech fields
    { $project: { course: 1, eduTech: 1 } },
    // stage - 4: Merge the results back into the same "test" collection
    { $merge: "test" }
])
// Group documents by age and list full documents per group
db.test.aggregate([
    // stage - 1: Group by "age" and gather the full document and count
    { $group: {
        _id: "$age", 
        fullDoc: { $push: "$$ROOT" }, 
        count: { $sum: 1 } }
    },
    // stage - 2: Show only name, email, and phone inside grouped docs
    { $project: {
        "fullDoc.name": 1,
        "fullDoc.email": 1,
        "fullDoc.phone": 1
    }}
])
// Calculate salary stats and differences
db.test.aggregate([
    // stage - 1: Group all documents and calculate salary metrics
    { $group: {
        _id: null,
        totalSalary: { $sum: "$salary" },
        maxSalary: { $max: "$salary" },
        minSalary: { $min: "$salary" },
        avgSalary: { $avg: "$salary" }
    }},
    // stage - 2: Project salary values and compute range
    { $project: {
        totalSalary: 1,
        maxSalary: 1,
        minSalary: 1,
        averageSalary: "$avgSalary",
        rangeSalary: { $subtract: ["$maxSalary", "$minSalary"] }
    }}
])
// Unwind "friends" array and count occurrences
db.test.aggregate([
    // stage - 1: Deconstruct "friends" array
    { $unwind: "$friends" },
    // stage - 2: Group by each friend and count how many times they appear
    { $group: { _id: "$friends", count: { $sum: 1 } } }
])
// Unwind "interests" and group by age
db.test.aggregate([
    // stage - 1: Flatten the "interests" array
    { $unwind: "$interests" },
    // stage - 2: Group by age, collect interests per age
    { $group: { _id: "$age", interestPerAge: { $push: "$interests" } } }
])
// Bucket documents by age into ranges
db.test.aggregate([
    // stage - 1: Create buckets for age groups
    { $bucket: {
        groupBy: "$age",
        boundaries: [20, 40, 60, 80], // Age ranges
        default: "upper 80",         // Anything above 80
        output: {
            count: { $sum: 1 },
            Name: { $push: "$name" }
        }
    }},
    // stage - 2: Sort by count descending
    { $sort: { count: -1 } },
    // stage - 3: Take top 2 buckets
    { $limit: 2 },
    // stage - 4: Show only the count
    { $project: { count: 1 } }
])
// Run multiple pipelines in parallel using $facet
db.test.aggregate([
    {
        $facet: {
            // pipeline - 1: Count friends occurrences
            "friendsCount": [
                // stage - 1
                { $unwind: "$friends" },
                // stage - 2
                { $group: { _id: "$friends", count: { $sum: 1 } } }
            ],
            // pipeline - 2: Count education types
            "eduCount": [
                // stage - 1
                { $unwind: "$education" },
                // stage - 2
                { $group: { _id: "$education", count: { $sum: 1 } } }
            ],
            // pipeline - 3: Count skill occurrences
            "skillsCount": [
                // stage - 1
                { $unwind: "$skills" },
                // stage - 2
                { $group: { _id: "$skills", count: { $sum: 1 } } }
            ]
        }
    }
])
// Join documents from another collection using $lookup
db.Orders.aggregate([
    {
        $lookup: {
            from: "test",            // Foreign collection
            localField: "userId",    // Field in current collection
            foreignField: "_id",     // Field in foreign collection
            as: "user"               // Output array field
        }
    }
])
// Analyze how a query is executed with execution stats
db.test.find({ _id: ObjectId("6406ad63fc13ae5a40000066") })
.explain("executionStats")
// Create an ascending index on the 'email' field
db.getCollection("massive-data").createIndex({ email: 1 })
// Create a full-text index on the 'about' field
db.getCollection("massive-data").createIndex({ about: "text" })
// Perform a full-text search for the word "dolor" in the 'about' field
db.getCollection("massive-data")
.find({ $text: { $search: "dolor" } })
.project({ about: 1 })
```


## 🧠 MongoDB Command Cheat Sheet

#### 📁 DATABASE COMMANDS
```javascript
// Show all databases
show dbs
// Switch to a specific database
use myDatabase
// Create a new database (will be created when you insert data)
use newDatabase
// Show the current database
db
// Drop the current database
db.dropDatabase()
// Delete a specific database
db.getSiblingDB("databaseName").dropDatabase()
```
#### 📂 COLLECTION COMMANDS
```javascript
// Show all collections in the current database
show collections
// Create a new collection (will be created when you insert data)
db.createCollection("myCollection")
// Drop a specific collection
db.myCollection.drop()
// Rename a collection
db.myCollection.renameCollection("newCollectionName")
// Count the number of documents in a collection
db.myCollection.countDocuments()
// Check if a collection exists
db.getCollectionNames().includes("myCollection")
// Get information about a collection
db.myCollection.stats()
// Get the first document in a collection
db.myCollection.findOne()
// Get all documents in a collection
db.myCollection.find()
// Get documents with specific fields
db.myCollection.find({}, { field1: 1, field2: 1 })
// Get documents with a specific condition and limit the number of results
db.myCollection.find({ status: "active" }).limit(10)
// Get documents with a specific condition and limit the number of results
db.myCollection.find({ status: "active" }).limit(10).skip(5)
// Get documents with a specific condition and sort the results
db.myCollection.find({ status: "active" }).sort({ age: -1 }).limit(10)
// Get documents with a specific condition and project specific fields, including the _id field and excluding the __v field, createdAt field, updatedAt field, deletedAt field, status field, tags field, description field, and notes field
db.myCollection.find({ status: "active" }, { name: 1, age: 1, _id: 1, __v: 0, createdAt: 0, updatedAt: 0, deletedAt: 0, status: 0, tags: 0, description: 0, notes: 0 })
```
#### 📄 CRUD OPERATIONS

#### ➕ INSERT
```javascript
// Insert a single document
db.myCollection.insertOne({ name: "John", age: 30, status: "active" })
// Insert multiple documents
db.myCollection.insertMany([
  { name: "Jane", age: 25, status: "active" },
  { name: "Mike", age: 35, status: "inactive" }
])
// Insert a document and return the inserted document
db.myCollection.insertOne({ name: "Alice", age: 28, status: "active" }, { writeConcern: { w: "majority" } })
// Insert a document and return the inserted document with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field
db.myCollection.insertOne({ name: "Hank", age: 34, status: "active", createdAt: new Date(), updatedAt: new Date(), notes: "New user" }, { writeConcern: { w: "majority" } }).insertedId.status.age.name.createdAt.updatedAt.notes
```

#### 🔍 READ

```javascript
// Find a single document by _id
db.myCollection.findOne({ _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") })
// Find documents with a specific condition
db.myCollection.find({ status: "active" })
// Find documents with a specific condition and limit the number of results
db.myCollection.find({ status: "active" }).limit(10)
// Find documents with a specific condition and return only the first matching document with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field, and sort the results by age in descending order, and skip the first 5 results, and limit the results to 10, and return only the _id field and the name field, and return only the first matching document with the _id field and the name field, and convert the result to an array
db.myCollection.find({ status: "active" }, { _id: 1, status: 1, age: 1, name: 1, createdAt: 1, updatedAt: 1, notes: 1 }).sort({ age: -1 }).skip(5).limit(10).project({ _id: 1, name: 1 }).limit(1).toArray()
// Find documents with a specific condition and return only the first matching document with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field, and sort the results by age in descending order, and skip the first 5 results, and limit the results to 10, and return only the _id field and the name field, and return only the first matching document with the _id field and the name field, and convert the result to an array, and return only the first matching document
db.myCollection.find({ status: "active" }, { _id: 1, status: 1, age: 1, name: 1, createdAt: 1, updatedAt: 1, notes: 1 }).sort({ age: -1 }).skip(5).limit(10).project({ _id: 1, name: 1 }).limit(1).toArray().then(result => result[0])
```

#### ✏️ UPDATE
```javascript
// Update a single document by _id
db.myCollection.updateOne({ _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") }, { $set: { age: 31 } })
// Update multiple documents with a specific condition
db.myCollection.updateMany({ status: "active" }, { $set: { status: "inactive" } })
// Update multiple documents and return the updated documents with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field
db.myCollection.updateMany(
  { status: "active" },
  { $set: { status: "inactive" } },
  { returnDocument: "after", projection: { _id: 1, status: 1, age: 1, name: 1, createdAt: 1, updatedAt: 1, notes: 1 } }
)
```

#### ❌ DELETE
```javascript
// Delete a single document by _id
db.myCollection.deleteOne({ _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") })
// Delete multiple documents with a specific condition
db.myCollection.deleteMany({ status: "inactive" })
// Delete all documents in a collection
db.myCollection.deleteMany({})
// Delete a specific document and return the deleted document with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field
db.myCollection.findOneAndDelete({ _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") }, { projection: { _id: 1, status: 1, age: 1, name: 1, createdAt: 1, updatedAt: 1, notes: 1 } })
```

#### 🔄 UPSERT
```javascript
// Update a document or insert a new one if it doesn't exist
db.myCollection.updateOne(
  { _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") },
  { $set: { age: 32 } },
  { upsert: true }
)
// Update a document or insert a new one if it doesn't exist, and return the updated or inserted document with the _id field, status field, age field, name field, createdAt field, updatedAt field, and notes field
db.myCollection.findOneAndUpdate(
  { _id: ObjectId("60c72b2f9b1e8c001c8e4d3a") },
  { $set: { age: 32 } },
  { returnDocument: "after", upsert: true, projection: { _id: 1, status: 1, age: 1, name: 1, createdAt: 1, updatedAt: 1, notes: 1 } }
)
```

#### 📊 AGGREGATION

```javascript
// Basic Aggregate
db.orders.aggregate([
  { $match: { status: "delivered" } },
  { $group: { _id: "$customer", total: { $sum: "$amount" } } },
  { $sort: { total: -1 } }
])
// Aggregate documents to filter by status, sort by age, skip the first 5 results, limit the results to 10, and project specific fields, and return only the first matching document
db.myCollection.aggregate([
  {
    $match: { status: "active" }
  },
  {
    $sort: { age: -1 }
  },
  {
    $skip: 5
  },
  {
    $limit: 10
  },
  {
    $project: {
      _id: 1,
      name: 1,
      age: 1,
      status: 1
    }
  },
  {
    $limit: 1
  }
])
```

#### 📌 INDEXES

```javascript
// Create an index on a specific field
db.myCollection.createIndex({ name: 1 })
// Create a unique index on a specific field
db.myCollection.createIndex({ email: 1 }, { unique: true })
// Check if an index exists
db.myCollection.getIndexes().some(index => index.name === "index_name")
// Get the index size for a specific field and direction, and return only the index size in megabytes
db.myCollection.stats().indexSizes["field_name_1"].sizeInMegabytes
// Get the index size for all fields and directions, and return only the index size in megabytes
db.myCollection.stats().indexSizes.sizeInMegabytes
```

#### 📈 PERFORMANCE

```javascript
// Explain a query to see its execution plan
db.myCollection.find({ status: "active" }).explain("executionStats")
// Get the current server status
db.serverStatus()
// Get the current database statistics
db.myCollection.stats()
// Get the current collection statistics
db.myCollection.stats()
// Get the current index statistics for all indexes and return only the index size in terabytes
db.myCollection.getIndexes().map(index => ({ name: index.name, size: index.size / (1024 * 1024 * 1024 * 1024) }))
```

#### 🔒 SECURITY

```javascript
// Create a new user with readWrite access to a specific database
db.createUser({
  user: "myUser",
  pwd: "myPassword",
  roles: [
    { role: "readWrite", db: "myDatabase" }
  ]
})
// Authenticate a user
db.auth("myUser", "myPassword")
// Drop a user
db.dropUser("myUser")
// Change a user's password
db.updateUser("myUser", { pwd: "newPassword" })
// List all users in the current database
db.getUsers()
// Check if a user exists
db.getUsers().some(user => user.user === "myUser")
// Get the current user's roles
db.runCommand({ connectionStatus: 1 }).authInfo.authenticatedUsers
// Get the current user's roles and return only the role names
db.runCommand({ connectionStatus: 1 }).authInfo.authenticatedUsers.map(user => user.roles).flat().map(role => role.role)
// Enable authentication for the MongoDB server
// This is typically done in the MongoDB configuration file (mongod.conf) by setting the `security.authorization` option to "enabled".
// Example configuration:
security:
  authorization: "enabled"
// Restart the MongoDB server after making changes to the configuration file
```

#### 🔄 BACKUP & RESTORE (CLI)

```bash
# Backup a MongoDB database
mongodump --db myDatabase --out /path/to/backup
# Restore a MongoDB database from a backup
mongorestore --db myDatabase /path/to/backup/myDatabase
# Backup a specific collection
mongodump --db myDatabase --collection myCollection --out /path/to/backup
# Restore a specific collection from a backup
mongorestore --db myDatabase --collection myCollection /path/to/backup/myDatabase/myCollection.bson
# Backup all databases
mongodump --out /path/to/backup
# Restore all databases from a backup
mongorestore /path/to/backup
# Backup a MongoDB database with authentication
mongodump --db myDatabase --out /path/to/backup --username myUser --password myPassword --authenticationDatabase admin
# Restore a MongoDB database with authentication
mongorestore --db myDatabase /path/to/backup/myDatabase --username myUser --password myPassword --authenticationDatabase admin
# Backup a MongoDB database with SSL/TLS enabled
mongodump --db myDatabase --out /path/to/backup --ssl --sslAllowInvalidCertificates
# Restore a MongoDB database with SSL/TLS enabled
mongorestore --db myDatabase /path/to/backup/myDatabase --ssl --sslAllowInvalidCertificates
# Backup a MongoDB database with compression
mongodump --db myDatabase --out /path/to/backup --gzip
# Restore a MongoDB database with compression
mongorestore --db myDatabase /path/to/backup/myDatabase --gzip
# Backup a MongoDB database with a specific query filter
mongodump --db myDatabase --collection myCollection --query '{ "status": "active" }' --out /path/to/backup
# Restore a MongoDB database with a specific query filter
mongorestore --db myDatabase /path/to/backup/myDatabase --query '{ "status": "active" }'
```




# Mongoose

## 🔹What is Mongoose and how does it relate to MongoDB?

Mongoose is an Object Data Modeling (ODM) library for MongoDB and Node.js. It provides a schema-based solution to model application data, allowing developers to define schemas for their data structures, enforce validation, and create relationships between different data models. Mongoose simplifies interactions with MongoDB by providing a higher-level abstraction over the native MongoDB driver, making it easier to work with MongoDB in a Node.js environment.

#### Key Features of Mongoose:
- **Schema Definition**: Mongoose allows you to define schemas for your data models, specifying the structure, data types, and validation rules for each field.
- **Data Validation**: Mongoose provides built-in validation mechanisms to ensure that data adheres to the defined schema before it is saved to the database.
- **Middleware**: Mongoose supports middleware functions (also known as hooks) that can be executed before or after certain operations, such as saving a document or removing it from the database.
- **Query Building**: Mongoose provides a powerful query builder that allows you to construct complex queries using a fluent API, making it easier to retrieve and manipulate data.
- **Relationships**: Mongoose supports relationships between different data models, allowing you to create references to other documents and populate related data easily.
- **Integration with MongoDB**: Mongoose integrates seamlessly with the MongoDB driver, allowing you to use Mongoose to interact with MongoDB in a Node.js application.
- **Plugins**: Mongoose supports plugins, which are reusable pieces of code that can extend the functionality of Mongoose models, such as adding timestamps or soft delete functionality.
- **Type Casting**: Mongoose automatically casts values to the appropriate data types defined in the schema, ensuring data consistency.
- **Error Handling**: Mongoose provides comprehensive error handling mechanisms, allowing you to catch and handle errors that occur during database operations.


#### ❓Advantages of using Mongoose for MongoDB
- **Data Modeling**: Mongoose helps you store MongoDB document structures like JavaScript objects through schemas. It supports powerful features such as data validation, setting default values, and nested document modeling.
- **Intuitive API**: Mongoose provides a simple and intuitive API for MongoDB CRUD operations. It helps in writing code and frees you from MongoDB's syntax.
- **Validation and Business Logic**: Mongoose allows you to apply document validation and business logic through schemas. These validations can be applied before, after, or over time.
- **Middleware and Hooks**: Mongoose helps you create custom logic for pre/post save, pre/post remove, and other events. It lets you interact with the database more intricately.
- **Pagination and Aggregation**: Mongoose offers the ability to implement paging, sorting, grouping, and other statistical features, making complex queries possible for your applications.
- **Logging and Debugging**: Mongoose provides a convenient framework for logging and debugging MongoDB queries, helping you identify and resolve issues with database communication.
- **Extensibility**: Mongoose is an open-source project, so it can be extended with various plugins and extensions, allowing you to add more complex features and functionalities.

#### 🏗️ Folder Structure (MVC Pattern with Mongoose)
```plaintext
project-root/
├── models/                # Mongoose models
│   ├── User.js            # User model
│   ├── Product.js         # Product model
│   └── Order.js           # Order model
├── controllers/           # Controllers for handling HTTP requests
│   ├── UserController.js  # Controller for User model
│   ├── ProductController.js # Controller for Product model
│   └── OrderController.js # Controller for Order model
├── routes/                # Express routes for handling HTTP requests
│   ├── userRoutes.js      # Route for User model
│   ├── productRoutes.js   # Route for Product model
│   └── orderRoutes.js     # Route for Order model
├── services/              # Services for business logic
│   ├── userService.js     # Service for User model
│   ├── productService.js  # Service for Product model
│   └── orderService.js    # Service for Order model
├── app.js                 # Main application file
├── package.json           # Package.json file
└── tsconfig.json          # TypeScript configuration file
```

#### 🏗️ Folder Structure (Modular Pattern with Mongoose)
```plaintext
project/
│
├── modules/
│   ├── user/
│   │   ├── user.model.js
│   │   ├── user.controller.js
│   │   ├── user.routes.js
│   │   ├── user.service.js
│   │   └── user.validation.js
│   │
│   ├── auth/
│   │   ├── auth.controller.js
│   │   ├── auth.routes.js
│   │   ├── auth.service.js
│   │   └── auth.middleware.js
│
├── config/
│   ├── db.js
│   └── env.js
│
├── middlewares/
│   ├── error.middleware.js
│   ├── validate.middleware.js
│
├── utils/
│   ├── ApiError.js
│   └── catchAsync.js
│
├── app.js
├── server.js
└── .env
```


#### ❓ TypeScript Interface in Mongoose
TypeScript is a superset of JavaScript that adds static typing to the language. It helps catch errors early in the development process by providing type checking and code completion features. TypeScript interfaces can be used in Mongoose to define the structure of documents in a MongoDB collection. This allows you to define the expected shape of documents and provides type safety during development.
#### Example TypeScript Interface in Mongoose
```javascript
export interface IAddress {
  country: string;
  city: string;
  street: string;
  zip: string;
} 
// Embedding the IAddress interface in IUser interface

export interface IUser {
  _id?: string;
  fullName: string;
  email: string;
  passwordHash: string;
  role?: 'admin' | 'manager' | 'user';
  isActive?: boolean;
  lastLogin?: Date | null;
  address: IAddress;
  createdAt?: Date;
  updatedAt?: Date;
}

export interface IOrganization {
  _id?: string;
  name: string;
  industry?: 'tech' | 'finance' | 'education' | 'healthcare';
  createdAt?: Date;
  updatedAt?: Date;
}

// Instance Methods
export interface UserInstanceMethods {
  hashPassword(password: string): Promise<string>;
}

// Static Methods
export interface UserStaticMethods extends model<IUser>{
  hashPassword(password: string): Promise<string>;
}
```

> **Instance methods** are functions that operate on individual documents. You define them on the schema's `methods` object. They have access to the document via `this`.

> **Static methods** are functions that operate on the model itself, not on individual documents. You define them on the schema's `statics` object. They do not have access to the document instance.


> In Mongoose, you can embed one schema inside another using sub-documents.

**✅ Use embedding when:**
- The embedded data is strongly related to the parent (e.g., a post and its comments).
- You often need to retrieve the parent and child together.
- The embedded data is not reused elsewhere.

**❌ Avoid embedding when:**

- The embedded data will grow large (MongoDB document size limit is 16MB).
- The child data needs to be accessed independently or frequently without the parent.
- The data is shared or referenced across different parents.



#### ❓Schema in Mongoose
A Schema in Mongoose is a blueprint or structure definition for documents within a MongoDB collection. It defines:
- The fields (keys) a document will have,
- The data types of each field,
- Optional configurations like validation rules, default values, indexes, relationships, and more.

This gives structure and consistency to data stored in a schemaless MongoDB database.

#### Example Schema in Mongoose

```javascript
// models/Organization.js
const mongoose = require('mongoose');

const organizationSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Organization name is required'],
    minlength: [3, 'Minimum 3 characters required']
  },
  industry: {
    type: String,
    enum: ['tech', 'finance', 'education', 'healthcare'],
    default: 'tech'
  }
}, 
  {  timestamps: true,
     versionKey: false  // Disable __v field
}); 

const Organization = mongoose.model('Organization', organizationSchema);
module.exports = Organization;
```

Suppose you’re building an enterprise-grade application for managing users in a SaaS platform. You need to define a User schema that includes:
```javascript
// models/User.js
const mongoose = require('mongoose');

const addressSchema = new mongoose.Schema({
  country: { type: String, required: true, trim: true },
  city: { type: String, required: true, trim: true },
  street: { type: String, required: true, trim: true },
  zip: { type: String, required: true, trim: true }
}, 
  { _id: false }
); // _id: false prevents a separate _id for the sub-document

const userSchema = new mongoose.Schema({
  fullName: {
    type: String,
    required: [true, 'Full name is required'],
    minlength: [2, 'Name must be at least 2 characters'],
    trim: true
  },
  email: {
    type: String,
    required: [true, 'Email is required'],
    unique: true,
    lowercase: true,
    // custom validation
    validate: {
    validator: function(value) {
      return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)
    },
      message: function(props) {
        return `Email ${props.value} is not valid`
    }},
  },

  // OR use the built-in validator
  passwordHash: {
    type: String,
    required: true
  },
  role: {
    type: String,
    enum: ['admin', 'manager', 'user'],
    default: 'user'
  },
  isActive: {
    type: Boolean,
    default: true
  },
  lastLogin: {
    type: Date
  },
  address: {
    type: addressSchema,
    required: true
  }
}, {
  timestamps: true // Automatically adds createdAt and updatedAt fields
  versionKey: false // Disable __v field
  // You can also add other options like collection name, toJSON, toObject, etc.
});


/* -------------------------- Built-in Instance Method -------------------------- */
userSchema.methods.isAdmin = function () {
  return this.role === 'admin';
};

/* ----------------------------- Static Method ----------------------------- */
userSchema.statics.findActiveUsers = function () {
  return this.find({ isActive: true });
};


// instance method
userSchema.method("hashPassword", async function(plainPassword: string){
    const password = await bcrypt.hash(plainPassword, 10);
    return password;
});



// static method
userSchema.static("hashPassword", async function(plainPassword: string){
    const password = await bcrypt.hash(plainPassword, 10);
    return password;
});


// Pre Hooks

// Document Middleware
userSchema.pre("save", async function(next) {
    this.password = await bcrypt.hash(this.password, 10)
    next()
});

//Query Middleware
userSchema.pre("find", function(next) {
    console.log("Inside pre find hook.");
    next()
});


//Post Hook

//Document Middleware
userSchema.post('save', function (doc, next) {
    console.log(`${doc.email} has been saved`);
    next()
});

//Query Middleware
userSchema.post("findOneAndDelete", async function(doc, next) {
    if(doc) {
        console.log(doc);
        await User.deleteMany({ user: doc._id })
    }
    next()
});

// using virtual
userSchema.virtual("fullName").get(function() {
    return `${this.firstName} ${this.lastName}`;
});
```

> bcrypt is a library for hashing passwords. It is commonly used in Mongoose schemas to securely store user passwords. In the example above, the `hashPassword` method hashes the password before saving it to the database.

> Query middleware allows you to modify the query before it is executed. In the example above, the `pre` hook logs a message before executing a find query.

> Post middleware allows you to perform actions after a query has been executed. In the example above, the `post` hook logs a message after a document has been saved.

> Virtuals are properties that are not stored in the database but can be computed from other fields. In the example above, the `fullName` virtual combines the first and last names into a single property.

> **Note**: The `timestamps` option automatically adds `createdAt` and `updatedAt` fields to the schema, which are useful for tracking when documents were created and last updated.

> **Note**: The `versionKey` option disables the `__v` field, which is used by Mongoose to track document revisions.
 

Benefits of Using Mongoose Schema
- Validation at the application level.
- Predictable data structure across the app.
- Middleware support (e.g., hashing passwords before saving).
- Custom instance & static methods (extend schema behavior).
- Relationships via ref and populate.

#### ❓Model in Mongoose
In Mongoose, a model is a constructor function created from a schema. It represents a collection in the MongoDB database and provides an interface to interact with the documents in that collection (e.g., querying, inserting, updating, deleting).

Compile the Schema into a Model
```javascript
const User = mongoose.model("User", UserSchema):
module.exports = User;
```
This line tells Mongoose:
> “Take this `userSchema`, and bind it to a collection named `users` in the database.”


#### Zod Validation Before Mongoose
Zod is a TypeScript-first schema declaration and validation library. It allows you to define schemas for your data structures and validate them before passing them to Mongoose. This can help catch validation errors early in the development process.
```javascript
const { z } = require('zod');

const userSchemaZod = z.object({
  fullName: z.string().min(2),
  email: z.string().email(),
  age: z.number().min(18).max(65),
  role: z.enum(['admin', 'user', 'guest']),
  organization: z.string().min(1),
  isActive: z.boolean().optional()
});

module.exports = { userSchemaZod };
```

#### Creating a User with Zod + Mongoose
```javascript
const User = require('../models/User');
const Organization = require('../models/Organization');
const { userSchemaZod } = require('../validation/userValidation');

const createUser = async (req, res) => {
  try {
    // Step 1: Validate with Zod
    const parsedData = userSchemaZod.parse(req.body);

    // Step 2: Check if organization exists
    const org = await Organization.findById(parsedData.organization);
    if (!org) return res.status(404).json({ error: 'Organization not found' });

    // Step 3: Create user using Mongoose
    const user = new User(parsedData);
    await user.save();

    res.status(201).json(user);
  } catch (err) {
    if (err.name === 'ZodError') {
      return res.status(400).json({ validation: err.errors });
    }
    res.status(500).json({ error: err.message });
  }
};
module.exports = { createUser };
```

#### Population (Referencing Organization)
```javascript
const getUserWithOrganization = async (req, res) => {
  const user = await User.findById(req.params.id).populate('organization');
  res.json(user);
};
```
> The `populate` method allows you to replace the specified field (in this case, `organization`) with the actual document from the referenced collection, making it easier to work with related data.


#### Filtering, Sorting, Skip, Limit
```javascript
const getUsers = async (req, res) => {
  const { role, isActive, sortBy = 'createdAt', order = 'desc', page = 1, limit = 10 } = req.query;

  const filter = {};
  if (role) filter.role = role;
  if (isActive !== undefined) filter.isActive = isActive === 'true';

  const users = await User.find(filter)
    .sort({ [sortBy]: order === 'desc' ? -1 : 1 })
    .skip((page - 1) * limit)
    .limit(parseInt(limit));

  res.json(users);
};
```
> filtering allows you to retrieve only the documents that match certain criteria, sorting arranges the results in a specific order, skipping allows you to skip a certain number of documents, and limiting restricts the number of documents returned.
