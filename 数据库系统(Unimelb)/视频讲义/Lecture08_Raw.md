<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240322190740263.png" alt="image-20240322190740263" style="zoom:50%;" /> 

Hello, everyone, and welcome back to the eighth lecture of our database systems series. Today, we will be diving into SQL, or Structured Query Language, which is the primary language used for interacting with and manipulating data in relational databases. This session aims to bridge the theoretical concepts of relational algebra, discussed in our previous lecture, with their practical application in SQL. By understanding how relational algebra principles translate into SQL commands, you will gain a deeper insight into the efficient management and manipulation of database information. So, let's embark on this journey to explore the intricacies of SQL and its pivotal role in the realm of relational databases.

大家好，欢迎回到我们数据库系统系列的第八讲。今天，我们将深入探讨SQL，或结构化查询语言，这是用于与关系数据库中的数据进行交互和操作的主要语言。本次课程旨在将上一讲讨论的关系代数的理论概念与SQL的实际应用桥接起来。通过理解关系代数原理如何转化为SQL命令，您将更深入地了解数据库信息的高效管理和操作。那么，让我们开始这一旅程，探索SQL的复杂性及其在关系数据库领域中的关键作用。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240322190815278.png" alt="image-20240322190815278" style="zoom:50%;" /> 

When exploring SQL, we encounter a fundamental concept known as CRUD, which is a recurrent acronym you'll often come across while researching SQL online. CRUD is nothing mysterious; it stands for Create, Read, Update, and Delete, encapsulating the core commands that SQL facilitates. These commands are essential as they are also supported by the majority of relational database systems. Throughout our lectures, we'll be examining SQL as a universal language. However, for those who may find themselves seeking additional guidance, I recommend consulting concise and clear tutorials. These resources can be incredibly beneficial, especially for those who are engaging with SQL for the first time.

当我们探索SQL时，会遇到一个基本概念，即CRUD，这是一个在网上研究SQL时经常遇到的重要缩写词。CRUD并没有什么神秘的；它代表了Create（创建），Read（读取），Update（更新）和Delete（删除），这些是SQL支持的核心命令。这些命令非常重要，因为它们也由大多数关系数据库系统支持。在我们的课程中，我们将把SQL作为一种通用语言来研究。但是，对于那些可能在寻求额外指导的人，我推荐查阅简明清晰的教程。这些资源对于第一次接触SQL的人来说可能特别有帮助。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240322190913106.png" alt="image-20240322190913106" style="zoom:50%;" /> 

The SQL language is versatile, comprising various command types, or sub-languages, namely DDL, DML, and DCL. DDL, the Data Definition Language, is tasked with structuring the database schema. It enables us to construct tables, append new columns, or remove existing ones, essentially shaping the database schema. Then, there's DML, or Data Manipulation Language, which is designed for handling the actual data within the database. This includes inserting new data, updating existing data, deleting records, and using the SELECT statement to query the database.

SQL语言非常多才多艺，包含了多种命令类型或子语言，即DDL、DML和DCL。DDL，数据定义语言，负责构造数据库架构。它使我们能够构建表格，添加新列或删除现有的列，基本上是在塑造数据库架构。然后是DML，或数据操作语言，旨在处理数据库内的实际数据。这包括插入新数据，更新现有数据，删除记录，以及使用SELECT语句来查询数据库。

We also explore DCL, the Data Control Language, which governs the permissions granted to various users. It determines who has access to which data and defines user roles. Additionally, we delve into database administration, including creating backups and performing recovery, as well as managing transactions. These topics equip you with a comprehensive understanding of SQL, enabling you to become a proficient and informed database user.

我们还会探讨DCL，数据控制语言，它管理授予各种用户的权限。它决定了谁可以访问哪些数据并定义用户角色。此外，我们还将深入研究数据库管理，包括创建备份和执行恢复，以及管理交易。这些主题为您提供了对SQL的全面了解，使您能够成为一个熟练且见多识广的数据库用户。

The importance of database administration, particularly backup and recovery, cannot be overstated. Students often question the necessity of learning these aspects when they are not currently dealing with critical data. However, it's imperative to understand that soon many will enter the workforce where data is invaluable, and the loss of significant data can result in severe consequences, including job loss.

数据库管理的重要性，特别是备份和恢复，不可低估。学生们常常质疑在他们目前没有处理关键数据的情况下学习这些方面的必要性。然而，必须明白，很快许多人将进入数据非常宝贵的劳动力市场，且丢失重要数据可能会导致严重后果，包括失业。

I emphasize practicing these critical skills in a safe, stress-free educational setting. This preparation is crucial, especially considering the high stakes involved in data handling within industries like banking. In such environments, a mistake during a backup or recovery process could not only be stressful but potentially 🚗eer-ending. Let's master these skills together to ensure you're well-prepared for the future.

我强调在一个安全、无压力的教育环境中练习这些关键技能。特别是考虑到在银行等行业内处理数据所涉及的高风险，这种准备是至关重要的。在这样的环境中，备份或恢复过程中的一个错误不仅会让人压力山大，还可能导致职业生涯的终结。让我们一起掌握这些技能，确保你为将来做好了充分准备。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240322191020920.png" alt="image-20240322191020920" style="zoom:50%;" /> 

Utilizing SQL begins with the implementation phase of the database. Recall that we've already covered how to construct our model. Following this, with the simple click of an export button, we generate a set of 'CREATE TABLE' statements. This is where the SQL language demonstrates its power, enabling the creation of a database. The next phase is the actual usage of the database, which involves the critical tasks of adding data, processing it, updating, deleting, and querying the database to retrieve valuable information. Remember, the essence of databases, as we discussed at the start of this course, is to unearth the 'hidden gold'—the insights that our database holds.

使用SQL首先开始于数据库的实现阶段。回想一下，我们已经讲过如何构建我们的模型。在此之后，通过简单点击导出按钮，我们生成了一系列的'CREATE TABLE'语句。这就是SQL语言展示其能力的地方，它使创建数据库成为可能。下一个阶段是数据库的实际使用，这涉及添加数据、处理数据、更新、删除以及查询数据库以检索有价值信息的关键任务。记住，正如我们在课程开始时讨论的，数据库的本质是发掘'隐藏的金子'——我们的数据库持有的洞察力。

The goal is to navigate the database to discover the knowledge hidden within its data—knowledge that our database can uniquely provide us. This process involves employing various SQL commands such as 'SELECT' to read and link the data from tables, and 'ALTER' and 'DROP' commands to modify the database structure. Likewise, 'INSERT', 'UPDATE', and 'DELETE' commands are used to change the data within the database. Each of these steps brings us closer to extracting the full value from the data at our disposal.

目标是导航数据库，以发现隐藏在其数据中的知识——这些知识是我们的数据库可以独特地为我们提供的。这个过程涉及使用各种SQL命令，如使用'SELECT'读取和链接表中的数据，以及使用'ALTER'和'DROP'命令修改数据库结构。同样，'INSERT'、'UPDATE'和'DELETE'命令用于更改数据库中的数据。这些步骤中的每一个都使我们更接近于从我们手头的数据中提取全部价值。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240322191113911.png" alt="image-20240322191113911" style="zoom:50%;" /> 

