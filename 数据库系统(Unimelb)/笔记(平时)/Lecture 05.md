# L05 Modelling with MySQL Workbench

# 1. Crow's Foot notation (Workbench)

> ## 1.1. Entity
>
> > **1️⃣**Diagram Structure: 
> >
> > 1. Entity: Represented as a rectangle
> > 2. Attributes: Placed within the entity, with following types
> >
> > **2️⃣**Strong / Weak Entity:  distinguished by ability to exist independently
> >
> > |  Type  | Connection  |        Description         |                             E.g.                             |
> > | :----: | :---------: | :------------------------: | :----------------------------------------------------------: |
> > | Strong | Dashed line |    Showing Independence    | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326014303663.png" alt="image-20240326014303663" style="zoom: 33%;" /> |
> > |  Weak  | Solid lines | Mark identify relationship | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326014018622.png" alt="image-20240326014018622" style="zoom:33%;" /> |
> >
> ## 1.2. Attributes
>
> > **1️⃣**Attributes types
> >
> > |     Type      |   Format   |     Icon      |
> > | :-----------: | :--------: | :-----------: |
> > |   Mandatory   | `NOT NULL` | Blue diamond  |
> > |   Optional    |   `NULL`   | Empty diamond |
> > | Derived(衍生) |    `[]`    | Empty diamond |
> > |  Multivalued  |    `{}`    | Empty diamond |
> > |   Composite   |    `()`    | Empty diamond |
> >
> > **2️⃣**Identifier(Key) Attribute: Every entity have an identifier, represented as a  ==yellow key icon==
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325212407255.png" alt="image-20240325212407255" style="zoom: 50%;" /> 
> >
> > **3️⃣**P.s. Derived attributes
> >
> > 1. Core Feature: 
> >
> >    - Derived attributes' values can be derived from some other  attributes
> >
> >    - Do not need to be stored  physically$\xrightarrow{}$Disappear at the physical design
> >
> > 2. E.g. Years employed can be derived from `DATE-now - Contact start`<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325215010330.png" alt="image-20240325215010330" style="zoom:60%;" />  
>
> ## 1.3. Relationship: Line connecting two entities
> >
> >**1️⃣**Overview
> >
> >|      Type      | Entity Participation | Key Constraint |                      Connection Symbol                       |
> >| :------------: | :------------------: | :------------: | :----------------------------------------------------------: |
> >| Optional Many  |       Partial        |    Without     | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325220038295.png" alt="image-20240325220038295" style="zoom: 15%;" /> |
> >| Mandatory Many |        Total         |    Without     | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325220117554.png" alt="image-20240325220117554" style="zoom:15%;" /> |
> >|  Optional One  |       Partial        |      With      | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325220252691.png" alt="image-20240325220252691" style="zoom:15%;" /> |
> >| Mandatory One  |        Total         |      With      | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325220337291.png" alt="image-20240325220337291" style="zoom: 15%;" /> |
> >
> >**2️⃣**E.g.
> >
> >|      Type      | Prof may teach___lecture |
> >| :------------: | :----------------------: |
> >| Optional Many  |           0/N            |
> >| Mandatory Many |           1/N            |
> >|  Optional One  |           0/1            |
> >| Mandatory One  |            1             |
> >
> >**3️⃣**Symbol consists of two parts:
> >
> >|        |                           Optional                           |                          Mandatory                           |                             Many                             |                             One                              |
> >| :----: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
> >| Part 1 | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325222130681.png" alt="image-20240325222130681" style="zoom:25%;" /> | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325222322092.png" alt="image-20240325222322092" style="zoom:25%;" /> |                             N/A                              |                             N/A                              |
> >| Part 2 |                             N/A                              |                             N/A                              | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325222453184.png" alt="image-20240325222453184" style="zoom:25%;" /> | <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325222526165.png" alt="image-20240325222526165" style="zoom:25%;" /> |
> >
>
> ## 1.4. Difference in Cardinalities(基数) Placement
>
> > **1️⃣**Chen's: On the side of the entity. 
> >
> >  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325234821273.png" alt="image-20240325234821273" style="zoom: 50%;" /> 
> >
> > **2️⃣**Crow's: Opposite end of the line
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240325234836617.png" alt="image-20240325234836617" style="zoom: 50%;" /> 

