# MongoDB Basic <a id="top"></a>

<!-- Date: 02/19/2022 -->

## Table of Contents

1. [What is MongoDB?](#what_is_mongodb)

   - [What is the MongoDB Database?](#chap1-what_is_the_mongob_database)
   - [How to install?](#chap1-how_to_install)
   - [How to use?](#chap1-how_to_use)
   - [What is MongoDB Atlas?](#chap1-what_is_mongodb_atlas)
   - [What is Compass?](#chap1-what_is_compass)

2. [Importing / Exporting data](#import_and_export_data)

   - [How does MongoDB store data?](#chap2-how_does_mongodb_store_data)
   - [Importing and Exporting Data](#chap2-importing_and_exporting_data)

3. [Commands with Mongo Shell](#commands_with_mongoshell)

   - [Basic commands](#chap3-basic_commands)
   - [Read documents](#chap3-read_documents)
   - [Insert documents](#chap3-insert_documents)
   - [Update documents](#chap3-update_documents)
   - [Delete data](#chap3-delete_data)

4. [Advanced CRUD Operations](#advanced_crud_operations)

   - [Comparison operators](#chap4-comparison_operators)
   - [Logic operators](#chap4-logic_operators)
   - [Expressive Query Operator](#chap4-expressive_query_operator)
   - [Array operator](#chap4-array_operator)
   - [Projection and $elemMatch](#chap4-projection_and_elemMatch)
   - [Array Operators and Sub-Documents](#chap4-array_operators_and_sub_documents)
   - [Other operators](#chap4-other_operators)
   - [Complex Update Object](#chap4-complex_update_object)

5. [Index and Aggregation Pipeline](#index_and_aggregation_pipeline)

   - [Aggregation Framework](#chap5-aggregation_framework)
   - [sort() and limit(), skip()](#chap5-sort_limit_skip)
   - [Introduction to Indexes](#chap5-introduction_to_indexes)
   - [Introduction to Data Modeling](#chap5-introduction_to_data_modeling)
   - [Upsert - Update or Insert?](#chap5-upsert)

---

## What is MongoDB? <a name="what_is_mongodb"></a>

### What is the MongoDB Database? <a name="chap1-what_is_the_mongob_database"></a>

- [**MongoDB**][mongodb] is a NoSQL document database. This means that data in MongoDB is stored as _documents_. These documents are in turn stored in what we call _collections_ of documents.

#### Terminology

- **_Database_** : &nbsp; A container for collections. This is the same as a database in SQL and usually each project will have its own database full of different collections

- **_Collection_** : &nbsp; A grouping of documents inside of a database. This is the same as a table in SQL and usually each type of data (users, posts, products) will have its own collection
- **_Document_** : &nbsp; A record inside of a collection. This is the same as a row in SQL and usually there will be one document per object in the collection. A document is also essentially just a JSON object.

  ```
  This is a document:
  {
      "name" : "Lakshmi",
      "title" : "Team Lead",
      "age" : 26
  }
  ```

- **_Field_** : &nbsp; A key value pair within a document. This is the same as a column in SQL. Each document will have some number of fields that contain information such as name, address, hobbies, etc. An important difference between SQL and MongoDB is that a field can contain values such as JSON objects, and arrays instead of just strings, number, booleans, etc.

_Structure of a MongoDB Database:_

<img alt="illustration" height="300px" src="https://media.geeksforgeeks.org/wp-content/uploads/20200127193216/mongodb-nosql-working.jpg" />

[Image Source](https://www.geeksforgeeks.org/what-is-mongodb-working-and-features/)

### How to install? <a name="chap1-how_to_install"></a>

1. [download MongoDB](https://www.mongodb.com/try/download/community?tck=docs_server)
2. [download MongoDB Shell](https://www.mongodb.com/try/download/shell?jmp=docs)
3. Add MongoDB binaries to the System PATH: If you add `C:\Program Files\MongoDB\Server\5.0\bin` to your System PATH you can omit the full path to the MongoDB Server binaries. You should also add the path to **_mongosh_** if you have not already done so.

### How to use? <a name="chap1-how_to_use"></a>

- Connect to the Atlas cluster: `mongo "mongodb+srv://<cluster>.lo58x.mongodb.net/myFirstDatabase" --username <username>` or `mongosh` to use local computer.
- Write code on the terminal of VS Code (recommend) or use `cmd` of Windows.

### What is MongoDB Atlas? <a name="chap1-what_is_mongodb_atlas"></a>

- [**MongoDB Atlas**][atlas] is a fully-managed _cloud database_ that handles all the complexity of deploying, managing, and healing your deployments on the cloud service provider of your choice (AWS , Azure, and Google Cloud Platform).
- MongoDB Atlas is the _best way_ to deploy, run, and scale MongoDB in the cloud. With Atlas, you’ll have a MongoDB database running with just a few clicks, and in just a few minutes.

#### Services

- manage the details of creating clusters.
- run and maintain database deployment.
- use cloud service provider of your choice.
- experiment with new tools and features.

#### Terminology

- **_Cluster_** : &nbsp; group of servers that store your data.
- **_Replica Set_** : &nbsp; a few connected machines that store the same data to ensure that if something happens to one of the machines the data will remain intact. Comes from the word replicate - to copy something.
- **_Instance_** : &nbsp; a single machine locally or in the cloud, running a certain software, in our case it is the MongoDB database.

### What is Compass? <a name="chap1-what_is_compass"></a>

- MongoDB has a great UI tool called [Compass](https://www.mongodb.com/try/download/compass)

<div align="right"> <a href="#top">Back to top</a> </div>

---

## Importing, Exporting Data <a name="import_and_export_data"></a>

### How does MongoDB store data? <a name="chap2-how_does_mongodb_store_data"></a>

- JSON and BSON

#### JSON (JavaScript Standard Object Notation)

- JSON format:

  ```
  {
    "name" : "Devi",
    "age": 19,
    "major": "Computer Science"
  }
  ```

- Pros:
  - user-friendly
  - readable
  - familier for many developers
- Cons:
  - json is text-based format, and text parsing is very slow
  - json's readable format is far from space efficient
  - Json only supports a limited number of basic data types

#### BSON (Binary JSON)

- Bridges the gap between binary representation and JSON format
- Optimized for:
  - speed
  - space
  - flexibility
- High performance
- General-purpose focus

#### Compare

|                  | JSON                           | BSON                                                                         |
| ---------------- | ------------------------------ | ---------------------------------------------------------------------------- |
| **Encoding**     | UTF-8 string                   | Binary                                                                       |
| **Data support** | String, Boolean, Number, Array | String, Boolean, Number (Integer, Long, Float, ...), Array, Date, Raw Binary |
| **Readability**  | human and machine              | only machine                                                                 |

#### Sumary

- MongoDB stores data in BSON, internally and over the network.
- JSON can be natively stored and retrieved in MongoDB.
- BSON provides additional features like speed and flexibility.

### Importing and Exporting Data <a name="chap2-importing_and_exporting_data"></a>

- Overview

  |     | JSON        | BSON         |
  | --- | ----------- | ------------ |
  |     | mongoimport | mongorestore |
  |     | mongoexport | mongodump    |

#### Export

- Export data in BSON :

  ```
  mongodump --uri "<Atlas Cluster URI>"
  ```

- Export data in JSON :

  ```
  mongoexport --uri "<Atlas Cluster URI>"
              --collection=<collection name>
              --out=<filename>.json
  ```

#### URI String

- Uniform Resource Identifier
- In MongoDB 's case, we are using an _srv string_ to connect to your Atlas cluster, which is a more specific connection string format that is used to establish a secure connection between your application and a MongoDB instance.

#### Import

- Import data in BSON dump :

  ```
  mongorestore --uri "<Atlas Cluster URI>"
              --drop dump
  ```

- Import data in JSON :

  ```
  mongoimport --uri "<Atlas Cluster URI>"
              --drop=<filename>.json
  ```

#### Sample example

```
mongodump --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"

mongoexport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --collection=sales --out=sales.json

mongorestore --uri "mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies"  --drop dump

mongoimport --uri="mongodb+srv://<your username>:<your password>@<your cluster>.mongodb.net/sample_supplies" --drop sales.json
```

<div align="right"> <a href="#top">Back to top</a> </div>

---

## Commands with Mongo shell <a name="commands_with_mongoshell"></a>

### Basic commands <a name="chap3-basic_commands"></a>

- `mongosh` : &nbsp; Open a connection to your local MongoDB instance. All other commands will be run within this mongosh connection.
- `show dbs` : &nbsp; Show all databases in the current MongoDB instance.
- `use <dbname>` : &nbsp; Switch to the database provided by dbname.
- `db` : &nbsp; Show current database name.
- `cls` : &nbsp; Clear the terminal screen.
- `show collections` : &nbsp; Show all collections in the current database.
- `db.dropDatabase()` : &nbsp; Delete the current database.
- `exit` : &nbsp; Exit the mongosh session.

### Read documents <a name="chap3-read_documents"></a>

> Each of these commands is run on a specific collection \
> `db.<collectionName>.<command>`

- `find()` : &nbsp; Get all documents

  ```
  db.users.find()
  ==> Get all users
  ```

- `find(<filterObject>)` : &nbsp; Find all documents that match the filter object

  ```
  db.users.find({ name: "Kyle" })
  ==> Get all users with the name Kyle

  db.users.find({ "address.street": "123 Main St" })
  ==> Get all users whose adress field has a street field with the value 123 Main St
  ```

- `find(<filterObject>, <selectObject>)` : &nbsp; Find all documents that match the filter object but only return the field specified in the select object

  ```
  db.users.find({ name: "Kyle" }, { name: 1, age: 1 })
  ==> Get all users with the name Kyle but only return their name, age, and _id

  db.users.find({}, { age: 0 })
  ==> Get all users and return all columns except for age

  ```

- `findOne()` : &nbsp; The same as find, but only return the first document that matches the filter object

  ```
  db.users.findOne({ name: "Kyle" })
  ==> Get the first user with the name Kyle
  ```

- `countDocuments()` : &nbsp; Return the count of the documents that match the filter object passed to it

  ```
  db.users.countDocument({}) return the same result as db.users.count()

  db.users.countDocuments({ name: "Kyle" })
  ==> Get the number of users with the name Kyle
  ```

- `count()` : &nbsp; returns the number of documents that match the find query

  ```
  db.zips.find({"state": "NY"}).count()
  => Return the count of the documents with state is NY
  ```

- `pretty()` : &nbsp; formats the documents in the cursor, looks better

  ```
  db.zips.find({"state": "NY", "city": "ALBANY"}).pretty()
  ```

### Insert documents <a name="chap3-insert_documents"></a>

> Each of these commands is run on a specific collection \
> `db.<collectionName>.<command>`

- **"\_id"** :
  - unique identifier for a document in a collection.
  - required in every MongoDB document.
- **ObjectId()** :

  - Default value for the \_id field unless otherwise specified.
  - Examples :

  ```
  "_id": ObjectId("5ec5f1b710ca9222e6a46cab")
  "_id": "710ca922"
  "_id": "101-EXG-27"
  ```

- Identical documents can exist in the same collection as long as their
  \_id values are different.

- MongoDB has schema validation functionality allows you to enforce
  document structure.

- `insert()` : &nbsp; Create a or many new document inside the specified collection

  ```
  db.inspections.insert({
        "_id" : ObjectId("56d61033a378eccde8a8354f"),
        "id" : "10021-2015-ENFO",
        "certificate_number" : 9278806,
        "business_name" : "ATLIXCO DELI GROCERY INC.",
        "date" : "Feb 20 2015",
        "result" : "No Violation Issued",
        "sector" : "Cigarette Retail Dealer - 127",
        "address" : {
                "city" : "RIDGEWOOD",
                "zip" : 11385,
                "street" : "MENAHAN ST",
                "number" : 1712
          }
    })

  db.users.insert([{ age: 26 }, { age: 20 }])
  ```

- `insertOne()` : &nbsp; Create a new document inside the specified collection

  ```
  db.users.insertOne({ name: "Kyle" })
  ==> Add a new document with the name of Kyle into the users collection

  ```

- `insertMany()` : &nbsp; Create multi new documents inside a specific collection

  ```
  db.users.insertMany([{ age: 26 }, { age: 20 }])
  ==> Add two new documents with the age of 26 and 20 into the users collection
  ```

- ` insert() order` :

  - Optional. If **true**, perform an ordered insert of the documents in the array, and if an error occurs with one of documents, MongoDB will return without processing the remaining documents in the array.

    ```
    db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },{ "_id": 3, "test": 3 }])
    =>  The second document that fails (with the same _id as document 1), MongoDB will return, document 3 will not be checked
    ```

  - If **false**, perform an unordered insert, and if an error occurs with one of documents, continue processing the remaining documents in the array.

    ```
    db.inspections.insert([{ "_id": 1, "test": 1 },{ "_id": 1, "test": 2 },{ "_id": 3, "test": 3 }],{ "ordered": false })
    => all documents are processed even if there is a document error
    ```

  - Defaults to **true**.

    ```
    db.inspections.insert([ { "test": 1 }, { "test": 2 }, { "test": 3 } ])
    => insert default by order in array
    ```

### Update documents <a name="chap3-update_documents"></a>

> Each of these commands is run on a specific collection \
> `db.<collectionName>.<command>`

- `updateOne()` : &nbsp; Update the first document that matches the filter object with the data passed into the second parameter which is the update object

  ```
  db.users.updateOne({ age: 20 }, { $set: { age: 21 } })
  ==> Update the first user with an age of 20 to the age of 21
  ```

- `updateMany()` : &nbsp; Update all documents that matches the filter object with the data passed into the second parameter which is the update object

  ```
  db.users.updateMany({ age: 12 }, { $inc: { age: 3 } })
  ==> Update all users with an age of 12 by add 3 to their age
  ```

- `replaceOne()` : &nbsp; Replace the first document that matches the filter object with the exact object passed as the second parameter. This will completely overwrite the entire object and not just update individual fields.

  ```
  db.users.replaceOne({ age: 12 }, { age: 13 })
  ==> Replace the first user with an age of 12 with an object that has the age of 13 as its only field
  ```

### Delete data <a name="chap3-delete_data"></a>

> Each of these commands is run on a specific collection <br> `db.<collectionName>.<command>`

- `deleteOne()` : &nbsp; Delete the first document that matches the filter object

  ```
  db.users.deleteOne({ age: 20 })
  ==> Delete the first user with an age of 20
  ```

- `deleteMany()` : &nbsp; Delete all documents that matches the filter object

  ```
  db.users.deleteMany({ age: 12 })
  ==> Delete all users with an age of 12
  ```

- `drop()` : &nbsp; drop collection

  ```
  db.inspection.drop()
  => Drop the inspection collection.
  ```

<div align="right"> <a href="#top">Back to top</a> </div>

---

## Advanced CRUD Operations <a name="advanced_crud_operations"></a>

### Comparison operators <a name="chap4-comparison_operators"></a>

> all use the same syntax of field: `{ <field>: { <operator>: <value> } }`

| Query Operators | Meaning                  | Example                                    | Explain examples                                      |
| --------------- | ------------------------ | ------------------------------------------ | ----------------------------------------------------- |
| `$eq`           | Equal to                 | `db.users.find({ name: { $eq: "Kyle" } }`  | Get all users with the name Kyle                      |
| `$ne`           | Not Equal to             | `db.users.find({ name: { $ne: "Kyle" } })` | Get all users with a name other than Kyle             |
| `$gt`           | Greater than             | `db.users.find({ age: { $gt: 12 } })`      | Get all users with an age greater than 12             |
| `$gte`          | Greater than or equal to | `db.users.find({ age: { $gte: 15 } })`     | Get all users with an age greater than or equal to 15 |
| `$lt`           | Less than                | `db.users.find({ age: { $lt: 12 } })`      | Get all users with an age less than 12                |
| `$lte`          | Less than or equal to    | `db.users.find({ age: { $lte: 15 } })`     | Get all users with an age less than or equal to 15    |

### Logic operators <a name="chap4-logic_operators"></a>

- `$and` : Check that multiple conditions are all true

  - Syntax: `{ $and: [ { <expression1> }, { <expression2> } , ... , { <expressionN> } ] }`
  - Example:

    ```
    db.users.find({ $and: [{ age: 12 }, { name: "Kyle" }] })
    ==> Get all users that have an age of 12 and the name Kyle

    db.users.find({ age: 12, name: "Kyle" })
    ==> This is an alternative way to do the same thing. Generally you do not need $and.
    ```

- `$or` : Check that one of multiple conditions is true

  - Syntax: `{ $or: [ { <expression1> }, { <expression2> }, ... , { <expressionN> } ] }`
  - Example:

    ```
    db.users.find({ $or: [{ age: 12 }, { name: "Kyle" }] })
    ==> Get all users with a name of Kyle or an age of 12
    ```

- `$nor` : Joins query clauses with a logical NOR returns all documents that fail to match both clauses.

  - Syntax: `{ $nor: [ { <expression1> }, { <expression2> }, ... { <expressionN> } ] }`
  - Example:

  ```
  db.inventory.find( { $nor: [ { price: 1.99 }, { sale: true } ]  } )
  ==> This query will return all documents that:
    - contain the price field whose value is not equal to 1.99 and contain the sale field whose value is not equal to true or
    - contain the price field whose value is not equal to 1.99 but do not contain the sale field or
    - do not contain the price field but contain the sale field whose value is not equal to true or
    - do not contain the price field and do not contain the sale field
  ```

- `$not` : Inverts the effect of a query expression and returns documents that do not match the query expression.

  - Syntax: `{ field: { $not: { <operator-expression> } } }`
  - Example:

  ```
  db.users.find({ name: { $not: { $eq: "Kyle" } } })
  ==> Get all users with a name other than Kyle
  ```

### Expressive Query Operator <a name="chap4-expressive_query_operator"></a>

- `$expr` : Do comparisons between different fields

  ```
  db.users.find({ $expr: { $gt: ["$balance", "$debt"] } })
  ==> Get all users that have a balance that is greater than their debt


  db.trips.find({ "$expr": { "$and": [ { "$gt": [ "$tripduration", 1200 ]},
                           { "$eq": [ "$end station id", "$start station id" ]}
                       ]}}).count()
  ==> Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station:
  ```

### Array operator <a name="chap4-array_operator"></a>

- `{<array field> : { "$size": <number>}}` : Returns a cursor with all documents where the specified array field is exactly the given length.
- `{<array field> : { "$all": <array>}}` : Returns a cursor with all documents in which the specified array field contains all the given elements regardless of their order in the array.
- **Querying an array field using** :
  - An array returns only exact array matches
  - A single element will return all documents where the specified array field contains this given element.
- Example: Find all documents with exactly 20 _amenities_ which include all the amenities listed in the query array:

  ```
  db.listingsAndReviews.find({ "amenities": {
                                    "$size": 20,
                                    "$all": [ "Internet", "Wifi",  "Kitchen",
                                            "Heating", "Family/kid friendly",
                                            "Washer", "Dryer", "Essentials",
                                            "Shampoo", "Hangers",
                                            "Hair dryer", "Iron",
                                            "Laptop friendly workspace" ]
                                          }
                              }).pretty()
  ```

### Projection and $elemMatch <a name="chap4-projection_and_elemMatch"></a>

> Projection

- Syntax: `db.<collection>.find({ <query> }, { <projection> })`
- Specify the fields that are or should not be displayed in the result pointer
- 1 and 0 do not combine in the same projection, except `{ "_id: 0", <field>: 1 }`
- Example: _Find all documents that have **Wifi** as one of the **amenities** of just including **price** and **address** in the resulting cursor_

  ```
  db.listingsAndReviews.find({ "amenities": "Wifi" }, { "price": 1, "address": 1, "_id": 0 }).pretty()
  ```

> $elemMatch

- Syntax: `{<field> : { "$elemMatch": { <field>: <value> }}}`
- Matches documents that contain an array field with at least one element that matches the specified query criteria.
  or
  Projects only the array elements with at least one element that matches the specified criteria.
- Example 1: _Find all documents where the student in class **431** received a grade higher than **85** for any type of assignment_

  ```
  db.grades.find({ "class_id": 431 },
                { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
              }).pretty()
  ```

- Example 2: _Find all documents where the student had an extra credit score:_

  ```
  db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
                }).pretty()
  ```

### Array Operators and Sub-Documents <a name="chap4-array_operators_and_sub_documents"></a>

- MQL(MongoDB Query Language) uses dot-notation to specify the address of nested elements in a document
- To use dot-notation in arrays specify the position of the element in the array.
- Syntax: `db.collection.find({"field 1.other field.also a field": "value"})`
- Example 1: _returns the names and addresses of all listings from the **sample_airbnb.listingsAndReviews** collection where the first widget in **amenities** is \"Internet"_

  ```
  db.listingsAndReviews.find({ "amenities.0": "Internet" }, { "name": 1, "address": 1 }).pretty()
  ==>
  ```

- Example 2: _Let's see how many of them are CEOs whose first name is **Mark** and who are listed as the first relationship in this array for their company's entry._

  ```
  db.companies.find({ "relationships.0.person.first_name": "Mark",
                      "relationships.0.title": { "$regex": "CEO" } },
                    { "name": 1 }).count()
  ```

- To learn about the $regex operator [check out this documentation page][https://docs.mongodb.com/manual/reference/operator/query/regex/]

### Other operators <a name="chap4-other_operators"></a>

- `$in` : Check if a value is one of many values

  ```
  db.users.find({ name: { $in: ["Kyle", "Mike"] } })
  ==> Get all users with a name of Kyle or Mike
  ```

- `$nin` : Check if a value is none of many values

  ```
  db.users.find({ name: { $nin: ["Kyle", "Mike"] } })
  ==> Get all users that do not have the name Kyle or Mike
  ```

- `$exists` : Check if a field exists

  ```
  db.users.find({ name: { $exists: true } })
  ==> Get all users that have a name field
  ```

### Complex Update Object <a name="chap4-complex_update_object"></a>

> Any combination of the below can be use inside an update object to make complex updates

- `$set` : &nbsp; Update only the fields passed to `$set`. This will not affect any fields not passed to `$set`.

  ```
  db.users.updateOne({ age: 12 }, { $set: { name: "Hi" } })
  ==> Update the name of the first user with the age of 12 to the value Hi
  ```

- `$inc` : &nbsp; Increment the value of the field by the amount given.

  ```
  db.users.updateOne({ age: 12 }, { $inc: { age: 2 } })
  ==> Add 2 to the age of the first user with the age of 12
  ```

- `$rename` : &nbsp; Rename a field

  ```
  db.users.updateMany({}, { $rename: { age: "years" } })
  ==> Rename the field age to years for all users
  ```

- `$unset` : &nbsp; Remove a field

  ```
  db.users.updateOne({ age: 12 }, { $unset: { age: "" } })
  ==> Remove the age field from the first user with an age of 12
  ```

- `$push` : &nbsp; Add a value to an array field

  ```
  db.users.updateMany({}, { $push: { friends: "John" } })
  ==> Add John to the friends array for all users
  ```

- `$pull` : &nbsp; Remove a value from an array field

  ```
  db.users.updateMany({}, { $pull: { friends: "Mike" } })
  ==> Remove Mike from the friends array for all users
  ```

<div align="right"> <a href="#top">Back to top</a> </div>

---

## Index and Aggregation Pipeline <a name="index_and_aggregation_pipeline"></a>

### Aggregation Framework <a name="chap5-aggregation_framework"></a>

- In its simplest form, another way to query data in MongoDB
- Example 1:

  - _Find all documents that have **Wifi** as one of the amenities. Only include **price** and **address** in the resulting cursor._

  ```
  db.listingsAndReviews.find({ "amenities": "Wifi" },
                            { "price": 1, "address": 1, "_id": 0 }).pretty()
  ```

  - _Using the aggregation framework find all documents that have **Wifi** as one of the amenities. Only include **price** and **address** in the resulting cursor._

  ```
  db.listingsAndReviews.aggregate([
                                    { "$match": { "amenities": "Wifi" } },
                                    { "$project": { "price": 1, "address": 1, "_id": 0 }}
                                  ]).pretty()
  ```

- `$group` :

  - Groups input documents by the specified \_id expression and for each distinct grouping, outputs a document. The \_id field of each output document contains the unique group by value. The output documents can also contain computed fields that hold the values of some accumulator expression.
  - Syntax:
    ```
    {
    $group:
      {
        _id: <expression>, // Group By Expression
        <field1>: { <accumulator1> : <expression1> },
        ...
      }
    }
    ```
  - Example: _Project only the **address** field value for each document, then group all documents into one document per **address.country** value, and count one for each document in each group._

    ```
    db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])

    Result:

    { "_id" : "Brazil", "count" : 606 }
    { "_id" : "Canada", "count" : 649 }
    { "_id" : "Turkey", "count" : 661 }
    { "_id" : "Hong Kong", "count" : 600 }
    { "_id" : "Australia", "count" : 610 }
    { "_id" : "China", "count" : 19 }
    { "_id" : "United States", "count" : 1222 }
    { "_id" : "Portugal", "count" : 555 }
    { "_id" : "Spain", "count" : 633 }
    ```

### sort() and limit(), skip() <a name="chap5-sort_limit_skip"></a>

- _sort_ : &nbsp; Sort the results of a find by the given fields

  ```
  db.users.find().sort({ name: 1, age: 1 })
  ==> Get all users sorted by name in alphabetical order and then if any names are the same sort by age in reverse order
  ```

- _limit_ : &nbsp; Only return a set number of documents

  ```
  db.users.find().limit(2)
  ==> Only return the first 2 users
  ```

- _skip_ : &nbsp; Skip a set number of documents from the beginning

  ```
  db.users.find().skip(4)
  ==> Skip the first 4 users when returning results. This is great for pagination when combined with limit.
  ```

- Note:

```
db.zips.find().sort({ "pop": 1, "city": -1 }).limit(3).pretty()
or
db.zips.find().limit(3).sort({ "pop": 1, "city": -1 }).pretty()

==> return the same result, sort pop ascending (0 -> infinity), city descending ("Z" -> "A"), and limit 3 results
```

### Introduction to Indexes <a name="chap5-introduction_to_indexes"></a>

> - Make queries even more efficient
> - Are one of the most impactful ways to improve query performance

- Indexes support the efficient execution of queries in MongoDB.
- Without indexes, MongoDB must perform a collection scan, scan every document in a collection, to select those documents that match the query statement. If an appropriate index exists for a query, MongoDB can use the index to limit the number of documents it must inspect.
- In a database – indexes is special data structure that stores a small portion of the collection's data set in an easy to traverse form.

- **When to index?** support your queries

- Example:

  - If I'm running queries on these trips collection and find that I **often** query by the **birth year**,

    > query 1: `db.trips.find({ "birth year": 1989 })` \
    > query 2: `db.trips.find({"start station id": 476}).sort("birth year": 1)`

  - so let's create an **index** base on the birth year values that supports these queries.

    ```
    db.trips.createIndex({"birth year": 1})
    ==> This command creates an index on the birth year field in increasing order.
    ```

  - Now:
    - **query 1**: MongoDB doesn't have to look at every document to get the needed results, It will just go directly to where the 1989 documents live and retrieve them.
    - **query 2**: MongoDB will still have to look through all the documents to find those where the start station ID is 476. But the good news is that it can use the index that we created in order to retrieve those results in sorted order by birth year, so there won't be a need to sort the cursor after the data is filtered
  - However, _not perfect for query 2_. We need to use a **compound index**, which ex is an index on multiple fields. \
    `db.trips.createIndex({"start station id": 1,"birth year": 1})`

### Introduction to Data Modeling <a name="chap5-introduction_to_data_modeling"></a>

- **_Data modeling_** - a way to organize fields in a document to support your application performance and querying capabilities.
- **Rule** : data is stored in the way that it is used. \
  --> it decision that you make about the _shape of your document_ and the _number of your collections_.
- When data modeling with MongoDB:
  - Data that is **used** together should be **stored** together.
  - Evolving application implies an evolving data model

### Upsert - Update or Insert? <a name="chap5-upsert"></a>

> Defaults to false

- Everything in MQL that is used to **locate** a document in a collection can also be used to modify this document.

```
db.collection.updateOne({<query to locate>},{<update>})
```

- Upsert is a hybrid of update and insert, it should **only be used when it is needed**.

```
db.collection.updateOne({<query>},{<update>},{"upsert":true})
```

- If **upsert is true**, if there are matching documents then **update** the matched document, otherwise **insert** a new document

---

> Source: \
> [https://university.mongodb.com/](https://university.mongodb.com/) \
> [Web Dev Simplified\_](https://youtu.be/ofme2o29ngU)

---

[mongodb]: https://www.mongodb.com/
[atlas]: https://www.mongodb.com/cloud/atlas/register
