# Lecture 9 SQL Summary

# 0. Things about SQL

> **1️⃣**SQL keywords & Attribute names  are case insensitive, but CAPITALISE them to make them clear
>
> ```sql
> ACCOUNTID == AccountID == AcCoUnTID
> ```
>
> **2️⃣**Table names are Operating System Sensitive
>
> **3️⃣**You can do maths in SQL, not only data retrieval (检索)

# 1. DML

> ## 1.1. Comparison & Logic Operators
>
> > **1️⃣**Comparison Operator
> >
> > | Operator | Description                                  |
> > | -------- | -------------------------------------------- |
> > | =        | Equal to                                     |
> > | <        | Less than                                    |
> > | >        | Greater than                                 |
> > | <=       | Less than or equal to                        |
> > | >=       | Greater than or equal to                     |
> > | <> OR != | Not equal to (depends on DBMS which is used) |
> >
> > **2️⃣**Logical Operator: `AND` `NOT` `OR`
> >
> > 1. Priority: `NOT > AND > OR`, BUT using `()` is better
> >
> > 2. E.g. 
> >
> >    ```sql
> >    SELECT * FROM Furniture WHERE 
> >    (Type='Chair' AND Colour = 'Black') OR (Type = 'Lamp' AND Colour = 'Black');
> >    ```
>
> ## 1.2. Set Operations
>
> >**1️⃣**Overview
> >
> >|    Operation    | Operation                       | For duplicate rows         | SQL ? |
> >| :-------------: | ------------------------------- | -------------------------- | :---: |
> >|     `UNION`     | Merge two results from `SELECT` | Eliminate                  |   √   |
> >|   `UNION ALL`   | Merge two results from `SELECT` | Reserve, include each copy |   √   |
> >|   `INTERSECT`   | Return rows in both tables      | N/A                        |   ×   |
> >| `INTERSECT ALL` | The same as `INTERSECT`         |                            |   ×   |
> >
> >**2️⃣**Notice
> >
> >1. `UNION `can be applied to any two result sets
> >2. But the two results must have same columns, and each column's data type are the same
> >
> >**2️⃣**E.g. Queries joining different tables, the output schema remains consistent
> >
> >```sql
> >SELECT Employee.Name, EmployeeType FROM Employee 
> >INNER JOIN Hourly
> >ON Employee.ID = Hourly.ID
> >
> >UNION
> >
> >SELECT Employee.Name, EmployeeType FROM Employee 
> >INNER JOIN Salaried
> >ON Employee.ID = Salaried.ID;
> >```
> >
> ><img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240402025626422.png" alt="image-20240402025626422" style="zoom: 33%;" /> 
>
> ## 1.3. Subquery
>
> > ### 1.3.1. Overview
> >
> > > **1️⃣**Query Nesting (嵌套) / Nest subqueries
> > >
> > > 1. SELECT statement placed within a larger query
> > > 2. Inner queries are executed first, then outer query can utilize
> > >
> > > **2️⃣**Operators
> > >
> > > | Operator | True if                                                   |
> > > | -------- | --------------------------------------------------------- |
> > > | `IN`     | Attribute is in the subquery list                         |
> > > | `NOT IN` | Attribute is not in the subquery list                     |
> > > | `ANY`    | Any single value returned by subquery meets the condition |
> > > | `ALL`    | All values returned by subquery meet the condition        |
> > > | `EXISTS` | Subquery returns one or more records                      |
> > >
> > > **3️⃣**Settings of the example: Auction Bids (拍卖出价)
> > >
> > > 1. Physical Model 
> > >
> > >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240402032453333.png" alt="image-20240402032453333" style="zoom:38%;" /> 
> > >
> > > 2. Tables
> > >
> > >    | Artefact ID | Name  | Description |
> > >    | ----------- | ----- | ----------- |
> > >    | 1           | Vase  | Old Vase    |
> > >    | 2           | Knife | Old Knife   |
> > >    | 3           | Pot   | Old Pot     |
> > >
> > >    | SellerID | Name | Phone      |
> > >    | -------- | ---- | ---------- |
> > >    | 1        | Abby | 0232323232 |
> > >    | 2        | Ben  | 0311111111 |
> > >    | 3        | Carl | 0333333333 |
> > >
> > >    | BuyerID | Name   | Phone      |
> > >    | ------- | ------ | ---------- |
> > >    | 1       | Maggie | 0333333333 |
> > >    | 2       | Nicole | 0444444444 |
> > >    | 3       | Oleg   | 0555555555 |
> > >
> > >    | SellerID | ArtefactID | BuyerID | Date       | Amount   | Acceptance |
> > >    | -------- | ---------- | ------- | ---------- | -------- | ---------- |
> > >    | 1        | 1          | 1       | 2012-06-20 | 81223.23 | N          |
> > >    | 1        | 1          | 2       | 2012-06-20 | 82223.23 | N          |
> > >    | 2        | 1          | 1       | 2012-06-20 | 19.95    | N          |
> > >    | 2        | 2          | 2       | 2012-06-20 | 23.00    | N          |
> >
> > ### 1.3.2.  `IN / NOT IN`
> >
> > > **1️⃣**E.g. 1 Find buyers who bidders on artefact 1
> > >
> > > ```sql
> > > SELECT * FROM Buyer
> > > WHERE BuyerID IN
> > > (SELECT BuyerID FROM Offer WHERE ArtefactID = 1)
> > > ```
> > >
> > > 1. Extract the BuyerIDs from the Offer table where the ArtefactID is 1$\to{}$Return BuyerID=1,2 
> > >
> > > 2. BuyerID=1,2 are utilized by the outer query$\to{}$​Select records from ='Buyer' table
> > >
> > > 3. Result
> > >
> > >    | BuyerID | Name   | Phone      |
> > >    | ------- | ------ | ---------- |
> > >    | 1       | Maggie | 0333333333 |
> > >    | 2       | Nicole | 0444444444 |
> > >
> > > **2️⃣**E.g. 2
> > >
> > > ```sql
> > > SELECT * FROM Artefact
> > > WHERE ID NOT IN
> > > (SELECT ArtefactID FROM Offer);
> > > ```
> > >
> > > 1. `(SELECT ArtefactID FROM Offer)`：ArtefactID = 1/2
> > >
> > > 2. `WHERE ID NOT IN (...)`: ArtefactID = 3
> > >
> > > 3. `SELECT * FROM Artefact....`: 
> > >
> > >    | Artefact ID | Name | Description |
> > >    | ----------- | ---- | ----------- |
> > >    | 3           | Pot  | Old Pot     |
> > >
> > > ➕A more efficient way compared to `IN`
> > >
> > > ```sql
> > > SELECT * FROM Buyer
> > > WHERE BuyerID IN (SELECT BuyerID FROM Offer WHERE ArtefactID = 1)
> > > 
> > > -- Equals to
> > > 
> > > SELECT BuyerID, Name, Phone
> > > FROM Buyer NATURAL JOIN Offer
> > > WHERE ArtefactID = 1
> > > ```
> >
> > ### 1.3.3. `EXISIS`: Buyers bidders on artefact 1
> >
> > > ```sql
> > > SELECT * FROM Buyer WHERE EXISTS
> > > (SELECT * FROM Offer WHERE Buyer.BuyerID = Offer.BuyerID AND ArtefactID = 1)
> > > ```
> > >
> > > **1️⃣**Interplay between outer / inner queries:  
> > >
> > > 1. Each record in the outer table will execute to see if there are any matching records
> > > 2. If the inner query returns at least one record, outer query keeps that row
> > >
> > > **2️⃣**In example
> > >
> > > 1. Iterate(遍历) through each buyer in the Buyer table
> > >
> > > 2. For each buyer, check the Offer table to see if meet the condition:
> > >
> > >    ```sql
> > >    Buyer.BuyerID = Offer.BuyerID AND ArtefactID = 1
> > >    ```
> > >
> > > 3. Result
> > >
> > >    | BuyerID | Name   | Phone      |
> > >    | ------- | ------ | ---------- |
> > >    | 1       | Maggie | 0333333333 |
> > >    | 2       | Nicole | 0444444444 |
> >
> > ### 1.3.4. `ANY/ALL/EXISTS` (differences)
> >
> > > **1️⃣**`ALL`: Aatisfy **all** inner conditions
> > >
> > > ```sql
> > > SELECT empno, sal FROM emp
> > > WHERE sal > ALL (200, 300, 400);
> > > -- equals to
> > > SELECT empno, sal FROM emp
> > > WHERE sal > 200 AND sal > 300 AND sal> 400;
> > > ```
> > >
> > > **2️⃣**`ANY`: Satisfy **at** **least** **one** **of** the inner conditions
> > >
> > > ```sql
> > > SELECT empno, sal FROM emp
> > > WHERE sal > ANY (200, 300, 400);
> > > -- equals to
> > > SELECT empno, sal FROM emp
> > > WHERE sal > 200 OR sal > 300 OR sal> 400;
> > > ```
> > >
> > > **3️⃣**Inner query returns **at** **least** **one** record
> > >
> > > ```sql
> > > SELECT empid, first_name, last_name FROM employees AS E
> > > -- Subquery: Employees who have at least one dependent
> > > WHERE EXISTS (         
> > >   SELECT * FROM dependents AS D
> > >   WHERE D.empid = E.empid
> > > );
> > > ```
>
> ## 1.4. Multiple record INSERTs, INSERT from a table
>
> > **1️⃣**Insert an entire table to another
> >
> > ```sql
> > INSERT INTO NewEmployee
> > SELECT * FROM Employee;
> > ```
> >
> > Copy all records from the `Employee` table to the `NewEmployee` table.
> >
> > **2️⃣**Inserting multiple records
> >
> > 1. Type1
> >
> >    ```sql
> >    INSERT INTO Employee VALUES
> >    (DEFAULT, "A", "A's Addr", "2012-02-02", NULL, "S"),
> >    (DEFAULT, "B", "B's Addr", "2012-02-02", NULL, "S"),
> >    (DEFAULT, "C", "C's Addr", "2012-02-02", NULL, "S");
> >    ```
> >
> > 2. Type2
> >
> >    ```sql
> >    INSERT INTO Employee (Name, Address, DateHired, EmployeeType) VALUES
> >    ("D", "D's Addr", "2012-02-02", "C"),
> >    ("E", "E's Addr", "2012-02-02", "C"),
> >    ("F", "F's Addr", "2012-02-02", "C");
> >    ```
>
> ## 1.5. `UPDATE`, `DELETE`, `REPLACE`
>
> > ### 1.5.1. `UPDATE`: Changes *existing* data in tables
> >
> > > **1️⃣**E.g. Execute the Block 1 first and then the Block 2
> > >
> > > ```sql
> > > -- Block1: Increase all salaries less than $100000 by 5% 
> > > UPDATE Salaried
> > > SET AnnualSalary = AnnualSalary * 1.05
> > > WHERE AnnualSalary <= 100000;
> > > 
> > > -- Block2: Increase all salaries greater than $100000 by 10% 
> > > UPDATE Salaried
> > > SET AnnualSalary = AnnualSalary * 1.10
> > > WHERE AnnualSalary > 100000;
> > > ```
> > >
> > > 1. Without a `WHERE` clause, an`UPDATE` will apply changes across the entire table
> > > 2. Sequence of the statements matters
> > >
> > > **2️⃣**`CASE` Command
> > >
> > > ```sql
> > > UPDATE Salaried
> > > SET AnnualSalary = 
> > > CASE
> > >     WHEN AnnualSalary <= 100000 THEN AnnualSalary * 1.05
> > >     ELSE AnnualSalary * 1.10    
> > > END;
> > > ```
> >
> > ### 1.5.2. `DELETE/REPLACE`
> >
> > > **1️⃣**`REPLACE`: 
> > >
> > > 1. Works identically (相同) as `INSERT`
> > > 2. EXCEPT the old / new row have the same key then it is overwritten
> > >
> > > **2️⃣**`Delete`: 
> > >
> > > 1. Dangerous command to delete all records
> > >
> > >    ```sql
> > >    DELETE FROM Employee
> > >    ```
> > >
> > > 2. Delete part of the records
> > >
> > >    ```sql
> > >    DELETE FROM Employee WHERE Name = "Grace"
> > >    ```
> > >
> > > 3. Delete targeted records only
> > >
> > >    ```sql
> > >    ON DELETE CASCADE 
> > >    ```
> > >
> > > 4. Delete targeted records and related records in different tables through foreign keys
> > >
> > >    ```sql
> > >    ON DELETE RESTRICT
> > >    ```
>
> ## 1.6. Views: Don’t physically store but show results
>
> > **1️⃣**Role
> >
> > 1. Hide the query complexity from users
> > 2. Hide data from users
> >
> > **2️⃣**Creating a View: 
> >
> > ```sql
> > CREATE VIEW <name_of_view> AS <valids_ql_statement>
> > ```
> >
> > Then
> >
> > 1. Its definition (NOT data BUT metadata) stored in the database
> > 2. Be Used just like any other table
> >
> > **3️⃣**E.g. Consolidating data from types of employees into a single virtual table named 'EmpPay'
> >
> > ```sql
> > CREATE VIEW EmpPay AS
> > 
> > SELECT Employee.ID, Employee.Name, DateHired, EmployeeType, HourlyRate AS Pay
> > FROM Employee INNER JOIN Hourly
> > ON Employee.ID = Hourly.ID
> > 
> > UNION
> > 
> > SELECT Employee.ID, Employee.Name, DateHired, EmployeeType, AnnualSalary AS Pay
> > FROM Employee INNER JOIN Salaried
> > ON Employee.ID = Salaried.ID
> > 
> > UNION
> > 
> > SELECT Employee.ID, Employee.Name, DateHired, EmployeeType, BillingRate AS Pay
> > FROM Employee INNER JOIN Consultant
> > ON Employee.ID = Consultant.ID;
> > ```
> >
> > 1. Result
> >
> >    ```sql
> >    SELECT * FROM EmpPay
> >    ```
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240402061343320.png" alt="image-20240402061343320" style="zoom:50%;" /> 
> >
> > 2. Result 2
> >
> >    ```sql
> >    ELECT * FROM EmpPay 
> >    WHERE EmployeeType = 'H' OR EmployeeType = 'C'
> >    ```
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240402061531615.png" alt="image-20240402061531615" style="zoom:55%;" /> 

