# MongoDB vs SQL: A Comparison of Migrations and Data Handling

## Introduction

When it comes to choosing a database management system, one of the fundamental considerations is the approach to data storage and retrieval. In this article, we will compare MongoDB and SQL (Structured Query Language) databases, specifically focusing on their migration capabilities and data handling methods. We will explore the advantages and disadvantages of each approach to help you make an informed decision for your project.

## Migrations and Adding Fields

### MongoDB

In MongoDB, performing migrations and adding fields requires running a script and querying the database to update all documents.

**Cons:**
1. **Incomplete Updates**: There is a possibility of missing some documents during the query, leading to incomplete updates.
2. **Schema Variability**: Due to the flexible nature of JSON in MongoDB, documents can have different attributes, making it challenging to query and account for edge cases.
3. **Potential Failures**: The migration operation can fail, resulting in the aforementioned issues.

**Solution: Defining a Schema**

To address these challenges, MongoDB introduced a "Schema Validator" that can be applied to a collection. A collection is a group of documents, each of which is a JSON file instance. With the schema validator, whenever a document is inserted or updated, it is checked against the defined schema.

However, modifying existing data to align with the schema is less straightforward compared to adding new columns in SQL databases.

**Mongoose: An Object-Data-Mapping (ODM) Library**

To simplify the process of defining and validating schemas in MongoDB, developers often use Mongoose, an ODM library for MongoDB in Node.js. Mongoose allows you to define schemas for MongoDB documents, resulting in automated validation, casting, and other convenient features.

It's important to note that while MongoDB allows data to take any form, Mongoose imposes restrictions. Once a schema is defined in Mongoose:

1. **Immutable Schema**: The schema cannot be changed, making it essential to design the schema carefully from the beginning.
2. **Incompatibility with Database Changes**: Modifying MongoDB at the database level (not through Mongoose), such as adding an attribute, renders the document invisible in Mongoose due to schema mismatches. This can lead to data loss and requires manual migration scripts.

> **_NOTE:_** Mongoose acts as a layer over MongoDB, interacting with it at the application level rather than directly at the database level.

### SQL

Migrations in SQL databases are relatively simple and safe. Adding a new field can be done with a straightforward SQL statement like:

```sql
ALTER TABLE users ADD COLUMN address VARCHAR;
```
Compared to the workarounds required in MongoDB, this approach provides a higher level of safety and ease.

Moreover, since schemas are already defined when using Mongoose with MongoDB, using SQL and defining tables does not impose additional restrictions beyond what is already present in Mongoose. SQL databases add an extra layer of safety in terms of data handling and usage.

## Data Fetching

Consider an example where you have a "users" collection and a "comments" collection. Each document in the "comments" collection has a "userId" field that refers to a document in the "users" collection.

Now, let's say you want to display a comment along with the name of the user who posted it. In MongoDB, retrieving this information involves multiple queries due to the lack of JOIN operations.

Firstly, you would query the "comments" collection to get the comment details using the comment ID:

```javascript
const comment = db.comments.findOne({ _id: commentId });
```

However, the comment document only contains the "userId" field, not the user details. To retrieve the user details, you need to make a second query to the "users" collection using the retrieved "userId":

```javascript
const user = db.users.findOne({ _id: comment.userId });
```

Now you have the comment and the user details, and you can combine or use them as needed. However, it's important to note that this approach of fetching related data from multiple collections can result in repetitive and less efficient querying, especially when dealing with large datasets or complex relationships.

In contrast, SQL databases offer the JOIN operation, allowing you to retrieve related data in a single query by joining the "comments" and "users" tables based on a common field such as the user ID. This approach can simplify data retrieval and improve performance, particularly in scenarios involving complex data relationships.