To break it down into simple steps, the diagram at the bottom of the screen exemplifies a physical model created using a modeling tool like Workbench, which is currently part of your practice. Once the model is constructed, it encapsulates all the entities, attributes, and data types. It designates mandatory and optional attributes, identifies the primary keys, and defines relationship labels. This stage is about reminding you of the necessary components when building a model. Upon completing this phase, you will export the model to generate 'CREATE TABLE' statements like the one illustrated here for the bank's headquarters. This marks the implementation stage of the database.

简单地分步骤来说，屏幕底部的图表示例了使用建模工具如Workbench创建的一个物理模型，这目前是你们的练习内容。一旦模型构建完成，它就包含了所有实体、属性和数据类型。它指定了必需和可选的属性，识别了主键，并定义了关系标签。这个阶段是关于提醒你们在建立模型时需要考虑的必要组成部分。完成这个阶段后，你将导出模型以生成类似这里所示的银行总部的'CREATE TABLE'语句。这标志着数据库的实现阶段。

In the next phase, the use of the database, we engage in populating our database with data. During this lecture, we will explore how to populate a table with data using examples like the ones shown. And lastly, in the third step, we can interrogate the database by running queries to search for intriguing patterns. In database systems, this querying process is often the most crucial element, as it allows us to extract meaningful insights from our data.

在下一个阶段，数据库的使用，我们开始向数据库中填充数据。在这节课中，我们将探讨如何使用显示的例子来向表中填充数据。最后，在第三步，我们可以通过运行查询来审查数据库，以寻找有趣的模式。在数据库系统中，查询过程通常是最关键的元素，因为它允许我们从数据中提取有意义的洞察。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323205720588.png" alt="image-20240323205720588" style="zoom:50%;" /> 

Let's begin by reviewing the steps involved in implementing or creating tables, which essentially revisits concepts we have covered after finalizing the modeling stage. By now, you should be able to understand how the entity depicted at the top of the screen translates into the SQL 'CREATE' statement at the bottom. All elements here should already be familiar to you, but there are two new aspects I want to clarify. Firstly, you may not have encountered 'AUTO_INCREMENT' before. This is a SQL function that automatically increments the value of a field. In our case, 'CustomerID' serves as a primary key and a surrogate key, ensuring uniqueness for each record in the table.

我们开始回顾实现或创建表的步骤，这实质上是在完成建模阶段后复习我们已经讨论过的概念。到目前为止，你们应该能够理解屏幕顶部显示的实体是如何转化为屏幕底部的SQL 'CREATE'语句的。这里的所有元素你们应该已经熟悉了，但有两个新的方面我想要澄清。首先，你们可能之前没有遇到过'AUTO_INCREMENT'。这是一个SQL功能，可以自动递增字段的值。在我们的例子中，'CustomerID'作为主键和代理键，确保了表中每条记录的唯一性。

Recall that every entity and relationship in our model must have a primary key, determined by business use cases. Sometimes keys are inherently meaningful, such as a student or staff ID. However, there may be instances where no attribute stands out as an obvious key. For example, in a table with attributes like first name, last name, and age, none are suitable as unique keys, not even in combination. This leads us to introduce a new attribute, a surrogate key, like 'CustomerID', which has no inherent meaning but ensures each record is uniquely identifiable. Since this key holds no meaningful value for us, we let the database manage it, which can be efficiently done using the 'AUTO_INCREMENT' function.

回想一下，我们模型中的每个实体和关系都必须有一个主键，这是通过业务用例来确定的。有时候，键本身就具有意义，比如学生或员工ID。但是，也可能会遇到没有属性显而易见地适合作为键的情况。例如，在一个具有名字、姓氏和年龄属性的表中，没有任何一个适合作为唯一键，即使它们组合在一起也不行。这就导致我们引入一个新的属性，一个代理键，像'CustomerID'，它没有内在的意义，但确保每条记录都是唯一可识别的。由于这个键对我们没有有意义的值，我们让数据库来管理它，这可以通过使用'AUTO_INCREMENT'功能有效完成。

The second new element is the 'ENUM' type, as seen with 'CustType', allowing only certain predefined values. In the 'CREATE' statement, we define an enumeration attribute that permits storage of specific values exclusively. In this case, for 'CustomerType', only 'Personal' or 'Company' are acceptable inputs. This ensures data integrity by restricting the entries to those predefined categories.

第二个新元素是'ENUM'类型，如'CustType'所示，只允许某些预定义的值。在'CREATE'语句中，我们定义了一个枚举属性，它只允许存储特定的值。在这种情况下，对于'CustomerType'，只有'Personal'或'Company'是可接受的输入。这通过将输入限制在那些预定义的类别中来确保数据的完整性。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323221349292.png" alt="image-20240323221349292" style="zoom:50%;" /> 

In our review, we revisit the concept of foreign keys, a fundamental aspect of relational databases that ensures tables are interlinked appropriately. A foreign key acts as a reference or pointer to a primary key in another table, establishing a relationship between tables. For example, in our accounts table, the foreign key points to the customer ID, indicating which customer each account belongs to. This mechanism is what enforces relational integrity within the database, making sure that relationships between tables are maintained correctly and the database doesn't permit inconsistencies.

在我们的复习中，我们回顾了外键的概念，这是关系数据库的一个基本方面，确保表之间被适当地关联。外键充当对另一个表中主键的引用或指针，建立表之间的关系。例如，在我们的账户表中，外键指向客户ID，表明每个账户属于哪个客户。这个机制是在数据库中强制实施关系完整性的，确保表之间的关系正确维护，数据库不允许存在不一致性。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323221458169.png" alt="image-20240323221458169" style="zoom:50%;" /> 

The SQL 'CREATE' statement includes a specification for foreign keys, which is critical in linking tables within a database. In the 'Account' table creation example shown here, the 'FOREIGN KEY' clause refers to 'Customer(CustomerID)', establishing a relationship with the 'Customer' table by indicating that the 'CustomerID' in the 'Account' table points to the 'CustomerID' in the 'Customer' table. Although this may not be new information, it's important for completeness and understanding as you revisit SQL. Being familiar with these elements is crucial as they form the foundation of relational database design.

SQL的'CREATE'语句中包含了外键的指定，这在数据库中链接表格时至关重要。如这里所示的'Account'表创建示例中，'FOREIGN KEY'子句引用了'Customer(CustomerID)'，通过指出'Account'表中的'CustomerID'指向'Customer'表中的'CustomerID'，建立了与'Customer'表的关系。虽然这可能不是新信息，但为了完整性和理解，当你重新审视SQL时，这是很重要的。熟悉这些元素至关重要，因为它们构成了关系数据库设计的基础。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323223321583.png" alt="image-20240323223321583" style="zoom:50%;" /> 

After our tables are set up, we begin the process of inserting data, as demonstrated here with two different INSERT statements. The first command, labeled (1), shows data being inserted specifically into the 'Customer' table. It's important to note that keywords like 'INSERT INTO' are highlighted in capital letters for clarity, though SQL is not case-sensitive. 'Customer' is identified as the table name, and within parentheses, we specify the exact attributes we want to populate: 'CustFirstName', 'CustLastName', and 'CustType'. Following the 'VALUES' keyword, we list the corresponding values for these attributes, with 'Peter' as the first name, 'Smith' as the last name, and 'Personal' as the customer type.

