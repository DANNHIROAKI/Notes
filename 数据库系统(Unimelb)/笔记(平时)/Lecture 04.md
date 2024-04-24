# L04 Relational Model

# 1. Relational Model

> ## 1.1. Relational Data Model
>
> > **1️⃣**Data Model: 
> >
> > 1. Conception: Model can translate real-world entities into computer readable structure
> > 2. Example: Object-oriented model / Network model / Hierarchical model
> > 3. Data model covered in this subject:  Relational model
> >
> > **2️⃣**Relational Model: Represents data in the form of tables (Like in excel)
> >
> > 1. In database realm(领域)
> >
> >    - Row : Tuples(元组) or records
> >    - Column: Attributes and fields
> >
> > 2. Example: Enrolled table & Students table
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319210614082.png" alt="image-20240319210614082" style="zoom:50%;" /> 
> >
> >    - Primary Key: Field `sid` in table, uniquely identifies each tuples within that table
> >    - Foreign Key: Pointer in the table, uniquely references the primary key of another table
>
> ## 1.2. Relational Databases
>
> > **1️⃣**Basic Definitions
> >
> > 1. Relational Databases: A set of relations
> > 2. ==Relation: A set of records==
> >
> > **2️⃣**2 parts of relation
> >
> > 1. Schema(示意图): Descriptive part, Specify the relation's **NAME**, each attribute ‘s **NAME and TYPE**
> > 2. Instance / Table: Relation$\xrightarrow{\text{Populated(填充) with actual data}}$​Table / Instance. In fact the term table / relation can be used interchangeably(客户换地)
> >
> > **3️⃣**Other Definitions:
> >
> > 1. Cardinality(基数): Number of rows/records/tuples in the table. 
> >    - All rows within a table are distinct(不同的)
> >    - No specific order among rows
> > 2. Degree: Number of column/attributes/fields in the table
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319233816944.png" alt="image-20240319233816944" style="zoom: 50%;" />   

# 2. SQL overview: The Entire Cycle

> **1️⃣**Conversion
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320014610955.png" alt="image-20240320014610955" style="zoom: 50%;" /> 
>
> 1. Conceptual$\to$Logical
>
> | ER (Conceptual) | $\xrightarrow{\text{Become}}$ | Relational Model (Logical) |
> | :-------------: | :---------------------------: | :------------------------: |
> |     Entity      | $\xrightarrow{\text{Become}}$ |          Relation          |
> |   Attributes    | $\xrightarrow{\text{Become}}$ | Attributes of the relation |
>
> 2. Logical$\xrightarrow{\text{Choose data type}}$​Physical
> 3. Implementation: Create tables using MySQL
> 4. Instance: Populate table with data
>
> **2️⃣**E.g. 
>
> 1. Conceptual Design
>
>    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320014726037.png" alt="image-20240320014726037" style="zoom:50%;" /> 
>
> 2. Logical Design: 
>
>    `Employee (ssn, name, age)`
>
> 3. Physical Design: 
>
>    `Employee (ssn CHAR(11), name VARCHAR(20), age INTEGER)`
>
> 4. Implementation: Example of creating Relations in SQL
>
>    ```sql
>    CREATE TABLE Employee (
>       ssn CHAR(11),
>       name VARCHAR(20), 
>       age INTEGER, 
>       PRIMARY KEY (ssn)
>    );
>    ```
>
> 5. Instance: 
>
>    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320014906038.png" alt="image-20240320014906038" style="zoom:50%;" /> 

# 3. Keys: One form of Integrity Constraint(IC)

