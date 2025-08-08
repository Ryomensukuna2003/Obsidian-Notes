## 1. Overview

- **MongoDB** stores data in **BSON** (Binary JSON).
    - BSON is a binary format that encodes type and length information.
    - This structure allows BSON to be traversed more quickly than plain JSON.

---

## 2. Basic Commands & CRUD Operations

### A. Basic Commands

```javascript
show dbs;
use <DB_NAME>; // Example: use 'x'
db.students.insertOne({ name: "Ram", age: 12 }); // Insertion
db.students.find(); // Retrieve all documents
db.students.updateMany({}, { $set: { hobbies: ['Anime', 'cooking'] } }); // Add/update field
db.students.find({ hobbies: "cooking" }).count(); // Count matching documents
```

- **Note on Nested Fields:**
    - To query nested fields, use string notation:
        
        ```javascript
        db.students.find({ "identity.hasPanCard": true });
        ```
        
- **Document Size Limit:**
    - A single document must not exceed **16 MB**.

---

### B. CRUD Operations

#### **Create**

- **Methods:**
    - `insertOne(data, options)`
    - `insertMany(data, options)`

```javascript
db.students.insertOne({ "name": "idk" });
db.students.insertMany([{ "name": "idk" }, { "name": "helo" }]); // Array required for insertMany
```

---

#### **Update**

- **Methods:**
    - `updateOne(filter, data, options)`
    - `updateMany(filter, data, options)`
    - `replaceOne(filter, data, options)`

```javascript
db.students.updateOne({ "name": "idk" }, { $set: { age: 15 } });
db.students.updateMany({ age: 12 }, { $set: { age: 15 } });
db.students.updateMany({ age: 12 }, { $set: { isEligible: false } }); // Adds new field if not exist
```

---

#### **Read**

- **Methods:**
    - `find(filter, options)` – returns a cursor (which you can iterate over).
    - `findOne(filter, options)` – returns a single document.

```javascript
db.students.find().forEach((x) => { printjson(x); });
db.students.find({ age: { $lt: 12 } });
db.students.find({ age: { $gte: 12 } });
db.students.find({ age: { $gt: 12, $le: 50 } });
db.students.find({ age: { $gt: 12, $le: 50 } }).toArray(); // Returns an array
db.students.find({ name: 1, _id: 0 }); // Projection: lists only the name field
```

---

#### **Delete**

- **Methods:**
    - `deleteOne(filter, options)`
    - `deleteMany(filter, options)`

```javascript
db.students.deleteMany({ isEligible: false });
db.students.deleteOne({ name: "idk" });
db.students.deleteMany({}); // Deletes all documents in the collection
```

---

## 3. Data Types in MongoDB

- **Text:** String values.
- **Boolean:** `true`/`false`.
- **Number:**
    - Integers
    - NumberLong
    - NumberDecimal
- **ObjectID:** Unique identifier for documents.
- **ISO Date:** Date objects.
- **TimeStamp:** Stores epoch time and includes an extra index useful for bulk operations.
- **Array:** Stores lists of values.
- **Embedded Document:** Nested objects within a document.

---

## 4. Database and Collection Operations

- **Drop a Database:**
    
    ```javascript
    db.dropDatabase();
    ```
    
- **Drop a Collection:**
    
    ```javascript
    db.school.drop(); // Where 'school' is the collection name
    ```
    

---

## 5. Options in Queries

- **Ordered Inserts (Default):**
    
    - Stops at the first error (e.g., duplicate _id).
    
    ```javascript
    db.students.insertMany([
      { _id: 'A', name: 'Ram' },
      { _id: 'B', name: 'Ram' },
      { _id: 'A', name: 'Ram' },
      { _id: 'D', name: 'Ram' }
    ]);
    ```
    
