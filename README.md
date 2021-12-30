# MongoDB Cheat Sheet <a id="top"></a>

#### Date: 30/12/2021

---

## Table of Contents

[How to install](#how-to-install) \
[How to use](#how-to-use) \
[Terminology](#terminology) \
[Basic Commands](#basic-commands) \
[Create](#create-commands) \
[Read](#read-commands) \
[Update](#update-commands) \
[Delete](#delete-commands) \
[Complex Filter Object](#complex-filter-object) \
[Complex Update Object](#complex-update-object) \
[Read Modifiers](#read-modifiers)

---

## How to install? <a name="how-to-install"></a>

1. [download MongoDB](https://www.mongodb.com/try/download/community?tck=docs_server)
2. [download MongoDB Shell](https://www.mongodb.com/try/download/shell?jmp=docs)
3. Add MongoDB binaries to the System PATH: If you add `C:\Program Files\MongoDB\Server\5.0\bin` to your System PATH you can omit the full path to the MongoDB Server binaries. You should also add the path to **_mongosh_** if you have not already done so.

## How to use? <a name="how-to-use"></a>

- Write code on the terminal of VS Code (recommend)
- Or use `cmd` of Windows

## Terminology <a name="terminology"></a>

- **_Database_** : &nbsp; A container for collections. This is the same as a database in SQL and usually each project will have its own database full of different collections
- **_Collection_** : &nbsp; A grouping of documents inside of a database. This is the same as a table in SQL and usually each type of data (users, posts, products) will have its own collection
- **_Document_** : &nbsp; A record inside of a collection. This is the same as a row in SQL and usually there will be one document per object in the collection. A document is also essentially just a JSON object.
- **_Field_** : &nbsp; A key value pair within a document. This is the same as a column in SQL. Each document will have some number of fields that contain information such as name, address, hobbies, etc. An important difference between SQL and MongoDB is that a field can contain values such as JSON objects, and arrays instead of just strings, number, booleans, etc.

<div align="right"> <a href="#top">Về đầu trang</a> </div>

## Basic Commands <a name="basic-commands"></a>

- `mongosh` : &nbsp; Open a connection to your local MongoDB instance. All other commands will be run within this mongosh connection.
- `show dbs` : &nbsp; Show all databases in the current MongoDB instance.
- `use <dbname>` : &nbsp; Switch to the database provided by dbname.
- `db` : &nbsp; Show current database name.
- `cls` : &nbsp; Clear the terminal screen.
- `show collections` : &nbsp; Show all collections in the current database.
- `db.dropDatabase()` : &nbsp; Delete the current database.
- `exit` : &nbsp; Exit the mongosh session.

<div align="right"> <a href="#top">Về đầu trang</a> </div>

## Create <a name="create-commands"></a>

> Each of these commands is run on a specific collection <br> `db.<collectionName>.<command>`

- _insertOne_ : &nbsp; Create a new document inside the specified collection

  ```
  db.users.insertOne({ name: “Kyle” })
  ==> Add a new document with the name of Kyle into the users collection
  ```

- _insertMany_ : &nbsp; Create multi new documents inside a specific collection

  ```
  db.users.insertMany([{ age: 26 }, { age: 20 }])
  ==> Add two new documents with the age of 26 and 20 into the users collection
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Read <a name="read-commands"></a>

> Each of these commands is run on a specific collection <br> `db.<collectionName>.<command>`

- _find_ : &nbsp; Get all documents

  ```
  db.users.find()
  ==> Get all users
  ```

- _find(\<filterObject>)_ : &nbsp; Find all documents that match the filter object

  ```
  db.users.find({ name: “Kyle” })
  ==> Get all users with the name Kyle

  db.users.find({ “address.steet”: “123 Main St” })
  ==> Get all users whose adress field has a street field with the value 123 Main St
  ```

- _find(\<filterObject>, \<selectObject>)_ : &nbsp; Find all documents that match the filter object but only return the field specified in the select object

  ```
  db.users.find({ name: “Kyle” }, { name: 1, age: 1 })
  ==> Get all users with the name Kyle but only return their name, age, and _id

  db.users.find({}, { age: 0 })
  ==> Get all users and return all columns except for age

  ```

- _findOne_ : &nbsp; The same as find, but only return the first document that matches the filter object

  ```
  db.users.findOne({ name: “Kyle” })
  ==> Get the first user with the name Kyle
  ```

- _countDocuments_ : &nbsp; Return the count of the documents that match the filter object passed to it

  ```
  db.users.countDocuments({ name: “Kyle” })
  ==> Get the number of users with the name Kyle
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Update <a name="update-commands"></a>

> Each of these commands is run on a specific collection <br> `db.<collectionName>.<command>`

- _updateOne_ : &nbsp; Update the first document that matches the filter object with the data passed into the second parameter which is the update object

  ```
  db.users.updateOne({ age: 20 }, { $set: { age: 21 } })
  ==> Update the first user with an age of 20 to the age of 21
  ```

- _updateMany_ : &nbsp; Update all documents that matches the filter object with the data passed into the second parameter which is the update object

  ```
  db.users.updateMany({ age: 12 }, { $inc: { age: 3 } })
  ==> Update all users with an age of 12 by add 3 to their age
  ```

- _replaceOne_ : &nbsp; Replace the first document that matches the filter object with the exact object passed as the second parameter. This will completely overwrite the entire object and not just update individual fields.

  ```
  db.users.replaceOne({ age: 12 }, { age: 13 })
  ==> Replace the first user with an age of 12 with an object that has the age of 13 as its only field
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Delete <a name="delete-commands"></a>

> Each of these commands is run on a specific collection <br> `db.<collectionName>.<command>`

- _deleteOne_ : &nbsp; Delete the first document that matches the filter object

  ```
  db.users.deleteOne({ age: 20 })
  ==> Delete the first user with an age of 20
  ```

- _deleteMany_ : &nbsp; Delete all documents that matches the filter object

  ```
  db.users.deleteMany({ age: 12 })
  ==> Delete all users with an age of 12
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Complex Filter Object <a name="complex-filter-object"></a>

> Any combination of the below can be use inside a filter object to make complex queries

- _$eq_ : &nbsp; Check for equality

  ```
  db.users.find({ name: { $eq: “Kyle” } })
  ==> Get all users with the name Kyle
  ```

- _$ne_ : &nbsp; Check for not equal

  ```
  db.users.find({ name: { $ne: “Kyle” } })
  ==> Get all users with a name other than Kyle
  ```

- _$gt / $gte_ : &nbsp; Check for greater than and greater than or equal to

  ```
  db.users.find({ age: { $gt: 12 } })
  ==> Get all users with an age greater than 12

  db.users.find({ age: { $gte: 15 } })
  ==> Get all users with an age greater than or equal to 15
  ```

- _$lt / $lte_ : &nbsp; Check for less than and less than or equal to

  ```
  db.users.find({ age: { $lt: 12 } })
  ==> Get all users with an age less than 12

  db.users.find({ age: { $lte: 15 } })
  ==> Get all users with an age less than or equal to 15
  ```

- _$in_ : &nbsp; Check if a value is one of many values

  ```
  db.users.find({ name: { $in: [“Kyle”, “Mike”] } })
  ==> Get all users with a name of Kyle or Mike
  ```

- _$nin_ : &nbsp; Check if a value is none of many values

  ```
  db.users.find({ name: { $nin: [“Kyle”, “Mike”] } })
  ==> Get all users that do not have the name Kyle or Mike
  ```

- _$and_ : &nbsp; Check that multiple conditions are all true

  ```
  db.users.find({ $and: [{ age: 12 }, { name: “Kyle” }] })
  ==> Get all users that have an age of 12 and the name Kyle

  db.users.find({ age: 12, name: “Kyle” })
  ==> This is an alternative way to do the same thing. Generally you do not need $and.
  ```

- _$or_ : &nbsp; Check that one of multiple conditions is true

  ```
  db.users.find({ $or: [{ age: 12 }, { name: “Kyle” }] })
  ==> Get all users with a name of Kyle or an age of 12
  ```

- _$not_ : &nbsp; Negate the filter inside of $not

  ```
  db.users.find({ name: { $not: { $eq: “Kyle” } } })
  ==> Get all users with a name other than Kyle
  ```

- _$exists_ : &nbsp; Check if a field exists

  ```
  db.users.find({ name: { $exists: true } })
  ==> Get all users that have a name field
  ```

- _$expr_ : &nbsp; Do comparisons between different fields

  ```
  db.users.find({ $expr: { $gt: [“$balance”, “$debt”] } })
  ==> Get all users that have a balance that is greater than their debt
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Complex Update Object <a name="complex-update-object"></a>

> Any combination of the below can be use inside an update object to make complex updates

- _$set_ : &nbsp; Update only the fields passed to $set. This will not affect any fields not passed to $set.

  ```
  db.users.updateOne({ age: 12 }, { $set: { name: “Hi” } })
  ==> Update the name of the first user with the age of 12 to the value Hi
  ```

- _$inc_ : &nbsp; Increment the value of the field by the amount given.

  ```
  db.users.updateOne({ age: 12 }, { $inc: { age: 2 } })
  ==> Add 2 to the age of the first user with the age of 12
  ```

- _$rename_ : &nbsp; Rename a field

  ```
  db.users.updateMany({}, { $rename: { age: “years” } })
  ==> Rename the field age to years for all users
  ```

- _$unset_ : &nbsp; Remove a field

  ```
  db.users.updateOne({ age: 12 }, { $unset: { age: “” } })
  ==> Remove the age field from the first user with an age of 12
  ```

- _$push_ : &nbsp; Add a value to an array field

  ```
  db.users.updateMany({}, { $push: { friends: “John” } })
  ==> Add John to the friends array for all users
  ```

- _$pull_ : &nbsp; Remove a value from an array field

  ```
  db.users.updateMany({}, { $pull: { friends: “Mike” } })
  ==> Remove Mike from the friends array for all users
  ```

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

## Read Modifiers <a name="read-modifiers"></a>

> Any combination of the below can be added to the end of any read operation

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

  <div align="right"> <a href="#top">Về đầu trang</a> </div>

---

_Source: Web Dev Simplified_
