### What is DBMS?

Software that allows users to create, manage, and access databases efficiently.

### Primary Key

Uniquely identifies each record in a table.

### Difference between Unique and Null Constraints

- **Unique**: Ensures no duplicate values.
- **Null**: Ensures the column does not contain null values.
### Degree of a Relation

Number of columns or attributes in a table.

### Logical vs Physical Database Design

- **Logical Design**:
    - What data should be stored
    - Attributes and relationships
- **Physical Design**:
    - Data storage format
    - Indexing, partitioning, and access methods

### Database Languages

- **DDL (Data Definition Language)**: CREATE, ALTER, DROP, TRUNCATE
- **DML (Data Manipulation Language)**: INSERT, UPDATE, DELETE
- **DQL (Data Query Language)**: SELECT

### Indexing

Technique to sort data in a column for faster search.

### WHERE vs HAVING

- **WHERE**: Filters rows before grouping.
- **HAVING**: Filters after grouping (used with aggregate functions).

### Transaction

A sequence of logically connected operations treated as a single unit. Includes:
- READ
- WRITE
- COMMIT
- ROLLBACK

### Query Optimization

- Improves performance
- Saves resources
- Enhances scalability
- Reduces costs

### File System vs DBMS

- **File System**: Basic storage, lacks relationships, and querying capabilities.
- **DBMS**: Structured storage with integrity, security, and query features.

### E-R Model (Entity-Relationship Model)

- **Entity** (e.g., Student)
    - Strong / Weak Entity 
- **Attributes** (e.g., Reg No)
    - Simple, Composite, Multi-valued, Derived, Key     
- **Relationships**
    - Unary, Binary, Ternary, N-ary

### Generalization vs Specialization

- **Generalization**: Bottom-up; combines entities.
- **Specialization**: Top-down; divides entity into sub-entities.

### Relational Algebra

- Selection (\sigma)
- Projection (\pi)
- Union (\cup)
- Set Difference (-)
- Cartesian Product (\times)

### Normalization

Organizing data to reduce redundancy and dependency.

#### Normal Forms Table

|Form|Issue|Fix|
|---|---|---|
|**1NF**|Non-atomic values|Ensure each field holds only atomic (indivisible) values|
|**2NF**|Partial Dependency|Move partially dependent attributes to new tables|
|**3NF**|Transitive Dependency|Eliminate indirect dependency; use separate table|
|**BCNF**|Determinant not a superkey|Decompose table to ensure every determinant is a superkey|

#### Examples

- **1NF**: If a student has multiple phone numbers in the same row, split them into separate rows so each field has just one value.
    
- **2NF**: Suppose you have a table with (StudentID, CourseID) as the key, and it also stores StudentName. Since StudentName only depends on StudentID, move it to a separate table.
- When a key is derived by a composite key then the table is not in 2NF.
    
- **3NF**: If a table has StudentID, CourseID, and Instructor, and Instructor depends on CourseID (not directly on StudentID), split Instructor into its own table.
    
- **BCNF**: If Instructor decides the Course, but Instructor is not a unique key, we split the data so Instructor becomes a key in its own table.

### Clustered vs Non-Clustered Index

- **Clustered**: Alters physical order of data
- **Non-Clustered**: Maintains separate index structure

### Triggers

Automated actions triggered on INSERT, UPDATE, DELETE.

### Pattern Matching in SQL

- `%`: Matches zero or more characters
- `_`: Matches exactly one character

```sql
SELECT * FROM table_name WHERE name LIKE '_a%';
```

### Concurrency Control

- **Locks (Shared/Exclusive)**:
    - **Shared Lock**: Lets many users read the data, but no one can change it while it's locked.
    - **Exclusive Lock**: Only one user can read and change the data — others have to wait.
    
- **Two-Phase Locking (2PL)**:
    - Helps avoid conflicts by organizing how locks are taken and released.
    - **Growing phase**: The transaction takes all the locks it needs.
    - **Shrinking phase**: Once it starts releasing locks, it can’t take new ones.
    - This process helps keep things consistent, but sometimes it can cause waiting (deadlocks).
        
- **Timestamp Ordering**:
    - Each transaction gets a unique timestamp when it starts.
    - Transactions are allowed only if they follow this time order.
    - If a newer transaction tries to do something before an older one, it might get canceled and restarted.
    - This avoids conflicts without using locks.
### Keys

- **Primary Key**: Unique identifier
- **Candidate Key**: All unique identifiers
- **Alternate Key**: Candidate key not chosen as primary