- **Unordered Inserts:**
    
    - Continues inserting documents even if some errors occur.
    
    ```javascript
    db.students.insertMany([
      { _id: 'A', name: 'Ram' },
      { _id: 'B', name: 'Ram' },
      { _id: 'A', name: 'Ram' },
    - Stops at the first error (e.g., duplicate id).
    

    db.students.insertMany([
      { _id: 'A', name: 'Ram' },
      { _id: 'B', name: 'Ram' },
      { _id: 'A', name: 'Ram' },
```javascript
db.createCollection("nonFiction", {
  validator: {
    $jsonSchema: {
      required: ['name', 'price'],
      properties: {
        name: {
          bsonType: 'string',
          description: 'must be a string'
        },
        price: {
          bsonType: 'number',
          description: 'must be a number'
        }
      }
    }
  },
  validationAction: 'error'
});
```

- **Notes:**
    - Use `$jsonSchema` to define validation rules.
    - `validationAction: 'error'` ensures documents not matching the schema are rejected.

---

## 7. Atomicity in MongoDB

- **Atomicity:**
    - Guaranteed at the **document level**.
    - **Single operations** like `insertOne` are atomic.
    - **Multi-document operations** (e.g., using `insertMany`) are not rolled back if some operations fail.

---

## 8. Comparison Operators

- **Operators:**
    - `$eq` : Equal
    - `$lt` : Less than
    - `$lte`: Less than or equal to
    - `$in` : In an array
    - `$ne` : Not equal
    - `$gt` : Greater than
    - `$gte`: Greater than or equal to
    - $nin: Not in an array


Logical Operators
$and
```Javascript
	db.students.find({$and:[{age:{$lte:12}},hobbies:'waliking']})
	db.students.find({age:{$gte:12},age:{$lte:12}}) // If we give same key as parameter then last key will run hence and should be used here
```
$or
```Javascript
	db.students.find({$or:[{age:{$lte:12}},age:{gte:{14}}]})
```
$nor
```Javascript
	db.students.find({$nor:[{age:{$lte:12}},age:{gte:{14}}]})
```
$not

### $exists Operator

Find documents where the job_role field exists and equal to “Cashier”.

```Javascript
db.employees.find({ "emp_age": { $exists: true, $gte: 30}}).pretty()
```

### $type Operator

The following query returns documents if the emp_age field is a double type. If we specify a different data type, no documents will be returned even though the field exists as it does not correspond to the correct field type.

```Javascript
db.employees.find({ "emp_age": { $type: "double"}})
```

### $jsonSchema Operator

Find documents that match the following JSON schema in the promo collection.

The $let aggregation is used to bind the variables to a results object for simpler output. In the JSON schema, we have specified the minimum value for the “period” field as 7, which will filter out any document with a lesser value.


```Javascript
let promoschema = {
bsonType: "object",
required: [ "name", "period", "daily_sales" ],
properties: {
"name": {
bsonType: "string",
description: "promotion name"
},
"period": {
bsonType: "double",
description: "promotion period",
minimum: 7,
maximum: 30
},
"daily_sales": {
bsonType: "array"
}
}
}
```

```Javascript
db.promo.find({ $jsonSchema: promoschema }).pretty()
```

### $mod Operator

Find documents where the remainder is 1000 when divided by 3000 in the inventory collection.

Note that the document “Milk Non-Fat – 1lt” is included in the output because the quantity is 1000, which cannot be divided by 3000, and the remainder is 1000.

```Javascript
db.inventory.find({"quantity": {$mod: [3000, 1000]}}).pretty()
```


### $regex Operator

Find documents that contain the word “Packed” in the name field in the inventory collection.

```Javascript
db.inventory.find({"name": {$regex: '.Packed.'}}).pretty()
```


```Javascript
db.students.find({$and:[{'product.name':'Apple'},'quantity':{$gte:15}]}) // wrong query
db.students.find({$elemMatch:{quantity:{$gte:15},'name':'apple'}}) // will search element wise
```

Sorting
```Javascript
db.students.find().sort({age:-1}) //decending
db.students.find().sort({age:1,name:1}) // ascending
```


inc operator
```Javascript
db.students.updateMany({},{$inc:{age:1}})
```

max operator
```Javascript
db.students.updateMany({name:'sita'},{$max:{$max:50}}) // if less then 50 then assign to 50 else skip
```

upsert operator
```Javascript
db.students.updateOne({'name':'golu'},{$set:{age:30}},{upsert:true}) // if not exist then create one
```

```Javascript
db.students.updateMany({'experience':{$elemMatch:{duration:{$lte:1}}}},{$set:{'experience.$[].neglect':1}})
```



Indexing
MongoDB uses B-tree to store indexes 

Trade off
Storage Space with write operations

Several Types of indexes
	Single index
	Compound index
	Text index

When not to use indexing?
Small collection
Frequently updated collection
Complex Queries
Collection is large then make less indexes


In case of multiple indexes 

Mongo selects some docs and race them with all indexes and stores the winner in cache 
For second querey it uses winner index 

Cache resets after
1000 writes
If index is reset
Mongo Server is restarted 
Other indexs are manipulated
