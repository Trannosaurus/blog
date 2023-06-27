# Migrations and adding fields

## MongoDB
Must run a script and query DB wide to update all documents

### cons:
1. Some documents could be missed in the query and wont be updated
2. due to the freeform of JSON, some documents can have some attributes while others dont leading to issues when querying and having to account for edge cases
3. This operation can fail leading to the issues above

### Solution: define a "schema"
MongoDB introduced a "Schema Validator" that can be applied to a collection

> **_NOTE:_**  a collection is a group of documents where are json file instances

whenever a document is inserted or updated, it will eb checked on the schema

changes can be made to that model , but validating existing data is less straightforward compared to adding new columns in SQL

### Mongoose
Additionally when taking this approach people often times use mongoose a Object-Data-Mapping (ODM) library for MongoDB in Node.js which allows you to define schems for MongoDB documents: results in automated validation, casting, etc.

Although with MongoDB schemas and data can take any form, in Mongoose, they can't. 
Once you have defined a schema, 

1. the schema cannot be changed
2. Making changes to MongoDB on the DB level, not Mongoose, like adding a attribute makes that document then not visible in Mongoose because it breaks the schema
    * This is very dangerous because during migrations, if migrations aren't aligned with the Mongoose scheme a lot of documents can be lost and wont be queried by Mongoose
    * Those files also will not be recognized by Mongoose so we would have to manually make scripts for these migrations

> **_NOTE:_**   Mongoose is a layer over MongoDB so when we define a schema it interacts with Mongoose in the application layer but not at the database level. 

## SQL
Migrations in SQL are as simple as 

```sql
ALTER TABLE users ADD COLUMN address VARCHAR;
```

This is a lot safer than the work arounds in MongoDB.

Additionally since we are already defining schemas with Mongoose, using SQL and defining tables are formats to the data doesn't make it anymore restricted than it is in Mongoose.
SQL adds a net of saftey in terms of how we handle data and also how data can be used. 