> ## 3.1. Key Overview
>
> > **1️⃣**Function: Associate tuples in different relations.
> >
> > **2️⃣**Types: 
> >
> > 1. Primary key: Ensuring each record in a database is unique and easy to locate
> >
> >    E.g. CID as Primary key in Enrolled table, correspond to each turple
> >
> > 2. Foreign key:  Refer to entries in another table
> >
> >    E.g. SID as Foreign key in Enrolled table, refer to  entries in Students table
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320020046568.png" alt="image-20240320020046568" style="zoom:50%;" /> 
>
> ## 3.2. Primary key
>
> > **1️⃣**Super key
> >
> > 1. Conception: A set of fields within a relation that uniquely identifies each tuple
> >
> > 2. Feature: 
> >
> >    - No two distinct tuples can share the same value super key fields
> >    - Every relation naturally has a super key since tuples must be different
> >
> > 3. E.g. 
> >
> >    - Super key can't be <font color='orange'>name or age</font>, since  two Smith or two 18-year-old will have the same fields
> >    - Super key actually can be <font color='orange'>names+login+age</font>
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320021912987.png" alt="image-20240320021912987" style="zoom:50%;" />  
> >
> > **2️⃣**Key in a relationship: 
> >
> > 1. Conception: Specific type of super key—one that has no redundant information
> > 2. Can be divided in: One primary(主) key + many candidates key, Every relation have a primary key
> > 3. Composite(复合) key:  Primary key comprise multiple columns when no individual column is unique
> >
> > **3️⃣**E.g. Students relationship
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320024141680.png" alt="image-20240320024141680" style="zoom: 67%;" /> 
> >
> > 1. Super key: Set of attributes ensure no two rows are duplicate
> >
> >    Can be <font color='orange'>sid+name+login+age+gpa</font>
> >
> > 2. Key: Unique+Minimize
> >
> >    Can be <font color='orange'>sid or login</font>
> >
> > 3. Primary key: Choose one from key, here we choose <font color='orange'>sid</font>
> >
> > 4. In SQL: 
> >
> > ```sql
> > CREATE TABLE Enrolled (
> >    sid CHAR(20), 
> >    cid CHAR(20), 
> >    grade CHAR(2), 
> >    PRIMARY KEY (sid,cid)
> > );
> > ```
>
> ## 3.3. Foreign Keys
>
> > **1️⃣**Conception: One/more column used to ‘refer’ to a tuple in another relation
> >
> > **2️⃣**Foreign Keys in SQL
> >
> > 1. SID in the 'enroll' table$\xrightarrow{\text{Foreign key}}$​SID in the 'students' table
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320032739617.png" alt="image-20240320032739617" style="zoom: 60%;" /> 
> >
> > 2. SQL code
> >
> > ```sql
> > CREATE TABLE Enrolled (
> >    sid CHAR(20),
> >    cid CHAR(20),
> >    grade CHAR(2),
> >    PRIMARY KEY (sid,cid),
> >    FOREIGN KEY (sid) REFERENCES Students 
> > );
> > ```
> >
> > **3️⃣**Foreign Keys Actions
> >
> > 1. If a tuple in enrolled table with a non-existent student id is inserted.
> >
> > 2. If  a record in students table be deleted, correspond record will be deleted in enrolled table. Not the other way around.

# 4. Integrities

> **1️⃣**Referential Integrity: All foreign key (references) are valid.
>
> **2️⃣**Domain constraint: E.g.  If an attribute is defined as an integer, then only integer values are entered for that attribute
>
> **3️⃣**Integrity Constraints(ICs): 
>
> 1. Conception: Condition every instance should obey
> 2. Legal instance: Instances adhere(遵守) to the defined integrity constraints

# 5. ER$\xrightarrow{}$​Logical & Physical Model