# 2. Conversion in $\text{1-M/M-M}$ relationships

> Conceptual$\xrightarrow{\text{Check and complete the following tasks}}$Logical/Physical$\to$​Implementation  
>
> | Task                       | Solution                                               |
> | -------------------------- | ------------------------------------------------------ |
> | Composite attributes       | **Flatten**                                            |
> | Multi-valued attributes    | **Flatten**, multi-valued attributes$\to$another table |
> | $\text{1-M}$ relationships | Add foreign key at many side                           |
> | $\text{M-M}$ relationships | Create an associative entity                           |
>
> ## 2.1. Composite attributes
>
> > **1️⃣**Conceptual$\xrightarrow{\text{Flatten}}$Logical$\xrightarrow[\text{Determining mandatory status}]{\text{Add data type}}$Physical
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326020143715.png" alt="image-20240326020143715" style="zoom: 30%;" /> $\text{→}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326212521898.png" alt="image-20240326212521898" style="zoom: 40%;" /> 
> > 
> > **2️⃣**Implementation
> > 
> > ```sql
> > CREATE TABLE Customer(
> >  CustomerID INT NOT NULL,                -- 客户的唯一标识符，不允许为空
> >   CustFirstName VARCHAR(100),             -- 客户的名字
> >  CustMiddleName VARCHAR(100),            -- 客户的中间名
> >   CustLastName VARCHAR(100) NOT NULL,     -- 客户的姓氏，不允许为空
> >  BussinessName VARCHAR(100),             -- 客户的商业名称
> >   CustType VARCHAR(1) NOT NULL,           -- 客户的类型，不允许为空
> >  CustAddressLine1 VARCHAR(100) NOT NULL, -- 客户地址第一行，不允许为空
> >   CustAddressLine2 VARCHAR(100) NOT NULL, -- 客户地址第二行，不允许为空
> >  CustSuburb VARCHAR(60) NOT NULL,        -- 客户的郊区，不允许为空
> >   CustPostcode CHAR(6) NOT NULL,          -- 客户的邮政编码，不允许为空，固定长度为6
> >   CustCountry VARCHAR(60) NOT NULL,       -- 客户的国家，不允许为空
> >   PRIMARY KEY (CustomerID)                -- 设置CustomerID为主键
> > );
> > ```
>
> ## 2.2. Multi-Valued Attributes
>
> > ### 2.2.1. Small, Fixed Number of Values: Flatten
> >
> > > Conceptual$\to$Logical$\to$Physical
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327002557281.png" alt="image-20240327002557281" style="zoom:40%;" />$\xrightarrow{\text{Extract Weak Entity}}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327002600332.png" alt="image-20240327002600332" style="zoom:40%;" />$\xrightarrow{\text{Add Data Type}}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327002603378.png" alt="image-20240327002603378" style="zoom:40%;" />
> >
> > ### 2.2.2. Many Values
> >
> > > **1️⃣** One Employee$\xrightarrow{\text{Plays}}$N Roles: Lookup Table
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327042206853.png" alt="image-20240327042206853" style="zoom:50%;" /> 
> > >
> > > 1. Create `Role` Entity
> > > 2.  Every  `Role` has $\text{FK}$ `employee_ID` 
> > >
> > > **2️⃣**N Employees$\xrightarrow{\text{Plays}}$N Roles: Associate Entity
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327044213850.png" alt="image-20240327044213850" style="zoom: 33%;" /> 
> > >
> > > 1. Create `Role` Entity & `Associate` Entity
> > > 2. Set  `employee_ID` and `role_ID` to be $\text{PFK}$
>
> ## 2.3. $\text{1-M}$ Relationship: Customer with many accounts
>
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326231637921.png" alt="image-20240326231637921" style="zoom: 45%;" /> 
> >
> > **1️⃣**Conceptual$\xrightarrow{\text{Add foreign key at many side}}$Logical: Every `Account` must have a `Customer_ID`
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326232238005.png" alt="image-20240326232238005" style="zoom: 50%;" /> $\xrightarrow{\begin{cases}\text{Foreign Key: Customer ID}\\\\\text{Primary Key: Account ID}\end{cases}}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326232828394.png" alt="image-20240326232828394" style="zoom:50%;" />
> >
> > **2️⃣**Rest Steps
> >
> > 1. Physical design
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240326233114066.png" alt="image-20240326233114066" style="zoom:50%;" /> 
> >
> > 2. Implementation
> >
> >    ```sql
> >    CREATE TABLE Account (
> >        AccountID smallint AUTO_INCREMENT,
> >        AccountName varchar(100) NOT NULL,
> >        OutstandingBalance DECIMAL(10,2) NOT NULL,
> >        CustomerID smallint NOT NULL,
> >        PRIMARY KEY (AccountID),
> >        FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
> >            ON DELETE RESTRICT
> >            ON UPDATE CASCADE
> >    ) ENGINE=InnoDB;
> >    ```
> >    
>
> ## 2.4. $\text{M-M}$​ Relationship: Customer Addresses
>
> > **1️⃣**Conceptual level: `AddressDateFrom/To` are descriptive attributes 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327051027986.png" alt="image-20240327051027986" style="zoom: 70%;" /> 
> >
> > **2️⃣**To Logical: Create an **Associative** **Entity** 
> >
> > 1. Including ALL the descriptive attributes: To avoid primary key violation(冲突), if customer used to living in one place twice
> > 2. Incorporating keys from BOTH sides
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327051825870.png" alt="image-20240327051825870" style="zoom:50%;" /> 
> >
> > **3️⃣**Implementation
> >
> > ```sql
> > CREATE TABLE CustomerAddress (
> >     CustomerID smallint,
> >     AddressID smallint,
> >     AddressDateFrom DATE,
> >     AddressDateTo DATE,
> >     PRIMARY KEY (CustomerID, AddressID, AddressDateFrom),
> >     FOREIGN KEY (CustomerID) REFERENCES Customer(CustomerID)
> >         ON DELETE RESTRICT
> >         ON UPDATE CASCADE,
> >     FOREIGN KEY (AddressID) REFERENCES Address(AddressID)
> >         ON DELETE RESTRICT
> >         ON UPDATE CASCADE
> > ) ENGINE=InnoDB;
> > ```