在我们的表格设置完成后，我们开始插入数据的过程，如这里用两种不同的INSERT语句所示。第一个命令，标记为（1），显示数据被具体插入到'Customer'表中。重要的是要注意，像'INSERT INTO'这样的关键字为了清晰起见都用大写字母突出显示，尽管SQL不区分大小写。'Customer'被识别为表名，在括号内，我们指定了我们想要填充的确切属性：'CustFirstName'，'CustLastName'和'CustType'。在'VALUES'关键字后，我们列出了这些属性对应的值，'Peter'作为名字，'Smith'作为姓，以及'Personal'作为客户类型。

In the second example, marked as (2), we omit the specification of attributes, implying that we're providing values for all columns in the order they are defined in the table schema. Here, 'DEFAULT' is used to denote that the 'CustID' will be auto-incremented by the database. The rest of the values provided include 'James' for the first name, 'NULL' for an undefined middle name, 'Jones' for the last name, 'JJ Enterprises' for the business name, and 'Company' for the customer type.

在第二个例子中，标记为（2），我们省略了属性的指定，这意味着我们按照它们在表模式中定义的顺序为所有列提供了值。这里，'DEFAULT'用来表示'CustID'将由数据库自动递增。提供的其余值包括'James'作为名字，'NULL'表示未定义的中间名，'Jones'作为姓，'JJ Enterprises'作为商业名称，以及'Company'作为客户类型。

The concept of 'NULL' in a database context is critical; it represents an unknown or undefined value, distinct from an empty string, which is denoted by two quotation marks with no content in between. Unlike an empty string, 'NULL' does not equate to zero for numerical fields. It is essential to understand this difference to correctly manage data entry and database integrity.

在数据库上下文中，'NULL'的概念至关重要；它代表一个未知或未定义的值，与空字符串不同，空字符串由两个没有内容的引号表示。与空字符串不同，'NULL'在数值字段中不等同于零。理解这种区别对于正确管理数据输入和数据库完整性至关重要。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323224702810.png" alt="image-20240323224702810" style="zoom:50%;" /> 



Understanding the meaning of 'NULL' is critical in database operations, especially for developers working on applications that interact with databases. The concept of 'NULL' can cause considerable confusion if not properly understood. 'NULL' represents an unknown or undefined value in the database; it is not equivalent to zero or an empty string. This is demonstrated by a case where a voting system in the US mishandled 'NULL' values. For several houses in Madison, Wisconsin, the coordinates were unknown, hence they were represented as 'NULL' in the database. However, the application developers misinterpreted 'NULL' as zero, which led to many people being erroneously assigned a coordinate of (0,0), placing them in the middle of the ocean. This incident underscores the importance of correctly understanding database concepts to avoid such errors in real-life applications.

了解'NULL'在数据库操作中的意义至关重要，特别是对于那些与数据库交互的应用程序的开发者来说。如果理解不当，'NULL'的概念可能会引起相当大的混淆。'NULL'在数据库中代表一个未知或未定义的值；它不等同于零或空字符串。这一点通过一个美国投票系统错误处理'NULL'值的案例得到了证明。在威斯康星州麦迪逊的几所房子，其坐标是未知的，因此在数据库中被表示为'NULL'。然而，应用程序开发者错误地将'NULL'解释为零，导致许多人错误地被分配了(0,0)的坐标，将他们放在了海洋中间。这个事件强调了正确理解数据库概念以避免在实际应用中出现此类错误的重要性。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324011422502.png" alt="image-20240324011422502" style="zoom:50%;" />  

After we've entered our data into the database, we can start analyzing it using SQL statements, which let us query the database. The example shown below is the simplest form of an SQL query, where we seek to retrieve all columns from a table. The asterisk symbol (*) here is used to indicate that we want all columns from the 'Customer' table. The result displayed represents three records that have been previously inserted into the database.

在我们将数据输入数据库之后，我们可以开始使用 SQL 语句来分析它，SQL 语句允许我们查询数据库。下面显示的例子是最简单的 SQL 查询形式，在这里我们试图检索一个表的所有列。这里的星号 (*) 表示我们要从 'Customer' 表中获取所有列。显示的结果代表了此前已经插入到数据库中的三条记录。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323225028621.png" alt="image-20240323225028621" style="zoom:50%;" /> 

Here we delve into the detailed structure of the SELECT statement in SQL. We start with the SELECT clause, where we list the attributes or expressions we intend to retrieve from the database. Next, the FROM clause specifies the tables or views from which we will be pulling data. The WHERE clause follows, allowing us to filter the data based on certain conditions. For grouping the results, we use the GROUP BY clause, followed by the HAVING clause, which permits us to apply conditions to the groups formed by GROUP BY. Ordering the output is done using the ORDER BY clause, and finally, the LIMIT clause is used to restrict the number of rows returned.

这里我们详细介绍了SQL中的SELECT语句的结构。我们从SELECT子句开始，列出我们打算从数据库中检索的属性或表达式。接下来，FROM子句指定了我们将从中提取数据的表或视图。接着是WHERE子句，允许我们根据某些条件过滤数据。为了对结果进行分组，我们使用GROUP BY子句，然后是HAVING子句，它允许我们对GROUP BY形成的组应用条件。使用ORDER BY子句对输出进行排序，最后，使用LIMIT子句限制返回的行数。

It's crucial to understand that the order of these clauses is significant. You cannot change their sequence; for instance, you cannot write FROM after WHERE, or WHERE after GROUP BY. While it is not necessary to include every clause in every SELECT statement, for those that you do include, maintaining the proper sequence is a must. Not every SELECT statement requires a WHERE, GROUP BY, or HAVING clause. However, for the clauses present in your query, the order must be respected.

理解这些子句的顺序是重要的。你不能改变它们的序列；例如，你不能在WHERE之后写FROM，或在GROUP BY之后写WHERE。虽然并不需要在每个SELECT语句中都包含每个子句，但对于你确实包含的那些子句，必须保持正确的顺序。并非每个SELECT语句都需要WHERE、GROUP BY或HAVING子句。然而，对于查询中出现的子句，必须尊重顺序。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323225405201.png" alt="image-20240323225405201" style="zoom:50%;" /> 

The most basic form of a query is exemplified by 'SELECT * FROM customer'. In this context, the asterisk (*) is a wild🚗d character that instructs the database to return all columns of data from the 'customer' table. To allow for a more interactive experience with the data, I've added additional records to the table, which although might not be immensely practical, serves our purpose for exploration and learning.

最基础的查询形式可以通过'SELECT * FROM customer'来示例。在这个上下文中，星号（*）是一个通配符，它指示数据库从'customer'表返回所有列的数据。为了能够更互动地体验数据，我已经向表中添加了额外的记录，虽然这可能并不极其实用，但为了探索和学习，它服务于我们的目的。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323230506605.png" alt="image-20240323230506605" style="zoom:50%;" /> 

