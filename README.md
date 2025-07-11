<!-- markdownlint-disable MD012 MD026 MD001 MD022 MD032 MD029 MD019 MD034 MD031 MD047 MD040 MD009 MD058 MD024  -->

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
| Performance            | Optimized for large datasets      | Optimized for complex queries    |


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
   - `$match`: Filters documents based on specified criteria
   - `$group`: Groups documents by a specified field and performs aggregations
   - `$sort`: Sorts documents based on specified fields
   - `$project`: Reshapes documents by including or excluding fields
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

