# L02 Database Development Process

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240313214210240.png" alt="image-20240313214210240" style="zoom: 67%;" /> 

DB Development Lifecycle

# 1. Database Planning : A very high level

> **0️⃣**==Unimportant in this subject==
>
> **1️⃣**E.g. Planning a bank's structure (Fig below), each box has its own data inside
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240313214811894.png" alt="image-20240313214811894"  /> 

# 2. System Definition

> **0️⃣**==​Slightly outside the subject's scope==
>
> **1️⃣**Function: 
>
> 1. Specifying scope(范围) and boundaries of the system to develop
> 2. How one system interfere/communicate with other systems.
>
> **2️⃣**E.g. How mortgage(抵押) system is connected to applications within the bank.

# 3. Requirements Definition/Analysis

> ==Starting point of this subject==, collect requirements the system has

# 4. Conceptual Design ==(Core step)==

> **1️⃣**Conception: Construct model of data used in this database, independent of all physical consideration.
>
> **2️⃣**ER diagrams: Entity(实体) Relationship, data model used in this subject
>
> E.g. ER diagram of a bank, record staff details (which department and branch they are assigned)
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240313222857023.png" alt="image-20240313222857023" style="zoom: 67%;" /> 
>
> (BankHQ= Bank Head Quarter银行总部)
>
> 1. One box$\xleftrightarrow{correspond}$One entit
>
> 2. [Bank]<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240314150509032.png" alt="image-20240314150509032" style="zoom:40%;" />[Department]: Means Bank as an entity, has many Departments
>
> 3. In box has three types of diamonds:
>
>    | Diamonds |    Meaning     |
>    | :------: | :------------: |
>    |  Yellow  |    Identity    |
>    |   Blue   | Mandatory item |
>    |  White   | Optional item  |

# 5. Logical Design ==(Core steps)== 

> **1️⃣**Feature: independent to specific database
>
> **2️⃣**ER Model$\xrightarrow{Transform}$Logical Model
>
> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240314152259547.png" alt="image-20240314152259547" style="zoom:45%;" /> 
>
> 1. Machenism: The Department got idBankHQ
> 2. How to prevent insert rubbish: If we insert a Department belongs to a BankHQ that does not exist, it will be invalid 

# 6. Physical Design ==(Core steps)==

>  Implement the logical design in specific DBMS
>
>  ## 6.1. Data type Choosing ==(Most important)==
>
>  > **1️⃣**e.g. `INT/CHAR/DATE....`
>  >
>  > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240314153833099.png" alt="image-20240314153833099" style="zoom: 38%;" /> 
>  >
>  > **2️⃣**Do consider corner cases(极端个例)
>  >
>  > E.g. For postcode, string is better than integer, Postcodes do not use arithmetic operations, and some postcodes contain letters. 
>
>  ## 6.2. Data dictionary
>
>  > **1️⃣**Description: An ongoing process during analysis and design of the database
>  >
>  > **2️⃣**E.g. First line is the required information
>  >
>  > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240314222104767.png" alt="image-20240314222104767" style="zoom: 47%;" /> 
>
>  ## 6.3. Other issues
>
>  > **1️⃣**How to store “Look Up”: Currency in settlement(结算) record table as an example
>  >
>  > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240317191927058.png" alt="image-20240317191927058" style="zoom: 67%;" /> 
>  >
>  > 1. Left: Store Currency Code (e.g. USD)$\xrightarrow[\text{USD to US Dollar}]{\text{Refer to currency table}}$Currency code transfer to Currency name
>  > 2. Right: Store Currency Name (e.g. United States Dollar)
>  >
>  > **2️⃣**Normalization & Denormalization
>  >
>  > 1. Normalization(99%): Reducing redundancy (冗余) in the data model
>  > 2. Denormalizatio(1%): Introducing some duplicates in individual tables, to speed up

# 7. Application Design

>  **1️⃣**Conception: Design the interface and application programs that use and process the database
>
>  **2️⃣**P.S. interface & applications:
>
>  |                          |                       Usage                        |
>  | :----------------------: | :------------------------------------------------: |
>  | interface & applications | Using database interfaces to manipulate/visit data |
>  |           DBMS           |          Create/manage/maintain databases          |

# 8. Rest Steps