Tables often contain many attributes, but typically we're interested in a subset of these attributes. This concept is known as projection in relational algebra, where we focus only on the attributes that are of interest to us. To accomplish this in SQL, we specify the attributes we wish to retain, listed and separated by commas alongside the SELECT statement. What was represented by the π (pi) symbol followed by attribute names in relational algebra is now translated into SQL as SELECT followed by the list of attribute names. As a result, we obtain a new table, in this case containing just a single column with last names.

由于表中通常包含许多属性，但通常我们只对其中的一部分属性感兴趣。这个概念在关系代数中被称为投影（projection），我们只关注我们感兴趣的属性。要在SQL中实现这一点，我们指定我们希望保留的属性，将它们列出并用逗号分隔，紧随SELECT语句之后。在关系代数中，π（pi）符号后跟属性名称，在SQL中转换为SELECT后跟属性名称列表。结果，我们得到了一个新表，在这种情况下，该表仅包含一个带有姓氏的单列。

It's important to note that while the theory of relational algebra states that projection eliminates duplicates to provide correct answers, commercial SQL systems deviate slightly from this theoretical aspect. Removing duplicates can be computationally expensive, so most systems do not remove them by default. If we want to eliminate duplicates in our results, we must explicitly use the DISTINCT keyword before the attribute names in our SQL query. Moving forward from projection, the next concept we cover is selection, which is an operation on a single table, as we recall from relational algebra.

需要注意的是，虽然关系代数理论指出投影操作会消除重复项以提供正确答案，但商业SQL系统在这一理论方面略有偏差。删除重复项在计算上可能代价高昂，因此大多数系统默认不会移除它们。如果我们想在结果中消除重复项，我们必须在SQL查询中的属性名称前明确使用DISTINCT关键字。从投影概念继续前进，我们接下来要覆盖的概念是选择（selection），这是一个单表操作，正如我们从关系代数中回忆的那样。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323230655117.png" alt="image-20240323230655117" style="zoom:50%;" /> 

Selection in databases is used to filter data, allowing us to keep only the records that meet certain conditions. In relational algebra, this is denoted by the Greek letter Sigma (σ) followed by the conditions for filtering. In SQL, this translates to using the WHERE clause accompanied by conditions that determine which records to retain. These conditions can be combined using the logical operators AND, OR, and NOT. The slide illustrates a combined use of selection and projection on a customer database. For projection, we’re extracting only the last names, and for selection, the WHERE clause filters the records to include only customers whose last name is 'Smith'. Consequently, the result displayed shows only the entries with the last name 'Smith'.

在数据库中，选择用于过滤数据，让我们只保留满足特定条件的记录。在关系代数中，这由希腊字母Sigma (σ)及其后的过滤条件表示。在SQL中，这转化为使用WHERE子句和伴随的条件来确定保留哪些记录。这些条件可以使用逻辑操作符AND、OR和NOT组合。幻灯片展示了在客户数据库上结合使用选择和投影的例子。对于投影，我们只提取姓氏，对于选择，WHERE子句过滤出只包括姓'Smith'的客户的记录。因此，显示的结果仅显示姓氏为'Smith'的条目。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323230740461.png" alt="image-20240323230740461" style="zoom:50%;" /> 

When it comes to selection in SQL, we've learned that conditions can take the form of arithmetic expressions, which are well-suited for numerical values. However, when dealing with string values, comparison operations like greater than or less than become irrelevant. In such cases, we use the LIKE clause for pattern matching within strings. The LIKE clause is accompanied by a regular expression, which employs two special wild🚗d characters: the percent sign (%) to represent zero, one, or multiple characters, and the underscore (_) to represent exactly one character.

在SQL中进行选择时，我们已经了解到条件可以采用算术表达式的形式，这适用于数值。但是，当处理字符串值时，像大于或小于这样的比较操作就变得不相关了。在这种情况下，我们使用LIKE子句进行字符串内的模式匹配。LIKE子句伴随着一个正则表达式，它使用两个特殊的通配符字符：百分号（%）代表零个、一个或多个字符，下划线（_）代表确切的一个字符。

The LIKE clause allows us to explore various string expressions. Here are a few examples: the expression 'a%' finds any values that begin with 'a', regardless of what follows. The second example '%a' looks for strings that end with 'a', and the third '%or%' searches for any occurrence of 'or' in any position within the string. The fourth example '*r%' finds values where 'r' is in the second position, and 'a*%_%' finds strings that start with 'a' and are at least three characters long. The final example 'a%o' searches for strings that start with 'a' and end with 'o'.

LIKE子句允许我们探索各种字符串表达式。这里有几个例子：表达式'a%'查找任何以'a'开头的值，不管其后是什么。第二个例子'%a'寻找以'a'结尾的字符串，第三个'%or%'搜索字符串中任何位置的'or'出现。第四个例子'*r%'找到'r'在第二位的值，而'a*%_%'找到以'a'开头且至少三个字符长的字符串。最后一个例子'a%o'搜索以'a'开头且以'o'结尾的字符串。

To test your understanding, try writing a regular expression that matches a string of at least four characters in length with the last character being 'o'. If this exercise is challenging, revisiting these examples should help solidify your grasp of how the LIKE clause operates.

为了测试你的理解，尝试编写一个正则表达式，匹配至少四个字符长，最后一个字符是'o'的字符串。如果这个练习有难度，回顾这些例子应该有助于巩固你对LIKE子句操作方式的理解。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323231011949.png" alt="image-20240323231011949" style="zoom:50%;" /> 

Column renaming is an additional operation we utilize when working with single tables. Comparable to role renaming in relational algebra, SQL achieves this with the AS clause where we assign a new name to an attribute or an expression result. The example shown here demonstrates renaming the result of a count function on customer IDs by customer type. Without renaming, the result column would have the default name of the function and parameter, which can be unclear or unwieldy. By using 'AS Count', we provide a clearer, more meaningful name to the result column, which in this case is simply labeled as 'Count'. It's important to clarify that while the results appear as tables, they are not actual tables stored in the database; they are virtual tables—temporary results displayed from executing the query.

列重命名是我们在处理单个表时使用的附加操作。与关系代数中的角色重命名类似，SQL通过AS子句实现，我们在那里为属性或表达式结果分配一个新名称。这里展示的例子演示了根据客户类型对客户ID进行计数函数的结果重命名。如果不进行重命名，结果列将具有函数和参数的默认名称，这可能不清晰或不易管理。通过使用'AS Count'，我们为结果列提供了一个更清晰、更有意义的名称，在这个例子中，简单地标记为'Count'。重要的是要澄清，虽然结果看起来像是表格，它们不是数据库中存储的实际表格；它们是虚拟表——执行查询后显示的临时结果。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323231056967.png" alt="image-20240323231056967" style="zoom:50%;" /> 

Aggregate functions are essential when we need to perform operations across multiple records within a single table, allowing us to condense large datasets into meaningful single outputs. These functions are widely used because databases often contain vast numbers of records, and simply outputting raw data would be overwhelming. Through aggregate functions such as AVG (average), MIN (minimum), MAX (maximum), COUNT (number of values), and SUM (sum of values), we can process and distill our data in a more digestible form. These are some of the most commonly utilized aggregate functions, and for those who are eager to delve deeper, a helpful link is provided in the lecture notes.

