# Lecture 8 SQL

Create, Read, Update, Delete commands (CRUD Comments)

# 0. SQL Overview

> ## 0.1. SQL Capabilities
>
> > | SQL Command Type   | Description                     | Commands                       |
> > | ------------------ | ------------------------------- | ------------------------------ |
> > | DDL (Definition)   | Define and set up the database  | CREATE, ALTER, DROP            |
> > | DML (Manipulation) | Maintain and use the database   | SELECT, INSERT, DELETE, UPDATE |
> > | DCL (Control)      | Governing permissions for users | GRANT, REVOKE                  |
> > | Other              | Administer the database         | Transaction Control Commands   |
>
> ## 0.2. How to use SQL
>
> > **1️⃣**Begins with the implementation phase: Using `CREAT` commands
> >
> > ```sql
> > CREATE TABLE BankHQ (
> >     BankHQID INT(4) AUTO_INCREMENT,
> >     HQAddress VARCHAR(300) NOT NULL,
> >     OtherHQDetails VARCHAR(500),
> >     PRIMARY KEY (BankHQID)
> > )
> > ```
> >
> > **2️⃣**Actual usage of the database: Discover the knowledge hidden within its data
> >
> > 1. `SELECT`: Read and link the data from tables
> > 2. `ALTER`and `DROP`: Modify the database structure
> > 3. `INSERT` / `UPDATE` / `DELETE`: Change the data within the database
> >
> > ```sql
> > -- Populate the data
> > 
> > -- Insert new record in BankHQ
> > INSERT INTO BankHQ VALUES 
> > (DEFAULT, "23 Charles St Peterson North 2022", 'Main Branch');
> > -- Insert another record in BankHQ
> > INSERT INTO BankHQ VALUES 
> > (DEFAULT, "213 Jones Rd Parkville North 2122", 'Sub Branch');
> > 
> > ```

# 1. Inner-table Operations