> ## 5.1. Single entity→Single relation
>
> > **1️⃣**Simple circumstances
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320035756022.png" alt="image-20240320035756022" style="zoom:50%;" /> 
> >
> > **2️⃣**Multi-valued attributes in logical design: Unpack
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320040214637.png" alt="image-20240320040214637" style="zoom:50%;" /> 
> >
> > |     Conceptual     |         Logical          |
> > | :----------------: | :----------------------: |
> > | N-valued attribute | N * one-valued attribute |
> >
> > **2️⃣**Composite attributes in logical design: Unpack
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320041011712.png" alt="image-20240320041011712" style="zoom:50%;" /> 
> >
> > |                Conceptual                 |                           Logical                            |
> > | :---------------------------------------: | :----------------------------------------------------------: |
> > | Composite attribute with N sub-attributes | Sub-attribute 1<br>Sub-attribute 2<br/>....<br/>Sub-attribute N |
>
> ## 5.4. Many$\xleftrightarrow{}$​Many
>
> > **1️⃣**Conceptual Design(ER)
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320042219262.png" alt="image-20240320042219262" style="zoom: 50%;" /> 
> >
> > **2️⃣**Logical Design(Relation model): 
> >
> > 1. Elements: Employee/Department table + ==junction table(many-many only)==
> > 2. Junction table key: Two primary keys of Employee/Department$\xrightarrow{\text{Combine}}$​​Its own composite primary key
> >
> > |              Relations               | Primary Keys(PK) | Foreign Keys(FK) |
> > | :----------------------------------: | :--------------: | :--------------: |
> > |    Employee (ssn, name, age)<br/>    |       ssn        |       N/A        |
> > | Department (did, dname, budget)<br/> |       did        |       N/A        |
> > |      Works_In (ssn, did, since)      |     ssn, did     |     ssn, did     |
> >
> > **3️⃣**Physical Level: Simply adding data types
> >
> > ```sql
> > Employee    (ssn CHAR(11),name VARCHAR(20),age INTEGER)
> > Department  (did INTEGER,dname VARCHAR(20),budget FLOAT)
> > Works_In    (ssn CHAR(11),did INTEGER,since DATE)
> > ```
> >
> > **4️⃣**Implementation: Creating tables in SQL
> >
> > ```sql
> > CREATE TABLE Employee ( -- Declare the primary key for each table
> >    ssn CHAR(11), 
> >    name VARCHAR(20),
> >    age INTEGER,
> >    PRIMARY KEY (ssn)
> > );
> > 
> > CREATE TABLE Department (
> >    did INTEGER,
> >    dname VARCHAR(20),
> >    budget FLOAT,
> >    PRIMARY KEY (did)
> > );
> > 
> > CREATE TABLE Works_In (
> >    ssn CHAR(11),
> >    did INTEGER
> >    since DATE,
> >    PRIMARY KEY (ssn, did)
> >    FOREIGN KEY (ssn) REFERENCES Employee,
> >    FOREIGN KEY (did) REFERENCES Department
> > );
> > ```
> >
> > **5️⃣**Instance: Populate tables with data while enforcing referential integrity
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320044323662.png" alt="image-20240320044323662" style="zoom:50%;" />  
> >
> > 
>
> ## 5.5. Ternary relationship
>
> > **1️⃣**Conceptual Design: 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320045513507.png" alt="image-20240320045513507" style="zoom:50%;" /> 
> >
> > **2️⃣**Logical Design
> >
> > ```sql
> > Contracts   (supplier_id,part_id,department_id) -- three of each are PFKs
> > Suppliers   (id,name)                           -- id(PK)
> > Parts       (id,name)                           -- id(PK)
> > Departments (id,name)                           -- id(PK)
> > ```
> >
> > **3️⃣**Physical design + Implementation
> >
> > ```sql
> > CREATE TABLE Contracts (
> > supplier_id INTEGER,
> > part_id INTEGER,
> > department_id INTEGER,
> > PRIMARY KEY (supplier_id, part_id, department_id),
> > FOREIGN KEY (supplier_id) REFERENCES Suppliers,
> > FOREIGN KEY (part_id) REFERENCES Parts,
> > FOREIGN KEY (department_id) REFERENCES Departments
> > );
> > 
> > CREATE TABLE Suppliers (
> > id INTEGER PRIMARY KEY,
> > name VARCHAR(255)  
> > );
> > 
> > CREATE TABLE Parts (
> > id INTEGER PRIMARY KEY,
> > name VARCHAR(255)  
> > );
> > 
> > CREATE TABLE Departments (
> > id INTEGER PRIMARY KEY,
> > name VARCHAR(255)  
> > );
> > ```
>
> ## 5.6. Key Constrains
>
> > **1️⃣**Cponceptual
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320051340819.png" alt="image-20240320051340819" style="zoom: 67%;" />  
> >
> > **2️⃣**Logical
> >
> > 1. Design 1
> >
> >    ```sql
> >    Employee (ssn, name, age)        -- PK  ssn
> >    Department (did, dname, budget)  -- PK  did
> >    Manages (ssn, did, since)        -- PFK ssn+did
> >    ```
> >
> > 2. Design 2
> >
> >    ```sql
> >    Employee (ssn, name, age)                   -- PK ssn
> >    Department (did, dname, budget, ssn, since) -- PK ssn+did
> >    ```
> >
> > **3️⃣**Physical+Implementation
> >
> > 1. Design 1: Non-enforcement of the "maximum of one manager per department" rule
> >
> >    ```sql
> >    CREATE TABLE Manages (
> >       ssn CHAR(11),
> >       did INTEGER,
> >       since DATE,
> >       PRIMARY KEY (ssn, did),
> >       FOREIGN KEY (ssn)
> >       REFERENCES Employees,
> >       FOREIGN KEY (did)
> >       REFERENCES Departments
> >    );
> >    
> >    CREATE TABLE Employee (
> >       ssn CHAR(11) PRIMARY KEY,
> >       name VARCHAR(20)  
> >       age INTEGER  
> >    );
> >    
> >    CREATE TABLE Department (
> >       did INTEGER PRIMARY KEY,
> >       name VARCHAR(20)  
> >       budget FLOAT  
> >    );
> >    ```
> >
> > 2. Design 2: Better, ensured a maximum of one manager per department
> >
> >    ```sql
> >    CREATE TABLE Employee (
> >       ssn CHAR(11) PRIMARY KEY,
> >       name VARCHAR(20)  
> >       age INTEGER  
> >    );
> >    
> >    CREATE TABLE Department (
> >       did INTEGER,
> >       dname CHAR(20),
> >       budget FLOAT,
> >       ssn CHAR(11),
> >       since DATE,
> >       PRIMARY KEY (did)
> >       FOREIGN KEY (ssn)   -- Ensure Each department have a single manager 
> >       REFERENCES Employees
> >    );
> >    ```
> >
> > ⚠️ Key Constraints Rule: primary key from the "many" side of a relationship becomes a foreign key on the "one" side
>
> ## 5.7. Participation Constraints
>
> >  ==Eevery department MUST have a manager==
> >
> >  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320054202193.png" alt="image-20240320054202193" style="zoom: 67%;" /> 
> >
> >  ```sql
> >  CREATE TABLE Department (
> >  did INTEGER NOT NULL,
> >  dname CHAR(20) NOT NULL,
> >  budget FLOAT NULL,
> >  ssn CHAR(11) NOT NULL,   -- ssn must be NOT NULL
> >  since DATE NULL,
> >  PRIMARY KEY (did),
> >  FOREIGN KEY (ssn) REFERENCES Employee
> >  ON DELETE NO ACTION
> >  );
> >  ```
> >
> >  ⚠️EVERY TIME create a table should specify whether attributes are NULL or NOT NULL
>
> ## 5.8. Weak Entities
>
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240320054233153.png" alt="image-20240320054233153" style="zoom:50%;" /> 
> >
> > 1. Weak entity + identifying relationship set:  Translated into a single table
> > 2. Owner entity deleted: All weak entities deleted
> >
> > ```sql
> > CREATE TABLE Dependent (
> >    dname CHAR(20) NOT NULL,
> >    age INTEGER NULL,
> >    cost DECIMAL(7,2) NOT NULL,
> >    ssn CHAR(11) NOT NULL,
> >    PRIMARY KEY (dname, ssn), -- weak+owner entity key as composite primary key
> >    FOREIGN KEY (ssn) REFERENCES Employees   -- refer to owner entity
> >    ON DELETE CASCADE   -- Owner deleted,then all weak entities deleted
> > );
> > ```
> >



  