聚合函数在我们需要在单个表格中对多个记录执行操作时至关重要，它们允许我们将庞大的数据集压缩成有意义的单个输出。这些函数被广泛使用，因为数据库通常包含大量的记录，仅仅输出原始数据将是压倒性的。通过聚合函数如AVG（平均值）、MIN（最小值）、MAX（最大值）、COUNT（值的数量）和SUM（值的总和），我们可以以更易于消化的形式处理和提炼我们的数据。这些是一些最常用的聚合函数，对于那些渴望深入研究的人来说，讲义中提供了一个有用的链接。

When using aggregate functions, it's important to understand that they typically ignore NULL values. For example, when calculating an average across a set of records, any NULL value within the set will not be considered in the computation, effectively treating the average as if the NULL value were non-existent. The exception to this is the COUNT function, which counts the number of records without regard to whether their values are NULL or not, focusing solely on the presence of the records themselves.

在使用聚合函数时，了解它们通常忽略NULL值是重要的。例如，在计算一组记录的平均值时，集合中的任何NULL值都不会被计算在内，有效地将平均值视为如果NULL值不存在一样。此规则的例外是COUNT函数，它计算记录的数量，而不考虑它们的值是否为NULL，仅关注记录本身的存在。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323231242563.png" alt="image-20240323231242563" style="zoom:50%;" /> 

Let’s explore how aggregate functions are practically applied. Two of the most commonly used functions are COUNT and AVG. COUNT returns the total number of records, while AVG calculates the average of the specified values. For instance, the first SQL statement we see simply determines the number of customers by counting customer IDs in the Customer table. This basic measure, often the starting point of data analysis, gives us an insight into the table’s 🚗dinality. The second example illustrates how to calculate the average balance across all accounts, a more efficient method than examining each account balance individually, especially when dealing with numerous accounts.

我们来探讨一下聚合函数在实践中是如何应用的。最常用的两个函数是COUNT和AVG。COUNT返回记录的总数，而AVG计算指定值的平均值。例如，我们看到的第一个SQL语句仅通过计算Customer表中的CustomerID来确定客户的数量。这个基本的度量，通常是数据分析的起点，让我们了解到表的基数。第二个例子展示了如何计算所有账户的平均余额，这比单独查看每个账户的余额更有效率，尤其是当处理大量账户时。

However, simply averaging account balances may not provide a meaningful picture in scenarios like a bank’s customer analysis, where a few high-net-worth individuals can skew the average significantly. A more useful approach could be to assess the average balance per customer. One method is to apply the AVG function with a specific condition, such as calculating the average balance for customer ID 1. Yet, this becomes impractical when we have multiple customers. A better alternative is to use the GROUP BY clause, which groups records by a certain criterion and computes an average for each group. The last SQL statement shown exemplifies how to calculate the average balance per customer using GROUP BY. This approach is particularly valuable when seeking a more granular understanding of data. Typically, GROUP BY is paired with aggregate functions to avoid generalizing over the entire data set.

然而，仅仅计算账户余额的平均值在某些场景下可能并不提供有意义的信息，比如在银行的客户分析中，少数高净值个人可能会显著影响平均值。一个更有用的方法可能是评估每个客户的平均余额。一种方法是对AVG函数应用特定条件，如计算客户ID 1的账户平均余额。然而，当我们有多个客户时，这种方法就变得不切实际了。一个更好的选择是使用GROUP BY子句，它根据某个标准对记录进行分组，并为每个组计算平均值。展示的最后一个SQL语句例子说明了如何使用GROUP BY计算每个客户的平均余额。这种方法在寻求更细粒度的数据理解时特别有价值。通常，GROUP BY与聚合函数配对使用，以避免对整个数据集进行概括。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323231930384.png" alt="image-20240323231930384" style="zoom:50%;" /> 

The GROUP BY clause is a powerful SQL feature that organizes records into groups based on one or more attributes, often used in conjunction with aggregate functions to compute a single result from each group. Here's an example demonstrating how to calculate the average balance per customer. The SQL query is: `SELECT AVG(OutstandingBalance) FROM Account GROUP BY CustomerID;`. This command consolidates the accounts by each customer and calculates an average balance for each group, effectively returning one record per customer, each representing the average balance of their accounts.

GROUP BY子句是SQL的一个强大功能，它根据一个或多个属性将记录组织成组，并且经常与聚合函数一起使用，以从每个组中计算出一个结果。这里有一个示例，演示了如何计算每个客户的平均余额。SQL查询为：`SELECT AVG(OutstandingBalance) FROM Account GROUP BY CustomerID;`。这个命令按每个客户汇总账户，并计算每组的平均余额，有效地为每个客户返回一个记录，每个记录代表他们账户的平均余额。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323232147528.png" alt="image-20240323232147528" style="zoom: 25%;" /> 



Let's take a practical look at a table named 'accounts', which includes columns for 'custID' and 'balance'. For instance, imagine there are three accounts for customer ID 1 with balances of 100, 101, and 102. Additionally, let's say customer ID 2 has two accounts with balances of 50 and 60. We're assuming other necessary columns like 'accountID' are present to avoid primary key violations. Utilizing the GROUP BY clause, the records for the same customer are grouped, and an aggregate function, such as AVG, is applied to calculate the average balance. For customer ID 1, the average balance calculated by SQL would be 101. For customer ID 2, it would be 55. Hence, the final result would show two records indicating the average balance per customer: 101 for customer ID 1 and 55 for customer ID 2, rather than displaying individual account balances.

让我们具体看看一个名为'accounts'的表，它包括'custID'和'balance'两列。例如，假设客户ID 1有三个账户，余额分别为100、101和102。此外，假设客户ID 2有两个账户，余额分别为50和60。我们假设其他必要的列如'accountID'存在，以避免主键冲突。使用GROUP BY子句，相同客户的记录被分组，并应用聚合函数如AVG来计算平均余额。对于客户ID 1，SQL计算的平均余额将是101。对于客户ID 2，将是55。因此，最终结果将显示每个客户的平均余额的两个记录：客户ID 1的平均余额为101，客户ID 2的为55，而不是显示每个账户的余额。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323232249169.png" alt="image-20240323232249169" style="zoom:50%;" /> 

The HAVING clause in SQL is essential when working with GROUP BY, as it allows for conditions to be applied specifically to aggregated data, something the WHERE clause can’t do. For example, if we want to list countries with more than five customers, we use the GROUP BY clause to group customers by country and then apply the HAVING clause to filter these groups. The condition would be specified as HAVING COUNT(CustomerID) > 5, ensuring only those countries with a customer count greater than five are included in the results. Always remember, use HAVING to impose conditions on aggregate data, not WHERE, and then you may order the output as needed.

在SQL中，HAVING子句是与GROUP BY一起使用时至关重要的，因为它允许对聚合数据应用特定的条件，这是WHERE子句无法做到的。例如，如果我们想列出有超过五个客户的国家，我们使用GROUP BY子句按国家对客户进行分组，然后应用HAVING子句来过滤这些组。条件将被指定为HAVING COUNT(CustomerID) > 5，确保只包括客户数超过五的国家在结果中。记住，使用HAVING来对聚合数据施加条件，而不是WHERE，然后你可以根据需要对输出结果进行排序。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323232332358.png" alt="image-20240323232332358" style="zoom:50%;" /> 