# 3. Unary Relationships

> ## 3.1. Unary $\text{1-1}$ Relationships
>
> > **1️⃣**Unary Relationship: Occurs between two entities of the same type
> >
> > **2️⃣**Rule
> >
> > 1. $\text{1-1}$ and $\text{1-N}$: Put a Foreign key in the relation
> > 2. $\text{N-N}$: Generate an Associative Entity
> >
> > **3️⃣**E.g. 
> >
> > 1. Conceptual$\to{}$Logical: Optional One
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327070444793.png" alt="image-20240327070444793" style="zoom:50%;" /> $\to$​​  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327070603097.png" alt="image-20240327070603097" style="zoom:50%;" /> Spouse—配偶
> >
> >    - Transfer the ID from one person to the other, and rename it
> >    - Spouse ID is optional, means not everyone get married
> >
> > 2. Implementation
> >
> >    ```sql
> >    CREATE TABLE Person (
> >        ID INT NOT NULL,
> >        Name VARCHAR(100) NOT NULL,
> >        DateOfBirth DATE NOT NULL,
> >        SpouseID INT,
> >        PRIMARY KEY (ID),
> >        FOREIGN KEY (SpouseID) REFERENCES Person (ID)
> >            ON DELETE RESTRICT
> >            ON UPDATE CASCADE
> >    );
> >    ```
>
> ## 3.2. Unary $\text{1-N}$​ Relationships
>
> > **1️⃣**Rule: Put a Foreign key in the relation
> >
> > **2️⃣**E.g. Some of the employees can be managers
> >
> > 1. Conceptual$\to{}$​Logical
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331203754056.png" alt="image-20240331203754056" style="zoom: 67%;" /> $\to{}$ <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331203901858.png" alt="image-20240331203901858" style="zoom: 67%;" />  
> >
> > 2. implementation
> >
> >    ```sql
> >    CREATE TABLE Employee (
> >        ID smallint NOT NULL,
> >        Name VARCHAR(100) NOT NULL,
> >        DateOfBirth DATE NOT NULL,
> >        ManagerID smallint,
> >        PRIMARY KEY (ID),
> >        FOREIGN KEY (ManagerID) REFERENCES Employee(ID)
> >            ON DELETE RESTRICT
> >            ON UPDATE CASCADE
> >    );
> >    ```
>
> ## 3.3. Unary $\text{N-N}$​ Relationships: Unusual
>
> > **1️⃣**Rule: Create Associative Entity like usual
> >
> > **2️⃣**E.g. One item has many other items, and can be one of the items of one bigger item
> >
> > 1. A chair might be made up of four legs and eight screws(螺丝)
> > 2. A chair can becomes part of a dining set
> >
> > **3️⃣**Conceptual$\to{}$​​Logical: 
> >
> > 1. Create Associative Entity `ComponentID`
> > 2. Move the descriptive attributes to the new entity
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331211414035.png" alt="image-20240331211414035" style="zoom: 40%;" /> $\to{}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331211449075.png" alt="image-20240331211449075" style="zoom: 33%;" />
> >
> > **4️⃣**Implementation
> >
> > ```sql
> > CREATE TABLE item (
> >     ID smallint,
> >     Name VARCHAR(100) NOT NULL,
> >     UnitCost DECIMAL(6,2) NOT NULL,
> >     PRIMARY KEY (ID)
> > ) ENGINE=InnoDB;
> > ```
> >
> > ```sql
> > CREATE TABLE Component (
> >     ID smallint,
> >     ComponentID smallint,
> >     Quantity smallint NOT NULL,
> >     PRIMARY KEY (ID, ComponentID),
> >     FOREIGN KEY (ID) REFERENCES Part(ID)
> >         ON DELETE RESTRICT
> >         ON UPDATE CASCADE,
> >     FOREIGN KEY (ComponentID) REFERENCES Part(ID)
> >         ON DELETE RESTRICT
> >         ON UPDATE CASCADE
> > ) ENGINE=InnoDB;
> > ```
> >
> > 