> ## 1.1. Create table: `CREAT`
>
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401070632764.png" alt="image-20240401070632764" style="zoom:50%;" /> 
> >
> > ```sql
> > CREATE TABLE Customer (
> >  CustomerID smallint auto_increment,
> >  CustFirstName varchar(100),
> >  CustMiddleName varchar(100),
> >  CustLastName varchar(100) NOT NULL,
> >  BusinessName varchar(200),
> >  CustType enum('Personal', 'Company') NOT NULL,
> >  PRIMARY KEY (CustomerID)
> > );
> > ```
> >
> > ```sql
> > CREATE TABLE Account (
> >  AccountID smallint auto_increment,
> >  AccountName varchar(100) NOT NULL,
> >  OutstandingBalance DECIMAL(10,2) NOT NULL,
> >  CustomerID smallint NOT NULL,
> >  PRIMARY KEY (AccountID),
> >  FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
> >      ON DELETE RESTRICT
> >      ON UPDATE CASCADE
> > );
> > ```
> >
> > **1️⃣**Some new elements
> >
> > 1.  `auto_increment`: When inserting a new record, attributes with auto_increment will +1
> > 2.  `ENUM`: Allowing only certain predefined values
> >
> > **2️⃣**About the Foreign Key
> >
> > ```sql
> > FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
> > ```
>
> ## 1.2. Insert Data: `INSERT INTO`
>
> > | CustID | FirstName | MiddleName | LastName | BusinessName   | CustType |
> > | ------ | --------- | ---------- | -------- | -------------- | -------- |
> > | 1      | Peter     | NULL       | Smith    | NULL           | Personal |
> > | 2      | James     | NULL       | Jones    | JJ Enterprises | Company  |
> > | 3      | NULL      | NULL       | Smythe   | NULL           | Company  |
> >
> > **1️⃣**Type 1: 
> >
> > 1. Specify the exact attributes to populate
> > 2. List the corresponding values for each attributes
> >
> > ```sql
> > INSERT INTO Customer (CustFirstName, CustLastName, CustType) 
> > VALUES ("Peter", "Smith", 'Personal');
> > ```
> >
> > **2️⃣**Type 2: Omit(省略) the specification of attributes, providing values in the order they are defined
> >
> > ```sql
> > INSERT INTO Customer 
> > VALUES (DEFAULT, "James", NULL, "Jones", "JJ Enterprises", 'Company');
> > 
> > INSERT INTO Customer1 
> > VALUES (DEFAULT, "", NULL, "Smythe", "", 'Company');
> > ```
> >
> > 1. `DEFAULT`: `CustID` will auto +1
> > 2. `NULL`: Unknown or undefined value, NOT equivalent to zero or an empty string
> > 3. `""`: Empty string
>
> ## 1.3. Query Table: `SELECT`
>
> > **1️⃣**Simplest form of an SQL query
> >
> > ```sql
> >SELECT * from Customer   -- * means we want all columns from the 'Customer' table
> > ```
> > 
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401074237703.png" alt="image-20240401074237703" style="zoom: 45%;" /> 
> >
> > **2️⃣**Other Types of `SELECT`
> >
> > | Clause   | Description                                                  |
> >| -------- | ------------------------------------------------------------ |
> > | `SELECT` | Columns that are returned from the query                     |
> > | `FROM`   | Tables where the data is obtained                            |
> > | `WHERE`  | Conditions to choose a row                                   |
> > | GROUP BY | Indicate categorization of results                           |
> > | HAVING   | Indicate the conditions under which a particular category (group) is included in the result |
> > | ORDER BY | Sort the result based on the criteria                        |
> > | LIMIT    | Limit which rows are returned by their return order (e.g., 5 rows, 5 rows from row 2) |
> > 
>
> ## 1.4. `FROM` and `WHERE`
>
> > **1️⃣**Projection: $\pi_{CustLastName}(Customer)$​
> >
> > ```sql
> > SELECT CustLastName FROM Customer;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401074608362.png" alt="image-20240401074608362" style="zoom:55%;" /> 
> >
> > 1. In SQL, projection operation will not remove duplicate items due to high cost
> >
> > 2. If need to eliminate duplicates in results, then
> >
> >    ```sql
> >    SELECT DISTINCT CustLastName FROM Customer;
> >    ```
> >
> > **2️⃣**Selection: $\pi_{CustLastName}(\sigma_{CustLastName}(Smith))$
> >
> > ```sql
> > SELECT CustLastName FROM Customer
> > WHERE CustLastName = “Smith”;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401074956161.png" alt="image-20240401074956161" style="zoom: 58%;" /> 
>
> ## 1.5. `LIKE` clause (子句) 
>
> > **1️⃣**Overview
> >
> > | Clause                            | `CustomerName` Maches                        |
> > | :-------------------------------- | :------------------------------------------- |
> > | `WHERE CustomerName LIKE 'a%'`    | axxxxx, Start with a                         |
> > | `WHERE CustomerName LIKE '%a'`    | xxxxxa, End with a                           |
> > | `WHERE CustomerName LIKE '%or%'`  | orxxxx, Contain or                           |
> > | `WHERE CustomerName LIKE '_r%'`   | xrxxxx, Have r in the second place           |
> > | `WHERE CustomerName LIKE 'a_%_%'` | axx, Start with a and at least 3-char length |
> > | `WHERE CustomerName LIKE 'a%o'`   | axxxxo, Start with a end with o              |
> >
> > **2️⃣**E.g.
> >
> > ```sql
> > SELECT CustLastName FROM Customer
> > WHERE CustLastName LIKE "Sm%"
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401080148234.png" alt="image-20240401080148234" style="zoom: 67%;" />  
>
> ## 1.6. `AS` Clause
>
> > **1️⃣**Without Renaming
> >
> > ```sql
> > SELECT CustType, COUNT(CustomerID)
> > FROM Customer
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401083141323.png" alt="image-20240401083141323" style="zoom:50%;" /> 
> >
> > **2️⃣**Renaming
> >
> > ```sql
> > SELECT CustType, COUNT(CustomerID) AS Count
> > FROM Customer
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401083220735.png" alt="image-20240401083220735" style="zoom:52%;" /> 
>
> ## 1.7. Aggregate (聚合) functions
>
> > ### 1.7.1. Overview
> >
> > > **1️⃣**Function: Condense (压缩) large datasets into meaningful single outputs
> > >
> > > **2️⃣**Type:
> > >
> > > | Function  |   Description    |
> > > | :-------: | :--------------: |
> > > |  `AVG()`  |  Average value   |
> > > |  `MIN()`  |  Minimum value   |
> > > |  `MAX()`  |  Maximum value   |
> > > | `COUNT()` | Number of values |
> > > |  `SUM()`  |  Sum of values   |
> > >
> > > **3️⃣**Note: These operations won't include NULL value, EXCEPT `COUNT()`
> >
> > ### 1.7.2. Examples
> >
> > > | SQL Query                                                    | Description                               |
> > > | ------------------------------------------------------------ | ----------------------------------------- |
> > > | `SELECT COUNT(CustomerID) FROM Customer;`                    | How many CustomerIDs                      |
> > > | `SELECT AVG(OutstandingBalance) FROM Account;`               | Average balance of ALL ACCOUNTS           |
> > > | `SELECT AVG(OutstandingBalance) FROM Account WHERE CustomerID=1;` | Average balance of Accounts of Customer 1 |
> > > | `SELECT AVG(OutstandingBalance) FROM Account GROUP BY CustomerID;` | Average balance EACH CUSTOMER             |
> > >
> > > `GROUP BY`: Divide the records into groups by certain rules, here customers are grouped by IDs (each one is a group)
>
> ## 1.8. `GROUP BY` clause
>
> > **1️⃣**Function: 
> >
> > 1. Organizes records into groups based on one or more attributes
> > 2. Conjunction with aggregate functions to compute a single result from each group
> >
> > **2️⃣**E.g. 
> >
> > ```sql
> > SELECT AVG(OutstandingBalance) 
> > FROM Account 
> > GROUP BY CustomerID;
> > ```
> >
> > 1. Each customer have one to many accounts, then form these accounts into one group
> > 2. Calculate every customer's accounts' average balance
>
> ## 1.9. `HAVING` Clause
>
> > **1️⃣**Function: Substitute for `WHERE` in aggregate function
> >
> > 1. Use `GROUP BY` to group customers
> > 2. Use `HAVING` to filter these groups
> >
> > **2️⃣**E.g. List countries with more than 5 accounts
> >
> > ```sql
> > SELECT COUNT(CustomerID), CountryName
> > FROM Customers
> > GROUP BY CountryName
> > HAVING COUNT(CustomerID) > 5;
> > ```
>
> ## 1.10. `ORDER BY` Clause
>
> > **1️⃣**To sort the result in ascending (ASC升序)
> >
> > ```sql
> > SELECT CustLastName, CustType
> > FROM Customer
> > ORDER BY CustLastName;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401205750838.png" alt="image-20240401205750838" style="zoom: 33%;" /> 
> >
> > **2️⃣**To sort the result in descending (DESC降序)
> >
> > ```sql
> > SELECT CustLastName, CustType
> > FROM Customer
> > ORDER BY CustLastName DESC;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401210129167.png" alt="image-20240401210129167" style="zoom: 33%;" /> 
> >
> > **3️⃣**To sort the result in d `Custname` DESC and`Custtype` ASC
> >
> > ```sql
> > SELECT CustLastName, CustType
> > FROM Customer
> > ORDER BY CustLastName DESC, CustType ASC;
> > ```
>
> ## 1.11. `LIMIT` and `OFFSET`
>
> > **1️⃣**List the first N items, <font color='red'>Result in red</font>
> >
> > ```sql
> > SELECT CustLastName, CustType
> > FROM Customer
> > ORDER BY CustLastName
> > LIMIT 5;
> > ```
> >
> > **2️⃣**List the middle N items, <font color='grenn'>Result in green</font>
> >
> > ```sql
> > SELECT CustLastName, CustType
> > FROM Customer
> > ORDER BY CustLastName
> > LIMIT 5 OFFSET 3;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401221007902.png" alt="image-20240401221007902" style="zoom:50%;" /> 