Relational tables, it should be noted, are sets. As such, the sequence in which we see them does not necessarily reflect any inherent order within the data—it can change depending on retrieval methods and storage, among other variables. These are topics we will explore in more detail later in the course. To impose a specific order, however, we utilize the ORDER BY clause, which allows for sorting in ascending (ASC) or descending (DESC) order, with ascending being the default when no specific direction is given.

关系表是集合，这一点需要注意。因此，我们看到的顺序并不反映数据的固有顺序——它可以根据检索方法和存储等多种因素而改变。这些主题我们将在课程后面详细探讨。然而，为了强制特定顺序，我们使用 ORDER BY 子句，它允许按升序（ASC）或降序（DESC）排序，默认情况下，如果没有给出具体方向，将按升序排序。

Sorting is not confined to a single column; we can order data across multiple columns, sorting first by one criterion, then another. The example on the left shows customer records sorted by last name in ascending order by default, as no order direction is specified. On the right, the DESC directive results in a descending order sort. This demonstrates the power of the ORDER BY clause to tailor data retrieval to our precise requirements.

排序并不限于单个列；我们可以跨多个列对数据进行排序，首先按一个标准，然后按另一个标准。左边的示例显示了按姓氏默认升序排序的客户记录，因为没有指定排序方向。在右边，DESC 指令导致数据按降序排序。这展示了 ORDER BY 子句调整数据检索以满足我们精确要求的能力。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323233550099.png" alt="image-20240323233550099" style="zoom:50%;" /> 

The LIMIT clause in SQL is a tool we employ when we need to constrain the number of records displayed as output on the screen. It's particularly practical for those instances when only the top results are relevant, which is quite common. By applying the LIMIT clause with a specified number, such as 'LIMIT 5' in our example, the query is executed and only the first five records are presented. This is where the query stops and the output is cut off, ensuring a focused and concise dataset.

LIMIT 子句在 SQL 中用于限制屏幕上显示的记录数。这在只需要最顶端的结果时非常实用，这是非常常见的情况。通过使用指定数字的 LIMIT 子句，例如我们例子中的 'LIMIT 5'，查询执行后仅显示前五条记录。查询到此为止，输出被截断，确保了数据集的集中和简洁。

In conjunction with LIMIT, the OFFSET clause is used when we want to bypass a certain number of records from the beginning. It works in tandem with LIMIT to then display a set amount of records following the ones that were skipped. For example, on the right side of the slide, 'OFFSET 3' is used to skip the first three records, and then 'LIMIT 5' ensures that the next five records, starting from the fourth, are shown. This pairing of commands offers flexibility in managing the data output, allowing us to fine-tune the results to our specific viewing requirements.

与 LIMIT 配合使用时，OFFSET 子句用于跳过开始的一定数量的记录。它与 LIMIT 协同工作，然后显示跳过之后的一定量的记录。例如，幻灯片右侧的 'OFFSET 3' 被用来跳过前三条记录，然后 'LIMIT 5' 确保从第四条开始的接下来五条记录被显示出来。这两个命令的结合为管理数据输出提供了灵活性，允许我们根据特定的查看需求微调结果。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323233839589.png" alt="image-20240323233839589" style="zoom:50%;" /> 

Up to this point, our database operations have focused solely on single tables. As I have mentioned earlier, working with a single table doesn't fully harness the power of database systems. For any form of record merging, combination, or lookup, we employ joins, which you will get ample opportunity to practice. So now, let's examine how to construct various types of joins using SQL. A cross product, for instance, is easily obtained by listing the tables we wish to combine, separated by commas. Therefore, the command `SELECT * FROM Customer, Account` creates a cross product of the Customer and Account tables. To recap, a cross product combines every record from one table with every record from another, resulting in a table where all possible combinations are represented. While we see here an exhaustive merging in this new table, I pointed out that cross products are not commonly used in practice. They typically don't serve our purposes since they combine data indiscriminately, without any logical basis for the merge.

到目前为止，我们的数据库操作仅专注于单个表上。正如我之前提到的，仅使用单个表并没有充分发挥数据库系统的威力。对于任何类型的记录合并、组合或查找，我们都会使用连接操作，而这些操作你将有充足的机会来练习。现在，让我们来看看如何使用 SQL 构造不同类型的连接。举个例子，通过逗号分隔的表列表可以很容易地得到交叉积。因此，命令 `SELECT * FROM Customer, Account` 会创建 Customer 和 Account 表之间的交叉积。再强调一遍，交叉积将一个表中的每条记录与另一个表中的每条记录相结合，结果产生一个所有可能组合都包含的新表。尽管我们在这里看到一个彻底的合并在这个新表中，但我指出交叉积在实践中并不常用。因为它们无选择地合并数据，没有合并的逻辑依据，通常并不满足我们的需求。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234228394.png" alt="image-20240323234228394" style="zoom:50%;" /> 

In exploring joins in SQL, we start with the inner join, or equi join, which links tables based on a specified condition. In MySQL, this is articulated using the INNER JOIN keyword, followed by the condition stated after the ON keyword. Such a join is particularly useful when we want to merge records that have matching values in both tables. For example, by joining on CustomerID, each customer from one table is associated only with their corresponding account details from another table, not with any other accounts. 

在 SQL 中探索连接时，我们从内连接（或等值连接）开始，这种连接基于特定条件连接表。在 MySQL 中，这通过 INNER JOIN 关键字表达，其后是 ON 关键字后面的条件。当我们想要合并在两个表中都有匹配值的记录时，这种连接特别有用。例如，通过在 CustomerID 上进行连接，一个表中的每个客户只与另一个表中相对应的账户详情相关联，而不是与任何其他账户相关联。

Moving on to the natural join, it's a variant that's widely utilized, perhaps even more so than the equi join in practice. A natural join links two tables based on attributes with identical names across both tables, and as such, it doesn't require an explicit condition to be stated—the condition is inferred. Notably, a natural join also performs a projection, removing duplicated columns that an equi join would retain. As you can see, natural join automatically dis🚗ds one of the duplicate CustomerID columns. However, it's important to note that natural joins are contingent on the exact matching of attribute names, whereas an equi join allows for greater flexibility in specifying join conditions.

接下来是自然连接，它是一个被广泛使用的变体，在实践中可能比等值连接使用得更多。自然连接基于两个表中名称相同的属性连接两个表，因此它不需要明确说明条件——条件是隐含的。值得注意的是，自然连接还执行投影，移除等值连接会保留的重复列。正如你所见，自然连接自动丢弃了其中一个重复的 CustomerID 列。然而，重要的是要注意，自然连接依赖于属性名称的完全匹配，而等值连接则允许在指定连接条件时更具灵活性。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234551078.png" alt="image-20240323234551078" style="zoom:50%;" /> 

Aside from the natural and inner joins we've discussed, SQL also provides outer joins, which come in two main types: left and right. An outer join is particularly useful because it includes in the result set those records that do not have corresponding matches in the other table. This means that, unlike inner joins which only return matched records from both tables, outer joins ensure that all records from one side of the join are returned, with NULL values filling in for any missing matches from the other side.