# 2. DDL Commands

> **1️⃣**`ALTER`
>
> 1. Add attributes to a table
>
>    ```sql
>    ALTER TABLE <TableName> ADD <AttributeName> <AttributeType>
>    ```
>
> 2. Delete table's attributes 
>
>    ```sql
>    ALTER TABLE TableName DROP AttributeName
>    ```
>
> **2️⃣**`RENAME`: Rename a  table
>
> ```sql
> RENAME TABLE <CurrentTableName> TO <NewTableName>
> ```
>
> **3️⃣**`TRUNCATE`: Same as `DELETE * FROM table`
>
> ```sql
> TRUNCATE TABLE <table_name>
> ```
>
> 1. To swiftly remove all records from a table, bur much faster
> 2. This operation cannot rolled back
>
> **4️⃣**`DROP`: Kill a relation – removes the data, removes the relation
>
> ```sql
> DROP TABLE TableName
> ```

# 3. DCL & Other Commands

> **1️⃣**Users and permissions
>
> 1. `CREATE USER`, `DROP USER`
> 2. `GRANT`, `REVOKE`: Assigning or removing user permissions
> 3. `SET PASSWORD`
>
> **2️⃣**Database administration
>
> 1. `BACKUP TABLE`,  `RESTORE TABLE`: To recover data in the event of accidental deletions
> 2. `DESCRIBE <table_name>`: Reveals the schema of a table
> 3. `USE <db_name>`: Selects the database to work with