# 2. Inter-table Operations

> ## 2.1. Cross Product
>
> > ```sql
> > SELECT * FROM Customer, Account;	-- Cross product between Customer, Account
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401221511400.png" alt="image-20240401221511400" style="zoom: 67%;" /> 
>
> ## 2.2. Inner (Equi) Join
>
> > **1️⃣**Articulated using the `INNER JOIN` keyword
> >
> > **2️⃣**Followed by the condition stated after the `ON` keyword
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401231354398.png" alt="image-20240401231354398" style="zoom:60%;" /> 
> >
> > ```sql
> > SELECT * FROM Customer INNER JOIN Account
> > ON Customer.CustomerID = Account.CustomerID;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401222224473.png" alt="image-20240401222224473" style="zoom: 60%;" /> 
>
> ## 2.3. Natural Join
>
> > **1️⃣**The condition is inferred, doesn't require an explicit condition to be stated
> >
> > **2️⃣**Natural joins are contingent on (依赖于) the exact matching (准确匹配) of attribute
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401231354398.png" alt="image-20240401231354398" style="zoom:60%;" /> 
> >
> > ```sql
> > SELECT * FROM Customer NATURAL JOIN Account
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401222626944.png" alt="image-20240401222626944" style="zoom: 55%;" />  
>
> ## 2.4. Outer Join: 
>
> > **1️⃣**Core: Ensure all records from one side of the join are returned, Left outer join for exmaple
> >
> > 1. Include every record from the left table, 
> > 2. if there's no matching record in the right table, the result will display NULL
> >
> > **2️⃣**Left outer join: The Akin without account will display as NULL
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401231458362.png" alt="image-20240401231458362" style="zoom:60%;" /> 
> >
> > ```sql
> > SELECT * FROM Customer LEFT OUTER JOIN Account
> > ON Customer.CustomerID = Account.CustomerID;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401230151917.png" alt="image-20240401230151917" style="zoom: 33%;" /> 
> >
> > **3️⃣**Right outer join: Accounts without customers will appear NULL (not depicted here)
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401231519010.png" alt="image-20240401231519010" style="zoom:60%;" /> 
> >
> > ```sql
> > SELECT * FROM Customer RIGHT OUTER JOIN Account
> > ON Customer.CustomerID = Account.CustomerID;
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401222224473.png" alt="image-20240401222224473" style="zoom: 60%;" /> 
> >
> > **4️⃣**Full outer join: Ensures that all matching records are included in the result set
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401232140498.png" alt="image-20240401232140498" style="zoom:60%;" /> 
> >
> > ```sql
> > SELECT * FROM Customer FULL OUTER JOIN Account
> > ON Customer.CustomerID = Account.CustomerID;
> > ```

# 3. Example

> ## 3.1. ​Condition
>
> > **1️⃣**Customer Table
> >
> > | CID  | Attribute 1 | Attribute 2 |
> > | :--: | :---------: | :---------: |
> > |  1   |     X1      |     Y1      |
> > |  2   |     X2      |     Y2      |
> > |  3   |     X3      |     Y3      |
> >
> > **2️⃣**Account Table
> >
> > | CID  | AID  | Attribute 3 |
> > | :--: | :--: | :---------: |
> > |  1   |  1   |     Z1      |
> > |  1   |  2   |     Z2      |
> > |  2   |  3   |     Z3      |
> > | NULL |  4   |     Z4      |
>
> ## 3.2. Inner Join: Matching `Customer IDs` 
>
> > | CID  | Attribute 1 | Attribute 2 | AID  | Attribute 3 |
> > | :--: | :---------: | :---------: | :--: | :---------: |
> > |  1   |     X1      |     Y1      |  1   |     Z1      |
> > |  1   |     X1      |     Y1      |  2   |     Z2      |
> > |  2   |     X2      |     Y2      |  3   |     Z3      |
>
> ## 3.3. Left Outer Join: All Customer Table Recorded
>
> > | CID  | Attribute 1 | Attribute 2 | AID  | Attribute 3 |
> > | :--: | :---------: | :---------: | :--: | :---------: |
> > |  1   |     X1      |     Y1      |  1   |     Z1      |
> > |  1   |     X1      |     Y1      |  2   |     Z2      |
> > |  2   |     X2      |     Y2      |  3   |     Z3      |
> > |  3   |     X3      |     Y3      | NULL |    NULL     |
>
> ## 3.4. Right Outer Join: All Account Table Recorded
>
> > | CID  | Attribute 1 | Attribute 2 | AID  | Attribute 3 |
> > | :--: | :---------: | :---------: | :--: | :---------: |
> > |  1   |     X1      |     Y1      |  1   |     Z1      |
> > |  1   |     X1      |     Y1      |  2   |     Z2      |
> > |  2   |     X2      |     Y2      |  3   |     Z3      |
> > | NULL |    NULL     |    NULL     |  4   |     Z4      |
>
> ## 3.5. Full Outer Join
>
> > | CID  | Attribute 1 | Attribute 2 | AID  | Attribute 3 |
> > | :--: | :---------: | :---------: | :--: | :---------: |
> > |  1   |     X1      |     Y1      |  1   |     Z1      |
> > |  1   |     X1      |     Y1      |  2   |     Z2      |
> > |  2   |     X2      |     Y2      |  3   |     Z3      |
> > |  3   |     X3      |     Y3      | NULL |    NULL     |
> > | NULL |    NULL     |    NULL     |  4   |     Z4      |