除了我们已经讨论过的自然连接和内连接，SQL 还提供了外连接，主要有两种类型：左连接和右连接。外连接特别有用，因为它会在结果集中包括那些在另一张表中没有对应匹配的记录。这意味着，与只返回两个表中匹配记录的内连接不同，外连接确保了连接一侧的所有记录都会被返回，对于另一侧缺失的匹配，则用 NULL 值填充。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234228394.png" alt="image-20240323234228394" style="zoom:50%;" /> 

Revisiting our earlier example, we have employed an inner join to correlate the 'Customer' and 'Account' tables. The result of this operation, as demonstrated, links Customer 1 and Customer 2 with their respective accounts. This kind of join is fundamental when the objective is to synchronize two tables based on shared attributes—in this case, ensuring that each customer is matched with their correct account details.

回到我们之前的例子，我们使用了内连接来关联 'Customer' 和 'Account' 表。如你所见，这个操作的结果将客户 1 和客户 2 与他们各自的账户相连接。当目标是基于共享属性同步两个表时，这种连接是基本操作——在本例中，确保每个客户都与他们正确的账户详情相匹配。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234742413.png" alt="image-20240323234742413" style="zoom:50%;" /> 

In this illustration of a cross product between the 'Customer' and 'Account' tables, each customer is combined with every account, which may not be immediately evident in the data presented. Not shown here, but present in the dataset, is the customer with ID three, 'Akin', who is also included in this comprehensive pairing.

在这个展示 'Customer' 表和 'Account' 表之间的交叉积的例子中，每个客户都与每个账户进行了组合，这在所呈现的数据中可能不立即显而易见。虽然在这里没有显示，但在数据集中确实存在的是，ID 为三的客户 'Akin'，他也被包含在这个全面的配对中。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234842908.png" alt="image-20240323234842908" style="zoom:50%;" /> 

In our dataset, we have three customers in total. When employing an inner join between the 'Customer' and 'Account' tables, it's evident that one customer is not represented in the output. This is because there is no corresponding account for this customer; thus, when an inner join is applied, such records without matches are excluded from the results. Outer joins, in contrast, are designed to include these unmatched records by displaying them alongside the matched ones.

在我们的数据集中，总共有三位客户。当使用 'Customer' 和 'Account' 表之间的内连接时，明显可以看出有一个客户在输出中没有被表示出来。这是因为这位客户没有对应的账户；因此，当应用内连接时，没有匹配的记录会从结果中被排除。与之相反，外连接的设计就是为了通过在匹配的记录旁边显示这些未匹配的记录来包含这些记录。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323234928584.png" alt="image-20240323234928584" style="zoom:50%;" /> 

In our dataset, we find an example of an outer join, which serves to include not only matching records from two tables but also those without corresponding matches. Specifically, a left outer join will include every record from the left table ('Customer' in our case) and if there's no matching record in the right table ('Account'), the result will display NULL values for the right table's fields. In the example shown, we can see the left outer join in action where the third customer, Akin, who has no matching account, is displayed with NULL values for the account-related fields.

在我们的数据集中，我们找到了一个外连接的例子，它不仅包括两个表中匹配的记录，还包括那些没有相应匹配的记录。具体来说，左外连接将包括左表（在我们的案例中是 'Customer'）的每条记录，如果在右表（'Account'）中没有匹配的记录，结果将显示 NULL 值，代表右表的字段。在所展示的示例中，我们可以看到左外连接的应用，其中第三个客户，Akin，没有匹配的账户，其账户相关字段显示为 NULL 值。

The right outer join operates in a symmetrical fashion, where it includes all records from the right table ('Account'), and if there are no corresponding customers, the customer-related fields are filled with NULLs. Although not depicted here, if our data model allowed for accounts without customers, we would see those accounts represented, merged with NULL values where the customer information would be. This distinction illustrates the fundamental difference between outer and inner joins: outer joins can reveal the absence of corresponding data between two tables. Beneath the surface, outer joins utilize a combination of joins and set difference operations from relational algebra, making the direction of the join (left or right) crucial since set difference is not a symmetrical operation.

右外连接以对称的方式运行，它包括右表（'Account'）的所有记录，如果没有对应的客户，客户相关的字段将填充 NULL 值。虽然这里没有显示，但如果我们的数据模型允许没有客户的账户存在，我们会看到这些账户被表示出来，与客户信息应在的地方合并的 NULL 值。这种区别说明了外连接和内连接之间的根本差异：外连接可以揭示两个表之间对应数据的缺失。在底层，外连接使用了关系代数中的连接和集合差运算的组合，这使得连接的方向（左或右）至关重要，因为集合差不是对称操作。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240323235419196.png" alt="image-20240323235419196" style="zoom:50%;" />  

Joints, being a critical concept in database systems, can be illustrated using various methods, and one effective way is through Venn diagrams, a concept you may recall from mathematics. Remembering that tables are sets, we can visualize them as two overlapping circles in a Venn diagram. The first circle encompasses all potential 'ID' values from table T1, while the second encompasses all 'ID' values from table T2. The intersection of these two sets represents the 'ID' values that are common to both tables—these are the values that joints are concerned with when combining data from T1 and T2.

连接是数据库系统中的一个关键概念，可以用多种方法来说明，其中一种有效的方式是通过文氏图，这是你可能从数学课上记得的概念。记住表是集合，我们可以将它们想象成文氏图中的两个重叠的圆圈。第一个圆圈包含了表 T1 中所有可能的 'ID' 值，而第二个圆圈包含了表 T2 中所有的 'ID' 值。这两个集合的交集代表了两个表中共有的 'ID' 值——当我们结合来自 T1 和 T2 的数据时，连接正是关注这些值。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000154469.png" alt="image-20240324000154469" style="zoom:50%;" /> 

When we execute a join like `T1 INNER JOIN T2 ON T1.ID = T2.ID`, we're essentially looking for IDs that exist in both tables. This is visually represented in the Venn diagram where the intersection highlights the common IDs. This operation is analogous to a natural join, where the join condition is implicit, provided the attribute names are identical in both tables. Such a join results in a dataset comprising only the records with matching IDs across both tables.

当我们执行像 `T1 INNER JOIN T2 ON T1.ID = T2.ID` 这样的连接时，我们实际上是在查找存在于两个表中的 ID。这在文氏图中得到了视觉表示，交集突出显示了共有的 ID。这个操作类似于自然连接，在自然连接中，只要两个表中的属性名称相同，连接条件就是隐含的。这样的连接会产生一个数据集，其中只包含在两个表中都有匹配 ID 的记录。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000243074.png" alt="image-20240324000243074" style="zoom:50%;" /> 

The `T1 LEFT OUTER JOIN T2 ON T1.ID = T2.ID` command not only retrieves records with IDs present in both T1 and T2 tables, as indicated by the intersection in the Venn diagram, but it also includes those records from T1 that do not have corresponding records in T2. In such cases, the missing T2 values are represented by NULLs, or "dummy values," to maintain the integrity of the result set.

除了在 T1 和 T2 两个表中都存在的 ID 记录，`T1 LEFT OUTER JOIN T2 ON T1.ID = T2.ID` 命令还包括了那些在 T2 中没有对应记录的 T1 中的记录，这在文氏图的交集中有所示。在这种情况下，缺失的 T2 值由 NULL 或“虚拟值”表示，以保持结果集的完整性。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000318608.png" alt="image-20240324000318608" style="zoom:50%;" /> 

