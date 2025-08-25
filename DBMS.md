##### Null, blank and 0:
null - no value
blank - character
0 - number
##### Locks:
Shared - when read
Exclusive - when write

##### Levels of dbms abtraction:
1. Physical level: how data is stored, uses B tree
2. Conceptual / Logical : define operations on DB
3. View level: virtual table created by select query
4. GUI

##### Architecture of DBMS:
1. DBMS
2. Application
3. GUI

***
### DBMS
**Data**: Facts and details about anything
**Information:** data after processing becomes info. processing includes organising, structuring, analysis
**Database:** organisation of related data
**DBMS:** software that helps users create, maintain and manage databases. facilitate storage retrieval, concurrency and security controls.

**Instance and Schema:** instance is the actual collection of data present at a particular point of time whereas schema is the structure of the db

**OLAP and OLTP**: olap is for analysis of historical data matlab only read function is req but oltp is for transaction

**DBA:** person responsible for managing securing and maintaining db in a particular organisation

**Data mining:** gathering hidden patterns from massive amounts of data from databases

#### ER model
Entity - class - rectangle
Relation - diamond
Attribute - oval
	1. single values - oval
	2. multi value - double oval
	3. derived attr - dashed oval

Entity two types: tangible which exist in real world and intangible which do not exist in real world like bank account
Weak entity: double rectangle, any entity which has no primary key

Degree of relation: how many entities are participating in a relation. eg teacher, student and course participate in study so degree is 3

Cardinality: 1:1, m:1, 1:m, m:m
##### Relational Model Deep Dive
Key Concepts
- **Relation**: Table with rows and columns
- **Tuple**: Row in a table
- **Attribute**: Column in a table
- **Domain**: Set of possible values for an attribute
- **Degree**: Number of attributes in a relation
- **Cardinality**: Number of tuples in a relation

Properties of Relations
- No duplicate tuples
- Tuples are unordered
- Attributes are unordered
- All attribute values are atomic (indivisible)
- Each attribute has a unique name

**Anomalies:** three types insertion, update and deletion
1. insertion anomaly: independent data cannot be inserted without also including an irrelevant attribute info eg adding student without course name even if course is not yet available
2. updation anomaly: making one change requires making changes to multiple instances of that entity
3. deletion: occurs when deleting an instance of a record leads to loss of other unrelated data

##### Functional dependency:
X → Y means X functionally determines Y
- For any two tuples with same X value, Y values must be same
- X is determinant, Y is dependent

##### Keys:
attributes or set of attr used to uniquely identify entries or maintain relations to other tables

1. super key: any attr or set of attr that can uniquely identify a tuple
2. candidate key: minimal super key, ie. a super key which is not a superset of another superkey eg if we can identify a row using A and AB so both A and AB are super keys but only AB is candidate key because AB is not minimal
3. primary key: out of candidate keys, one key is chosen as primary key
4. alternate keys: rest of the candidate keys are alt keys
5. foreign key: maps to another table
6. composite key: when table has no candidate key, a group of attr is chosen for identification called composite key

##### Normalisation:
why?
1. anomalies
2. reduce redundancy
3. increase consistency
4. simplification of dbms
5. improved db design
6. standardisation

disadv: makes db slow and increases overhead because increases number of tables
###### Normal Forms:
1. **1NF: single valued**
2. **2NF: all non-key attr should be fully dependent on the key attr**
    Example: For a composite key (StudentID, CourseID), if the StudentName depends only on StudentID and not on the entire key, it violates 2NF. To normalize, move StudentName into a separate table where it depends only on StudentID.
3. **3NF: no transitive dependency** 
	Example: Consider a table with (StudentID, CourseID, Instructor). If Instructor depends on CourseID, and CourseID depends on StudentID, then Instructor indirectly depends on StudentID, which violates 3NF. To resolve this, place Instructor in a separate table linked by CourseID.
4. **BCNF: boyce-codd: every determinant must be a super key**
	3NF Rule: A non-key attribute should not depend on another non-key attribute (no transitive dependency).
	BCNF Rule: ANY attribute should not depend on a non-super-key attribute.