> **1️⃣**Implementation: Physical model$\xrightarrow{\text{MySQL Workbench}}$​Functioning database (alive)
>
> **2️⃣**Data Conversion and Loading: 
>
> 1. Insert the data, or input data from other application.
> 2. Make the dirty data clean (Be in the correct format to insert)
>
> **3️⃣**Testing: To test if the design meet the series requirements. 
>
> **4️⃣**Maintenance:



# PS. Some data types in mySQL

# 1. Character Type

> |       Type        |              Feature               |                     Notation                     |
> | :---------------: | :--------------------------------: | :----------------------------------------------: |
> | `CHAR(M)`$^{[1]}$ |            Fixed-length            | $M\in(0,255)$, Right-padded with spaces.$^{[2]}$ |
> |   `VARCHAR(M)`    |          Variable-length           |                 $M\in(1,65535)$                  |
> |  `BIT/BOOL/CHAR`  |         Same as `CHAR(1)`          |                       N/A                        |
> |      `BLOB`       |        Store binary number         |        Store up to 65535 Bytes of 010...         |
> |      `TEXT`       |          Store Character           |       Store up to 65535 Characters$^{[3]}$       |
> | `ENUM(a,b,c...)`  |      Store Fixed-length List       |        Up to 65535 Member in list$^{[4]}$        |
> |  `SET(a,b,c...)`  | Store variable-length List$^{[5]}$ |             Up to 64 Member in list              |
>
> [1] In `CHAR(M)` the M means the length of character
>
> [2] E.g. If we got an `temp` variable with type CHAT(5), If store the string `Hi` in `temp`, the actual data stored will be `Hi<tab><tab><tab>`
>
> [3] Not every character takes up one byte, in UTF-8 encoding a character may take up 1 to 4 bytes
>
> [4] The index of a member starts from 1, for example, the index of a1 and a2 in ENUM(a1,a2,a3) is 1 and 2 respectively.
>
> [5] For example, for SET(a1,a2,a3), you can store `<empty>/a1/a1a2/a1a2a3`

# 2. Integer Types

> | Type                    | Signed Range     | Unsigned Range |
> | ----------------------- | ---------------- | -------------- |
> | `TINYINT[(M)]`          | $-128\to127$     | $0\to255$      |
> | `SMALLINT[(M)]`         | $-32768\to32767$ | $0\to65535$    |
> | `INT[(M)]/INTEGER[(M)]` | ........         | ........       |
> | `BIGINT[(M)]`           | ........         | ........       |
>
> Example: `TINYINT(4)` means the width of the number when displayed is set to 4 bits. If the number stored is 5 and you request a display width, it may be displayed as `0005`

# 3. Real(实数) Type

> |            Type             |               Precision               |
> | :-------------------------: | :-----------------------------------: |
> |   `FLOAT[(M,D)]`$^{[1]}$    |                single                 |
> | `DOUBLE[(M,D)]/REAL[(M,D)]` |   double (Larger allowable values)    |
> | `DECIMAL[(M[,D])]`$^{[2]}$  | Fixed point type (Stored as a string) |
>
> [1] Parameter Meaning
>
> - `M`: Total length of the value (before and after the decimal point)
>
> - `D`: number of decimal places(小数位数)
>
> [2] Differences between `FLOAT/DOUBLE` and `DECIMAL`
>
> - `FLOAT/DOUBLE`: Storing approximations of values in binary format
> - `DECIMAL`: Storing numbers as strings preserves their exact value

# 4. Data & Time Type

> |    Type     |       Format        |                          Range                          |
> | :---------: | :-----------------: | :-----------------------------------------------------: |
> |   `DATA`    |     YYYY-MM-DD      |                1000-01-01$\to$9999-12-31                |
> |   `TIME`    |      HH:MM:SS       |                -838:59:59$\to$838:59:59                 |
> | `DATETIME`  | YYYY-MM-DD HH:MM:SS |       1000-01-01 00:00:00$\to$9999-12-31 23:59:59       |
> | `TIMESTAMP` | YYYY-MM-DD HH:MM:SS | 1970-01-01 00:00:00 UTC$\to$about 2037, Cnvert to local |
> |  `YEAR[4]`  |        YYYY         |                      1901$\to$2155                      |
>
> `NOW()`: A function return the presnet time with format YYYY-MM-DD HH:MM:SS