The `T1 RIGHT OUTER JOIN T2 ON T1.ID = T2.ID` command, represented symmetrically in a Venn diagram, retrieves not only the records with IDs found in both tables, which is the intersection, but also all records from the right-hand table (T2) that don't have a corresponding match on the left (T1). These unmatched T2 records are paired with NULL values. This highlights the key difference between a right outer join and other join types, such as a left outer join or an inner join, which exclusively focuses on matched records.

`T1 RIGHT OUTER JOIN T2 ON T1.ID = T2.ID` 命令在文氏图中对称地表示，不仅检索在两个表中都找到的 ID 记录，即交集部分，还包括所有右表（T2）中没有在左表（T1）中对应匹配的记录。这些未匹配的 T2 记录将与 NULL 值配对。这突出了右外连接与其他连接类型（如左外连接或内连接）的主要区别，后者仅专注于匹配的记录。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000440529.png" alt="image-20240324000440529" style="zoom:50%;" /> 

The `T1 FULL OUTER JOIN T2 ON T1.ID = T2.ID` operation seeks to unify the complete set of records from both T1 and T2. It ensures that all matching records are included in the result set, and additionally, it includes records from both sides that do not have a corresponding match on the other. These unmatched records are combined with NULL values, or "dummy values," to signify the absence of a match. This type of join provides a comprehensive overview by including all possible records from both tables in the query result.

`T1 FULL OUTER JOIN T2 ON T1.ID = T2.ID` 操作旨在统一来自 T1 和 T2 的全部记录集合。它确保所有匹配的记录都包含在结果集中，并且还包括来自两边的没有在另一边找到对应匹配的记录。这些未匹配的记录将与 NULL 值或“虚拟值”结合，以表示没有找到匹配。这种类型的连接通过在查询结果中包含来自两个表的所有可能记录，提供了一个全面的概览。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000654096.png" alt="image-20240324000654096" style="zoom: 33%;" /> 

Let's take a look at how SQL JOINs work with a practical example. Imagine we have a `customer` table with a `Customer ID` and other attributes. We have customers with IDs 1, 2, and 3, each having some data. Additionally, we have an `account` table with `Customer ID`, `Account ID`, `Balance`, and more. In this setup, `Customer ID` 1 has an account with `Account ID` 1 and some balance, `Customer ID` 2 has `Account ID` 2, and there's an `Account ID` 4 that is not linked to any customer. It's an unusual scenario, but let's assume our design allows for it.

现在我们用一个实际例子来解释SQL的JOIN操作。设想我们有一个`customer`表，有`Customer ID`和其他属性。我们有ID为1，2和3的客户，每个客户都有一些数据。此外，我们有一个`account`表，有`Customer ID`、`Account ID`、`Balance`等数据。在这个设置中，`Customer ID` 1有一个`Account ID` 1和一些余额，`Customer ID` 2有`Account ID` 2，还有一个`Account ID` 4未链接到任何客户。这是一个不寻常的情况，但我们假设我们的设计允许这样。

The `INNER JOIN` will merge rows based on matching `Customer IDs` in both tables. So, `Customer ID` 1 joins with `Account ID` 1, and `Customer ID` 2 joins with `Account ID` 2. The result will be a combined table with `Customer ID`, `Account ID`, and related data for accounts 1 and 2. Note that `Customer ID` 3 won't appear in this output, as there's no corresponding account.

`INNER JOIN`将基于两个表中匹配的`Customer IDs`合并行。因此，`Customer ID` 1与`Account ID` 1相结合，`Customer ID` 2与`Account ID` 2相结合。结果将是一个合并的表，有`Customer ID`、`Account ID`和1号和2号账户的相关数据。注意`Customer ID` 3不会出现在这个输出中，因为没有对应的账户。

Using a `LEFT OUTER JOIN`, we can include all records from the left (customer) table. This means `Customer ID` 3 will appear in the output, but it will have null values for `Account ID` and other account-related attributes because it has no matching account.

使用`LEFT OUTER JOIN`，我们可以包括左侧（customer表）的所有记录。这意味着`Customer ID` 3将出现在输出中，但它将对`Account ID`和其他与账户相关的属性有空值，因为它没有匹配的账户。

A `RIGHT OUTER JOIN` includes all records from the right (account) table. So, we'll have an entry for `Account ID` 4 with a null `Customer ID` and the corresponding account data. Every attribute from the customer side will be null because there's no matching customer.

`RIGHT OUTER JOIN`包括右侧（account表）的所有记录。因此，我们将有一个`Account ID` 4的条目，有一个空的`Customer ID`和相应的账户数据。因为没有匹配的客户，客户端的每个属性都将为空。

Finally, a `FULL OUTER JOIN` combines the results of both left and right outer joins. It includes all records from both tables, with nulls in place for any unmatched columns from either side.

最后，`FULL OUTER JOIN`结合了左外连接和右外连接的结果。它包括两个表中的所有记录，对于任一侧未匹配的列，将使用空值代替。

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324000904526.png" alt="image-20240324000904526" style="zoom:67%;" /> 

In today’s lecture, we delved into the practical application of SQL JOIN operations, an essential tool for querying and managing databases. We examined how `INNER JOIN`, `LEFT OUTER JOIN`, `RIGHT OUTER JOIN`, and `FULL OUTER JOIN` work using a hypothetical customer-account relationship. The `INNER JOIN` combines records with matching IDs, omitting any without a match. The `LEFT OUTER JOIN` ensures all customer records appear, adding nulls where there's no account match. The `RIGHT OUTER JOIN` includes all account records, with nulls for unmatched customers. And the `FULL OUTER JOIN` captures every record from both tables, with nulls filling the gaps. As we prepare for practical sessions, it's crucial to grasp these concepts and apply them through the detailed examples and tasks in your lab materials, especially if SQL is new to you. Your workshops will provide further practice, reinforcing these operations. I encourage you to engage with these resources—they’re designed to build your proficiency. Thank you for your attention, and I look forward to our next session.

在今天的讲座中，我们深入探讨了SQL JOIN操作的实际应用，这是查询和管理数据库的重要工具。我们用一个假设的客户-账户关系来检验`INNER JOIN`、`LEFT OUTER JOIN`、`RIGHT OUTER JOIN`和`FULL OUTER JOIN`的工作原理。`INNER JOIN`结合具有匹配ID的记录，省略任何没有匹配的记录。`LEFT OUTER JOIN`确保所有客户记录出现，添加空值在没有账户匹配的地方。`RIGHT OUTER JOIN`包括所有账户记录，对于未匹配的客户则为空。而`FULL OUTER JOIN`捕捉两个表中的每条记录，用空值填补间隙。当我们为实践课程做准备时，掌握这些概念并通过您的实验材料中的详细例子和任务应用它们至关重要，尤其是如果SQL对您来说是新的。您的研讨会将提供进一步的实践，强化这些操作。我鼓励您使用这些资源——它们旨在提高您的熟练度。感谢您的关注，期待我们的下一次课程。