5. **4NF: multi-valued dependency nahi honi chahiye**
   explanation:
		**Relation**: STUDENT_SKILLS_HOBBIES
	
	|Student|Skill|Hobby|
	|---|---|---|
	|John|Java|Reading|
	|John|Java|Swimming|
	|John|Python|Reading|
	|John|Python|Swimming|
	|Mary|C++|Dancing|
	|Mary|C++|Cooking|
	|Mary|JavaScript|Dancing|
	|Mary|JavaScript|Cooking|
	
	**Analysis**:
	
	- John has skills: {Java, Python}
	- John has hobbies: {Reading, Swimming}
	- Mary has skills: {C++, JavaScript}
	- Mary has hobbies: {Dancing, Cooking}
	
	**Multivalued Dependencies**:
	
	- Student →→ Skill (for each student, there's a set of skills independent of hobbies)
	- Student →→ Hobby (for each student, there's a set of hobbies independent of skills)
	
	**4NF Violation**:
	
	- Student →→ Skill exists, but Student is not a super key in this context
	- This creates redundancy: John's skills appear with every hobby combination
	
	**Problems**:
	
	1. **Redundancy**: John's Java skill is repeated for each hobby
	2. **Insert Anomaly**: To add a new skill for John, we need to add rows for each existing hobby
	3. **Delete Anomaly**: If John drops swimming, we might lose information about his programming skills
	4. **Update Anomaly**: If John's skill name changes, multiple rows need updating
	
	### Converting to 4NF
	
	**Solution**: Decompose into separate relations for each MVD
	
	**Decomposed Relations**:
	
	1. **STUDENT_SKILLS**:
	    
	    |Student|Skill|
	    |---|---|
	    |John|Java|
	    |John|Python|
	    |Mary|C++|
	    |Mary|JavaScript|
	    
	2. **STUDENT_HOBBIES**:
	    
	    |Student|Hobby|
	    |---|---|
	    |John|Reading|
	    |John|Swimming|
	    |Mary|Dancing|
	    |Mary|Cooking|
6. **5NF: no join dependencies ie. it can be decomposed to more than 2 tables and a join query can produce the exact original table**

##### Indexing:
why? to increase speed of retrieval and storage

if we just sort the db then storage takes more time-complexity and if we dont sort then retrieval takes more time

hence we make an index file which associates index numbers to data references where indices are sorted hence time complexity to search a particular index is O(logN) and storage is O(1).
 
 types: single level and multi level, sparse and dense: sparse matlab har entry ko index nahi milta and dense me milta hai
 - Single level: 
	 - primary indexing: db sorted based on primary key and index refers to primary key
	 - cluster indexing: db ordered on non-key attr and index on that, the index refers to the first occurrence of the non-key attr in the db
	 - secondary: happens when db is not sorted on any attr, no. of entries in db = no. of entries in index
- Multi level: index of index file because db is too big

###### B tree and B+ tree:
used to access index when db is too large, stores indices in binary tree for efficient retrieval

diff between b tree and b+ tree:
b tree only supports top-down traversal of tree
b+ tree supports top-down traversal and sequential traversal by maintaining a copy of upper branch nodes in the leaves as well

##### SQL:
format:
SELECT *col name* FROM *table name* WHERE *condition*

Some commands trivia:
- SELECT **DISTINCT** name FROM student_table
- SELECT name, salary FROM employee **WHERE** salary > 50000
- SELECT name, salary FROM employee WHERE salary **BETWEEN** 5000 AND 10000
- SELECT stud_name FROM student **UNION** SELECT emp_name FROM employee
- SELECT depositor.name, account.amount FROM depositor, account WHERE depositor.name = account.name
	*we can do this by natural join so we wont have to write these conditions*
- SELECT depositor.name, account.amount FROM depositor **NATURAL JOIN** account
- SELECT **TOP 5** \* FROM employees ORDER BY salary DESC
- SELECT dept_id, manager_id, COUNT(\*) AS team_size FROM employees **GROUP BY** dept_id, manager_id;
- SELECT dept_id, AVG(salary) AS avg_salary FROM employees GROUP BY dept_id **HAVING** AVG(salary) > 60000AND COUNT(\*) >= 3;
- ***rename:*** SELECT account_name **AS** Account FROM account
- AGGREGATE FUNCTIONS in sql:
	- average: select average(balance) from account
	- count
	- sum
	- min
	- max
- SELECT e.name, d.dept_name FROM employees e LEFT JOIN departments d ON e.dept_id = d.dept_id;
-  SELECT * FROM employees WHERE name LIKE 'J%';
- SELECT * FROM employees WHERE dept_id IN (1, 2, 3);

Trivia: 
having and where do the same thing but having clause is used **after** grouping data

inner join - common from both
left join - all from left and only common ones from right
right join = all from right and only common ones from left
full outer = all from both
cross join - cartesian product = all combinations
natural join = automatically joins on common attr

##### Transactions:
A transaction is a series of operations such as read, write, update performed on a db as a single logical unit of work
###### ACID properties:
Atomicity: each transaction either must be completed or not done at all
Consistent: state of db before and after transaction must be same
Isolation: each transaction acts as if its being performed in isolation, that it does not affect other on going transactions and does not get affected by other trans
Durable: changes should persist

###### States:
1. Active - performing operations
2. Partially Commited - operations performed but not saved in db
3. Commited - saved in db
4. Failure - error occurred
5. Aborted - rolled back
6. Terminated - end

###### Concurrency problems in dbms
1. Dirty Read - transaction updated a value and failed, before the changes could be reversed another transaction read that value
2. Incorrect Summary - when one transaction is using the aggregate function but the other transaction is updating the values simultaneously 
3. Lost Update - one transaction makes an update to data but another transaction updates it again
4. Unrepeatable read - 2 or more read operations of the same transaction read different values of same instance
5. Phantom read - transaction reads a value once but the next time it tries reading that value, the variable doesnt exist

To solve these problems, we can use serialisability and locks, time stamping

# Complete DBMS Interview Questions Guide for CSE Freshers

## 🔰 **BASIC LEVEL QUESTIONS**

### Fundamental Concepts

1. **What is DBMS? What are its advantages over file systems?**
    
    - Database Management System manages data efficiently
    - Advantages: Data independence, reduced redundancy, concurrent access, security, backup/recovery
2. **What is a database? Differentiate between database and DBMS.**
    
    - Database: Collection of interrelated data
    - DBMS: Software to manage databases
3. **What are the different types of databases?**
    
    - Relational, NoSQL (Document, Key-Value, Graph, Column-family), Object-oriented, Hierarchical, Network
4. **Explain the three-level architecture of DBMS (ANSI-SPARC).**
    
    - External Level (View Level)
    - Conceptual Level (Logical Level)
    - Internal Level (Physical Level)
5. **What is data independence? Types?**
    
    - Logical Data Independence: Changes in logical schema don't affect external schema
    - Physical Data Independence: Changes in physical schema don't affect logical schema
6. **What are the components of DBMS?**
    
    - Query Processor, Storage Manager, Transaction Manager, Buffer Manager, Recovery Manager

### Keys and Constraints

7. **What are different types of keys in DBMS?**
    
    - Primary Key, Foreign Key, Candidate Key, Super Key, Composite Key, Alternate Key
8. **What is the difference between Primary Key and Unique Key?**
    
    - Primary Key: Cannot be NULL, only one per table, automatically creates clustered index
    - Unique Key: Can have one NULL, multiple allowed, creates non-clustered index
9. **What are constraints in DBMS?**
    
    - NOT NULL, UNIQUE, PRIMARY KEY, FOREIGN KEY, CHECK, DEFAULT
10. **What is referential integrity?**
    
    - Ensures foreign key values match primary key values in referenced table

### Basic SQL

11. **What are DDL, DML, DCL, and TCL commands?**
    
    - DDL: CREATE, ALTER, DROP, TRUNCATE
    - DML: SELECT, INSERT, UPDATE, DELETE
    - DCL: GRANT, REVOKE
    - TCL: COMMIT, ROLLBACK, SAVEPOINT
12. **Difference between DELETE, TRUNCATE, and DROP.**
    
    - DELETE: Removes rows, can be rolled back, slower
    - TRUNCATE: Removes all rows, cannot be rolled back, faster
    - DROP: Removes entire table structure

## 🔷 **INTERMEDIATE LEVEL QUESTIONS**

### Normalization

13. **What is normalization? Why is it needed?**
    
    - Process of organizing data to reduce redundancy and dependency
    - Eliminates insertion, deletion, and update anomalies
14. **Explain different normal forms (1NF, 2NF, 3NF, BCNF).**
    
    - 1NF: Atomic values, no repeating groups
    - 2NF: 1NF + no partial dependencies
    - 3NF: 2NF + no transitive dependencies
    - BCNF: 3NF + every determinant is a candidate key
15. **What are functional dependencies?**
    
    - If A determines B, then A → B (for every value of A, there's exactly one value of B)
16. **What is denormalization? When is it used?**
    
    - Intentionally introducing redundancy to improve query performance
    - Used in data warehouses and when read performance is critical

### Transactions and Concurrency

17. **What is a transaction? What are ACID properties?**
    
    - Transaction: Unit of work that must be completed entirely or not at all
    - ACID: Atomicity, Consistency, Isolation, Durability
18. **What are different transaction states?**
    
    - Active, Partially Committed, Committed, Failed, Aborted
19. **What is concurrency control? Why is it needed?**
    
    - Manages simultaneous database operations to maintain consistency
    - Prevents lost updates, dirty reads, phantom reads
20. **What are different types of locks?**
    
    - Shared Lock (S-Lock): For reading
    - Exclusive Lock (X-Lock): For writing
    - Intent Locks: IS, IX, SIX
21. **What is deadlock? How is it handled?**
    
    - Circular wait condition where transactions wait for each other
    - Detection, Prevention, Avoidance methods

### Indexing

22. **What is an index? Types of indexes?**
    
    - Data structure to speed up data retrieval
    - Types: Clustered, Non-clustered, Unique, Composite, Partial
23. **Difference between clustered and non-clustered index.**
    
    - Clustered: Physical order matches index order, one per table
    - Non-clustered: Separate structure pointing to data rows, multiple allowed
24. **What is B-Tree? Why is it used in databases?**
    
    - Self-balancing tree data structure
    - Optimized for disk I/O, maintains sorted order

## 🔴 **ADVANCED LEVEL QUESTIONS**

### Complex Concepts

25. **What is query optimization? Explain query execution plan.**
    
    - Process of finding most efficient way to execute queries
    - Execution plan shows steps database takes to retrieve data
26. **What are different join types and algorithms?**
    
    - Types: INNER, LEFT, RIGHT, FULL OUTER, CROSS
    - Algorithms: Nested Loop, Hash Join, Sort-Merge Join
27. **What is database sharding? Types?**
    
    - Horizontal partitioning of data across multiple databases
    - Types: Range-based, Hash-based, Directory-based
28. **Explain CAP theorem.**
    
    - Consistency, Availability, Partition Tolerance
    - In distributed systems, you can guarantee only 2 out of 3
29. **What are isolation levels?**
    
    - Read Uncommitted, Read Committed, Repeatable Read, Serializable
30. **What is two-phase locking protocol?**
    
    - Growing Phase: Acquire locks, cannot release
    - Shrinking Phase: Release locks, cannot acquire

### Advanced SQL

31. **What are window functions? Give examples.**
    
    - Functions that operate on a set of rows related to current row
    - Examples: ROW_NUMBER(), RANK(), LEAD(), LAG()
32. **What is the difference between HAVING and WHERE clause?**
    
    - WHERE: Filters rows before grouping
    - HAVING: Filters groups after grouping
33. **Explain different types of subqueries.**
    
    - Scalar, Row, Table subqueries
    - Correlated vs Non-correlated

## 🔥 **TRICKY & FREQUENTLY ASKED QUESTIONS**

### Conceptual Tricks

34. **Can a table have multiple primary keys?**
    
    - No, but it can have composite primary key (multiple columns forming one key)
35. **What happens if we don't specify PRIMARY KEY?**
    
    - Table can still exist but may have duplicate rows
    - Some databases create hidden row identifiers
36. **Can foreign key be NULL?**
    
    - Yes, unless specified as NOT NULL
    - NULL foreign key means no relationship
37. **What is the difference between UNION and UNION ALL?**
    
    - UNION: Removes duplicates, slower
    - UNION ALL: Keeps duplicates, faster
38. **Can we use ORDER BY with UNION?**
    
    - Yes, but only at the end of the entire UNION statement
39. **What happens during a deadlock?**
    
    - Database automatically detects and kills one transaction (deadlock victim)
    - Usually kills the transaction that has done less work

### Performance & Design Tricks

40. **When should you NOT use indexes?**
    
    - Small tables
    - Frequently updated columns
    - Tables with high INSERT/UPDATE/DELETE activity
41. **What is the cost of maintaining indexes?**
    
    - Slower INSERT/UPDATE/DELETE operations
    - Additional storage space
    - Index maintenance overhead
42. **Why are NULLs tricky in databases?**
    
    - NULL comparisons always return UNKNOWN
    - COUNT(*) includes NULLs, COUNT(column) doesn't
    - NULL != NULL (returns NULL, not TRUE)
43. **What's the difference between VARCHAR and CHAR?**
    
    - CHAR: Fixed length, padded with spaces
    - VARCHAR: Variable length, no padding
44. **Can you have a foreign key without a primary key?**
    
    - Foreign key must reference a PRIMARY KEY or UNIQUE constraint

### SQL Gotchas

45. **What does this query return: SELECT NULL = NULL?**
    
    - Returns NULL (not TRUE or FALSE)
    - Use IS NULL instead
46. **Difference between IN and EXISTS?**
    
    - IN: Better for small result sets
    - EXISTS: Better for large result sets, stops at first match
47. **What's wrong with SELECT * ?**
    
    - Performance issues
    - Network overhead
    - Maintenance problems when table structure changes

## 🎯 **SCENARIO-BASED QUESTIONS**

48. **Design a database for a library management system.**
    
    - Tables: Books, Authors, Members, Issues, Returns
    - Relationships and constraints
49. **How would you find the second highest salary?**
    
    ```sql
    SELECT MAX(salary) FROM employees 
    WHERE salary < (SELECT MAX(salary) FROM employees);
    ```
    
50. **How to find duplicate records?**
    
    ```sql
    SELECT column1, column2, COUNT(*) 
    FROM table_name 
    GROUP BY column1, column2 
    HAVING COUNT(*) > 1;
    ```
    
51. **How would you optimize a slow-performing query?**
    
    - Check execution plan
    - Add appropriate indexes
    - Rewrite query logic
    - Consider partitioning
52. **Design considerations for a high-traffic e-commerce database.**
    
    - Read replicas for scaling
    - Caching strategies
    - Database partitioning
    - Connection pooling

## 🔧 **PRACTICAL IMPLEMENTATION QUESTIONS**

53. **Write a query to find employees with salary greater than their department average.**
    
    ```sql
    SELECT e1.* FROM employees e1
    WHERE salary > (SELECT AVG(salary) FROM employees e2 
                    WHERE e2.dept_id = e1.dept_id);
    ```
    
54. **How to implement soft delete?**
    
    - Add a boolean column (is_deleted)
    - Update instead of delete
    - Filter in all SELECT queries
55. **How to handle concurrent bookings (like movie tickets)?**
    
    - Use transactions with appropriate isolation levels
    - Implement optimistic or pessimistic locking
    - Check availability within transaction

## 💡 **BONUS QUESTIONS FOR FRESHERS**

56. **What databases have you worked with?**
    
    - Be honest about your experience
    - Mention any projects or coursework
57. **Explain your database project from college.**
    
    - Describe the problem solved
    - Technologies used
    - Challenges faced
58. **How do you stay updated with database technologies?**
    
    - Online courses, documentation, blogs
    - Practical projects and experimentation
59. **What's the difference between SQL and NoSQL?**
    
    - SQL: Structured, ACID, vertical scaling
    - NoSQL: Flexible schema, eventual consistency, horizontal scaling
60. **Why are databases important for software applications?**
    
    - Data persistence and organization
    - Concurrent access and consistency
    - Query capabilities and performance

---

## 📚 **Study Tips for Freshers**

1. **Practice SQL queries** on platforms like HackerRank, LeetCode
2. **Understand concepts** rather than memorizing answers
3. **Create sample databases** and experiment with different scenarios
4. **Focus on fundamentals**: Keys, Normalization, Transactions, Indexing
5. **Be prepared to write code** during interviews
6. **Understand trade-offs** in database design decisions

## 🎯 **Most Frequently Asked in Interviews**

- ACID properties
- Normalization (especially 1NF, 2NF, 3NF)
- Primary vs Foreign keys
- SQL joins
- Index types and usage
- Transaction isolation levels
- Difference between clustered and non-clustered indexes

Remember: **Practice writing SQL queries** and **understand the reasoning** behind database design decisions. Good luck with your interviews!