# 4. Binary Relationship

> **1️⃣**Rule: Move the **KYE** to the
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327062800755.png" alt="image-20240327062800755" style="zoom: 38%;" /> 
>
> 1. In 1-M relationship
>    - Crow's: PK from ONE side$\to$FK at MANY side
>    - Chen's: PK from MANY side$\to$​FK at ONE side
> 2. P.s. For M-M Relationship: Create an Associative Entity 
>
> **2️⃣**E.g. 
>
> 1. Conceptual: Move the key to the Optional side (Care center)
>
>    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327062939512.png" alt="image-20240327062939512" style="zoom: 45%;" />  
>
> 2. Logical
>
>    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240327063020177.png" alt="image-20240327063020177" style="zoom: 40%;" /> 

# 5. Ternary $\text{N-N}$ relationships

> **1️⃣**E.g. Werehouse(仓库)—Item—Supplier relationship
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331214447731.png" alt="image-20240331214447731" style="zoom: 67%;" /> 
>
> 1. A supplier can dispatch(发送) multiple items to various warehouses
> 2. A item can be dispatch by different supplier to various warehouses
> 3. A Warehouse contains many items form various supplier
>
> **2️⃣**Logical Design: Generate an Associative Entity
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331214523119.png" alt="image-20240331214523119" style="zoom: 40%;" /> 

# 6. Weak Entity (Identifying Relationship)

> **1️⃣**Rule
>
> 1. Weak entity incorporate the strong entity's identifier into its identifier (Foreign Key)
> 2. Foreign Key becomes part of the Primary Key
>
> **2️⃣**E.g. Loan and its payment
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331220340189.png" alt="image-20240331220340189" style="zoom: 40%;" /> 



![image-20240331220552529](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240331220552529.png)