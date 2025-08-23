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