$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140021621.png" alt="image-20240420140021621" style="zoom:50%;" /> 

This is one of several possible architectures; each system has its own slight variations.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140104341.png" alt="image-20240420140104341" style="zoom:50%;" /> 

```
大家好，欢迎回到数据库系统系列第 13 讲。今天，我们将深入探讨查询优化。作为开场白，请记住优化器是查询处理模块的重要组成部分，这也是我们上周开始探讨的主题。到此为止，我们已经讨论了执行器，详细介绍了执行计划的构建模块。我们还研究了各种操作（无论是连接还是访问路径）是如何执行的。今天，我们将把这些元素拼凑在一起，了解它们如何构成一个全面的查询执行计划。
Hello, everyone, and welcome back to lecture 13 of our database systems series. Today, we'll delve into query optimization. To set the stage, remember that the optimizer is a crucial component of the query processing module, a topic we began exploring last week. Up to this point, we've discussed the executor, detailing the building blocks of execution plans. We've examined how various operations, whether joins or access paths, are executed. Today, we will piece these elements together to understand how they form a comprehensive query execution plan.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140122998.png" alt="image-20240420140122998" style="zoom:50%;" /> 

**• Overview**
• Query optimization
• Cost estimation

```
For this lecture. We will use material from chapters 12 and 15 of the database management systems book. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140203113.png" alt="image-20240420140203113" style="zoom:50%;" /> 

```sql
Select *
From Blah B
Where B.blah = “foo”
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140227459.png" alt="image-20240420140227459" style="zoom:50%;" /> 

```
回顾一下我们已经讨论过的工作流程。用户输入查询后，查询首先会到达查询解析器，由其检查语法并进行一些重写。然后，查询会进入查询优化器，如前所述，查询优化器是数据库系统的大脑，负责处理最复杂的任务。优化器的主要作用是探索执行查询的所有可能方式，这一阶段称为计划生成。然后，每个可能的计划都要使用估算器进行成本评估，估算器依赖于目录中的基本信息，我们今天将讨论这些信息。选出最具成本效益的计划，并将其传递给查询计划评估器或执行器。然后，执行器会执行查询计划--一连串旨在检索所需结果的步骤。在这个执行阶段，所有内容都会整合在一起产生结果。
Just to recap, let's revisit the workflow we've already discussed. Once a query is entered by a user, it first reaches the query parser, which checks the syntax and may perform some rewriting. Then, the query progresses to the query optimizer, which, as I've mentioned, acts as the brain of database systems, handling the most complex tasks. The primary role of the optimizer is to explore all possible ways a query can be executed, a phase known as plan generation. Each potential plan is then evaluated for its cost using an estimator that relies on basic information from the catalog, which we will discuss today. The most cost-effective plan is selected and passed to the query plan evaluator or executor. This executor then implements the query plan—a sequence of steps designed to retrieve the desired results. This execution phase is where everything comes together to produce the outcome.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140309242.png" alt="image-20240420140309242" style="zoom:50%;" /> 

**1️⃣**Typically there are many ways of executing a given query, all giving the same answer

**2️⃣**Cost of alternative methods often **varies enormously**

**3️⃣**Query optimization aims to find the execution strategy with the lowest cost

**4️⃣**We will cover:

1. Relational algebra equivalences
2. Cost estimation: Result size estimation and reduction factors
3. Enumeration of alternative plans

```
查询优化可以说是数据库管理中最关键的一环。执行单个查询的方法有很多，虽然所有替代方法都可能产生相同的结果，但与这些方法相关的成本却可能相差很大。一个未经优化的查询可能需要几天甚至几个月的时间才能执行，而一个经过优化的查询可以更快地产生结果。查询优化的主要目标是找到检索所需信息的最有效步骤序列。这种效率不仅对数据库性能至关重要，而且对使用这些数据库的人--无论是学生、专业人士还是企业--的工作效率和满意度也至关重要。
Query optimization is arguably the most crucial aspect of database management. There are numerous ways to execute a single query, and while all alternatives may yield the same result, the costs associated with these methods can vary significantly. An unoptimized query might take days or even months to execute, whereas an optimized query can produce results much faster. The primary goal of query optimization is to find the most efficient sequence of steps to retrieve the desired information. This efficiency is vital not only for database performance but also for the productivity and satisfaction of those who use these databases—whether students, professionals, or businesses.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140309242.png" alt="image-20240420140309242" style="zoom:50%;" /> 

**1️⃣**Typically there are many ways of executing a given query, all giving the same answer

**2️⃣**Cost of alternative methods often **varies enormously**

**3️⃣**Query optimization aims to find the execution strategy with the lowest cost

**4️⃣**We will cover:

1. Relational algebra equivalences
2. Cost estimation: Result size estimation and reduction factors
3. Enumeration of alternative plans

```
尽管查询优化非常重要，但从学生到专业人士，很多人都觉得查询优化令人生畏，宁愿避免处理它。然而，查询优化的基本原理非常简单。优化器利用关系代数等价关系来探索替代执行计划，确保所有可能的方法都能产生准确的结果。通过理解和应用简单的算法，任何人都能显著提高查询性能。这些知识在工作中非常宝贵，因为优化查询可以大大加快数据分析的速度，受益的不仅是自己，还有同事、客户和上级。
Despite its importance, many individuals, from students to professionals, find query optimization daunting and prefer to avoid dealing with it. However, the underlying principles of query optimization are quite straightforward. The optimizer leverages relational algebra equivalences to explore alternative execution plans, ensuring that all possible approaches yield accurate results. By understanding and applying simple algorithms, anyone can significantly enhance the performance of their queries. This knowledge is invaluable in the workforce, where optimizing a query can drastically speed up data analysis, benefiting not just oneself but also colleagues, clients, and superiors.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140309242.png" alt="image-20240420140309242" style="zoom:50%;" /> 

**1️⃣**Typically there are many ways of executing a given query, all giving the same answer

**2️⃣**Cost of alternative methods often **varies enormously**

**3️⃣**Query optimization aims to find the execution strategy with the lowest cost

**4️⃣**We will cover:

1. Relational algebra equivalences
2. Cost estimation: Result size estimation and reduction factors
3. Enumeration of alternative plans

```
实际上，查询优化包括两个关键部分：估计结果大小和计算执行成本。结果大小或产生的元组数量至关重要，因为它会影响查询计划中的后续操作。了解输入大小有助于准确计算每个操作的成本，无论是连接、选择还是投影。明天，我们将深入探讨如何将这些元素集成到全面的查询计划中，并将我们讨论过的所有概念连接到优化数据库查询的统一策略中。
In practical terms, query optimization involves two key components: estimating the result size and calculating the cost of execution. The result size, or the number of tuples produced, is critical because it affects subsequent operations within the query plan. Knowing the input size helps in accurately costing each operation, whether it's a join, selection, or projection. Tomorrow, we will delve deeper into how these elements are integrated into comprehensive query plans, connecting all the concepts we’ve discussed into a cohesive strategy for optimizing database queries.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140513012.png" alt="image-20240420140513012" style="zoom:50%;" /> 

**1️⃣**A tree, with relational algebra operators as nodes and accesspaths as leaves

**2️⃣**Each operator labeled with a choice of algorithm

```sql
SELECT snamefrom Sailors NATURAL JOIN Reserves
WHERE bid = 100 and rating > 5
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140558041.png" alt="image-20240420140558041" style="zoom:50%;" /> 

```
查询计划本质上是一个蓝图，概述了数据库查询应该如何执行。虽然我之前对它的描述有些模糊，但重要的是要了解查询计划的结构是一棵关系代数树。树中的节点是关系代数运算符，树叶代表访问路径，表示应如何访问数据。每个节点还与特定算法相关联，反映了我们讨论过的连接、选择和投影等操作的各种实现策略。
A query plan is essentially a blueprint that outlines how a database query should be executed. While I've previously described it somewhat vaguely, it’s important to understand that a query plan is structured as a relational algebra tree. The nodes in this tree are relational algebra operators, and the leaves represent access paths indicating how data should be accessed. Each node is also associated with a specific algorithm, reflecting the various implementation strategies we've discussed for operations such as joins, selections, and projections.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140513012.png" alt="image-20240420140513012" style="zoom:50%;" /> 

**1️⃣**A tree, with relational algebra operators as nodes and accesspaths as leaves

**2️⃣**Each operator labeled with a choice of algorithm

```sql
SELECT snamefrom Sailors NATURAL JOIN Reserves
WHERE bid = 100 and rating > 5
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140558041.png" alt="image-20240420140558041" style="zoom:50%;" /> 

```
让我们举例说明如何构建查询计划。在一个简单的查询中，我们从与后备人员自然连接的水手中选择姓名，其中ID等于10，等级大于5。在我们计划的最底层，我们有一个自然连接操作，其中水手的 ID 与后备役水手的 ID 相匹配。在此基础上，我们根据两个谓词进行选择，检查是否满足条件。如果这些条件都满足，我们就进入预测阶段。这种分层设置确保了每个操作在逻辑上都是有序的。
Let’s consider an example to illustrate how we construct a query plan. Take a simple query where we select the name from sailors naturally joined with reserves, where both the ID equals 10 and the rating is greater than five. At the base of our plan, we have a natural join operation where a sailor's ID matches a reserve's sailor ID. Above this, we perform selections based on two predicates to check if the conditions are met. If these are satisfied, we proceed to the projection stage. This hierarchical setup ensures that each operation is logically and sequentially organized.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140513012.png" alt="image-20240420140513012" style="zoom:50%;" /> 

**1️⃣**A tree, with relational algebra operators as nodes and accesspaths as leaves

**2️⃣**Each operator labeled with a choice of algorithm

```sql
SELECT snamefrom Sailors NATURAL JOIN Reserves
WHERE bid = 100 and rating > 5
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140558041.png" alt="image-20240420140558041" style="zoom:50%;" /> 

```
要理解查询计划中的操作流程，可以把它想象成流经河流的水，图元从底部向上移动。一开始，"水手 "表中的图元被拉上来，接着是 "储备 "表中的图元。然后通过连接操作检查是否有匹配的 ID。如果一个元组不符合 ID 大于 10 和评级超过 5 的后续条件，它就会被丢弃。否则，它将在我们所说的流水线--数据处理的连续流中继续向上运行。
To understand the flow of operations in a query plan, think of it as water flowing through a river, where tuples move from the bottom upwards. Initially, tuples from the "sailors" table are pulled up, followed by tuples from the "reserves". These are then passed through a join operation to check for matching IDs. If a tuple fails to meet the subsequent conditions of having an ID greater than 10 and a rating over five, it is dis🚗ded. Otherwise, it continues upwards in what we call a pipeline—a continuous flow of data processing.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140513012.png" alt="image-20240420140513012" style="zoom:50%;" /> 

**1️⃣**A tree, with relational algebra operators as nodes and accesspaths as leaves

**2️⃣**Each operator labeled with a choice of algorithm

```sql
SELECT snamefrom Sailors NATURAL JOIN Reserves
WHERE bid = 100 and rating > 5
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140558041.png" alt="image-20240420140558041" style="zoom:50%;" /> 

```
最后，计算这些计划的成本至关重要。成本一般用 I/O 操作数来表示，主要是访问了多少页，因为这是执行查询时最耗费资源的部分。举例来说，成本可以用 500 + 500 * 1000 这样的公式计算，分别代表水手表和储备表的大小。这种计算使用面向页面的嵌套循环方法。当图元满足条件并在计划中移动时，操作将 "在运行中 "执行，这意味着它们将被实时处理，而不会存储回磁盘，从而降低了成本并提高了效率。
Finally, costing these plans is crucial. Costs are generally expressed in terms of the number of I/O operations, primarily how many pages are accessed, as this is the most resource-intensive part of executing a query. Using the example, the cost might be calculated using a formula like 500 + 500 * 1000, representing the sizes of the sailors and reserves tables, respectively. This calculation uses the page-oriented nested loops approach. As tuples meet conditions and move through the plan, operations are performed "on the fly", meaning they're processed in real-time without being stored back to disk—minimizing cost and enhancing efficiency.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140622885.png" alt="image-20240420140622885" style="zoom:50%;" /> 

• Overview
**• Query optimization**
• Cost estimation

```
So now let's focus on query optimization.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140704730.png" alt="image-20240420140704730" style="zoom:50%;" /> 

Sailors (**sid: integer**, sname: string, rating: integer, age: real)
Reserves (**sid: integer, bid: integer, day: dates**, rname: string)
Boats (**bid: integer,** bname: string, color: string)

```
在整个讨论过程中，我将继续使用涉及水手、预备队和船只的模式。如果在我提供的示例中出现任何困惑，我鼓励你们重新审视这一示意图，以澄清任何不确定之处。这种方法应有助于确保我们探讨的概念清晰易懂。
I will continue using the schema involving sailors, reserves, and boats throughout our discussions. If any confusion arises from the examples I provide, I encourage you to revisit this schema to clarify any uncertainties. This approach should help ensure that the concepts we explore are clear and understandable.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420140830855.png" alt="image-20240420140830855" style="zoom:50%;" /> 

**1️⃣**Example:

```sql
SELECT S.sname FROM Reserves R, Sailors S
WHERE R.sid=S.sid AND R.bid=100 AND S.rating>5
```

**2️⃣**Query optimization steps:

1. Query first broken into “blocks”
2. Each block converted to relational algebra
3. Then, for each block, several alternative **query plans** are considered
4. Plan with the lowest **estimated cost** is selected

```
查询优化器分四个不同步骤执行任务，这对理解其整体功能至关重要。首先，每个查询都被分割成块，每个块代表一个以 select 子句开头的不同语句。这种分割在处理嵌套语句时尤为重要--以 select 开头的每个不同部分都被视为一个块。对于每个块，我们将其转化为关系代数。然后，我们利用关系代数的等价性，探索该语句块的所有可能替代方案。我们对每个备选方案的成本进行评估，然后为每个区块选择估计成本最低的计划。这一过程以关系代数规则为指导原则，按顺序一个区块一个区块地进行。请记住，执行顺序是从最内层的程序块开始，然后向外推进，每次处理关系代数层次结构中的一个选择语句。
The query optimizer performs its tasks in four distinct steps, which are crucial for understanding its overall function. Initially, each query is segmented into blocks, where each block represents a distinct statement beginning with a select clause. This segmentation is particularly important when dealing with nested statements—each distinct part that begins with select is recognized as a block. For each block, we translate it into relational algebra. Utilizing relational algebra equivalences, we then explore all possible alternatives for that block. Each alternative is assessed for its cost, and we select the plan with the lowest estimated cost for each block. This process is done sequentially, one block at a time, using relational algebra rules as guiding principles. Remember, the execution order starts with the innermost block and progresses outward, handling one select statement at a time in terms of relational algebra hierarchy.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141015293.png" alt="image-20240420141015293" style="zoom:50%;" /> 

**1️⃣**Query block is any statement starting with select

**2️⃣**Query block = unit of optimization

**3️⃣**Typically inner most block is optimized first, then moving towards outers

```sql
SELECT S.sname   -- Outer block
FROM Sailors S
WHERE S.ageIN
```

```sql
(SELECT MAX (S2.age)   -- Nested block
 FROM Sailors S2
 GROUP BY S2.rating)
```

```
将查询分解成代码块非常简单。基本上，每当语句以 select 开始，就定义了一个新的查询块。然后，这个查询块就成为一个优化单元。在分析计划时，这些查询通常会变得相当复杂。通常情况下，执行过程从最内层的代码块开始，然后一层一层向外推进。这种有条不紊的方法可确保查询的每个部分都得到有效优化，然后再进入下一个部分。
Breaking down a query into blocks is straightforward. Essentially, each time a statement starts with a select, it defines a new block. This query block then becomes a unit of optimization. When analyzing plans, these queries can often become quite complex. Typically, the execution process begins with the innermost block and then progresses outward, one layer at a time. This methodical approach ensures that each part of the query is optimized effectively before moving on to the next.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141203095.png" alt="image-20240420141203095" style="zoom:50%;" /> 

**1️⃣**Query:

```sql
SELECT S.sid
FROM Sailors S, Reserves R, Boats B
WHERE S.sid= R.sidAND R.bid= B.bidAND B.color= “red”
```

**2️⃣**Relational algebra:
$$
\Pi_{S.sid} (\sigma_{B.color = ''red''} (Sailors \bowtie Reserves \bowtie Boats))
$$

```
对于每个数据块，我们都要将其转换为关系代数表达式，而这正是扎实理解关系代数的关键所在。下面的示例可以说明这一过程：考虑一个涉及三个表和一个条件的选择子句--水手，其中水手 ID 等于后备水手 ID；船只，其中船只 ID 等于后备船只 ID，且船只颜色为红色。在关系代数中，这种设置转化为表 Sailors、Reserves 和 Boats 之间的自然连接。特定条件 "红色 "在关系代数中表示为选择操作。最后，操作以投影结束，在投影中只选择并显示水手 ID。这一细分囊括了这一特定查询块在关系代数中的转换步骤。
For each block, we transform it into a relational algebra expression, which is where a solid understanding of relational algebra becomes crucial. Here's an example to illustrate this process: consider a select clause involving three tables and a condition—Sailor, where Sailor ID equals Reserve Sailor ID, Boat where Boat ID equals Reserves Boat ID, and the boats are colored red. In relational algebra, this setup translates into a natural join between the tables Sailors, Reserves, and Boats. The specific condition, 'color red', is represented as a selection operation in relational algebra. Finally, the operation concludes with a projection, where only the Sailor ID is selected and displayed. This breakdown encapsulates the transformation steps in relational algebra for this particular query block.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

**1️⃣**Selections:

$\sigma_{c_1 \wedge \cdots \wedge c_n}(R) \equiv \sigma_{c_1}\left(\ldots\left(\sigma_{c_n}(R)\right)\right)$ (Cascade)

$\sigma_{c_1}\left(\sigma_{c_2}(R)\right) \equiv \sigma_{c_2}\left(\sigma_{c_1}(R)\right)$ (Commute)

**2️⃣**Projections: 

$\pi_{a_1}(R) \equiv \pi_{a_1}\left(\ldots\left(\pi_{a_n}(R)\right)\right)$ (Cascade) 

$a_i$ is a set of attributes of $\mathrm{R}$ and $a_i \subseteq a_{i+1}$ for $i=1 \ldots n-1$​

**3️⃣**These equivalences allow us to 'push' selections and projections ahead of joins.

```
在优化查询的过程中，我们会探索关系代数等价关系，以确定不同的可执行计划。关系理论确保无论我们选择哪种方案，结果都能保持一致。这种可靠性源于关系代数的一些基本规则，它们为我们提供了操作和试验各种备选方案的灵活性。我们将深入探讨这些规则，尤其是它们如何应用于查询计划中的选择。
In the process of optimizing queries, we explore relational algebra equivalences to identify different executable plans. Relational theory ensures that no matter which option we choose, the result remains consistent. This reliability stems from a few fundamental rules of relational algebra that provide us with the flexibility to manipulate and experiment with various alternatives. We are about to dive into these rules, particularly focusing on how they apply to selections in query plans.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

**1️⃣**Selections:

$\sigma_{c_1 \wedge \cdots \wedge c_n}(R) \equiv \sigma_{c_1}\left(\ldots\left(\sigma_{c_n}(R)\right)\right)$ (Cascade)

$\sigma_{c_1}\left(\sigma_{c_2}(R)\right) \equiv \sigma_{c_2}\left(\sigma_{c_1}(R)\right)$ (Commute)

**2️⃣**Projections: 

$\pi_{a_1}(R) \equiv \pi_{a_1}\left(\ldots\left(\pi_{a_n}(R)\right)\right)$ (Cascade) 

$a_i$ is a set of attributes of $\mathrm{R}$ and $a_i \subseteq a_{i+1}$ for $i=1 \ldots n-1$​

**3️⃣**These equivalences allow us to 'push' selections and projections ahead of joins.

```
第一套规则涉及选择的应用。这里有两条主要规则：选择项可以级联方式或交换方式应用。对于级联选择，可以想象一系列条件，例如年龄大于 5 岁和评分大于 7 分。无论这些条件是在表中一次性应用，还是一个接一个地顺序应用，结果都是一样的。符合这些条件的记录将是相同的，这表明了选择的级联性质。另一方面，选择的交换规则表明，条件的顺序不会影响结果。例如，无论是先应用 "条件一"，再应用 "条件二"，还是先应用 "条件二"，再应用 "条件一"，合格记录的集合都不会改变。这一规则凸显了关系代数在查询优化中抽象而强大的特性。
The first set of rules involves the application of selections. There are two main rules here: selections can be applied in a cascade or in a commutative manner. For cascading selections, imagine a series of conditions—such as age greater than five and rating greater than seven. Whether these conditions are applied all at once over a table or sequentially, one after another, the outcome is the same. The records that meet these criteria will be the same, demonstrating the cascading nature of selections. On the other hand, the commutative rule of selections shows that the order of conditions does not affect the result. For example, whether we apply 'condition one' followed by 'condition two', or 'condition two' followed by 'condition one', the set of qualifying records remains unchanged. This rule highlights the abstract yet powerful nature of relational algebra in query optimization.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420214207261.png" alt="image-20240420214207261" style="zoom: 67%;" /> 

```
举个例子来说明，让我们考虑一下 "水手 "表，并应用一些条件：选择年龄大于 50 且等级等于 10 的水手。在关系代数中，可以这样处理：首先根据年龄筛选水手，然后将这些结果向前推，随后检查他们的评级。由于这些运算的交换属性，这个顺序实际上等同于先检查等级，再检查年龄；条件的顺序不会影响最终结果。无论采用哪种方式，输出结果都保持一致，这表明这两种方法是等价的，并验证了处理查询条件的灵活性。
To illustrate with an example, let's consider our table 'sailors' and apply some conditions: select sailors where age is greater than 50 and rating equals 10. In relational algebra, this could be approached by first filtering sailors based on age, then pushing those results forward, and subsequently checking their rating. This sequence is effectively equivalent to checking the rating first and then age due to the commutative property of these operations; the order of the conditions doesn’t impact the final result. Either way, the output remains consistent, demonstrating that both approaches are equivalent and validate the flexibility in processing query conditions.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

**1️⃣**Selections:

$\sigma_{c_1 \wedge \cdots \wedge c_n}(R) \equiv \sigma_{c_1}\left(\ldots\left(\sigma_{c_n}(R)\right)\right)$ (Cascade)

$\sigma_{c_1}\left(\sigma_{c_2}(R)\right) \equiv \sigma_{c_2}\left(\sigma_{c_1}(R)\right)$ (Commute)

**2️⃣**Projections: 

$\pi_{a_1}(R) \equiv \pi_{a_1}\left(\ldots\left(\pi_{a_n}(R)\right)\right)$ (Cascade) 

$a_i$ is a set of attributes of $\mathrm{R}$ and $a_i \subseteq a_{i+1}$ for $i=1 \ldots n-1$​

**3️⃣**These equivalences allow us to 'push' selections and projections ahead of joins.

```
在投影方面，有一种涉及级联投影的微妙技术。其原理非常简单：如果有一连串的投影，那么每一个后续投影都必须涉及前一个投影中包含的列的子集。这种方法乍看之下可能像复杂的数学，但如果考虑到数据库表的结构和功能，它实际上是非常合乎逻辑的。通过确保每个投影比上一个投影更能缩小焦点，我们就能有效地逐步完善数据输出。
Moving on to projection, there's a nuanced technique involving cascading projections. The principle here is quite straightforward: if you have a sequence of projections, each subsequent projection must involve a subset of the columns included in the previous one. This approach, which might seem like complex math at first glance, is actually quite logical when you consider the structure and function of database tables. By ensuring each projection narrows the focus more than the last, we efficiently refine the data output step-by-step.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420214723572.png" alt="image-20240420214723572" style="zoom: 33%;" /> 

```
让我们举例说明级联预测的概念。假设我们有一个名为 "水手 "的表，其中包含 ID、年龄、姓名、排名等多列。在第一次投影中，为了简单起见，我们决定只保留前三列--ID、Age 和 Name。这样一来，我们实际上就舍弃了包括排名在内的其他列。现在，如果我们想从第一列级联应用第二列投影，我们就只能使用初始投影中保留的列。例如，我们不能将 "排名 "包含在后续的预测中，因为它没有包含在第一次预测中。但是，如果需要的话，我们可以选择只预测 "姓名"，因为它仍然可以从上一个预测的输出中获得。这种技术确保了每个后续投影只能使用前一个投影中可用列的子集，这是理解级联投影的关键所在。
Let's explore an example to illustrate the concept of cascading projections. Imagine we have a table called 'sailors' with multiple columns such as ID, Age, Name, Ranking, and others. In our first projection, we decide to keep only the first three columns—ID, Age, and Name—for simplicity. By doing this, we have effectively dis🚗ded the other columns, including Ranking. Now, if we wish to apply a second projection cascading from the first, we are limited to using only the columns retained from the initial projection. For instance, we cannot include Ranking in our subsequent projection since it was not included in the first projection. However, we could choose to project only the Name if desired, as it remains available from the previous projection's output. This technique ensures that each subsequent projection can only work with a subset of the columns available from the prior one, which is the key to understanding cascading projections.
```
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141428733.png" alt="image-20240420141428733" style="zoom:50%;" /> 

**1️⃣**Selections:

$\sigma_{c_1 \wedge \cdots \wedge c_n}(R) \equiv \sigma_{c_1}\left(\ldots\left(\sigma_{c_n}(R)\right)\right)$ (Cascade)

$\sigma_{c_1}\left(\sigma_{c_2}(R)\right) \equiv \sigma_{c_2}\left(\sigma_{c_1}(R)\right)$ (Commute)

**2️⃣**Projections: 

$\pi_{a_1}(R) \equiv \pi_{a_1}\left(\ldots\left(\pi_{a_n}(R)\right)\right)$ (Cascade) 

$a_i$ is a set of attributes of $\mathrm{R}$ and $a_i \subseteq a_{i+1}$ for $i=1 \ldots n-1$​

**3️⃣**These equivalences allow us to 'push' selections and projections ahead of joins.

```
现在，这些等价关系本质上允许我们将选择或预测推到琼斯的前面，本质上是在玩另类游戏。
Now these equivalences essentially allow us to push selection or projection ahead of Jones essentially to play with the alternatives. 
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141906601.png" alt="image-20240420141906601" style="zoom:50%;" /> 

**1️⃣**Selection: 
$
\begin{aligned}
& \sigma_{\text {age }<18 \wedge \text { rating }>5} \text { (Sailors) } \\
& \leftrightarrow \sigma_{\text {age }<18}\left(\sigma_{\text {rating }>5}\right. \text { (Sailors)) } \\
& \leftrightarrow \sigma_{\text {rating }>5}\left(\sigma_{\text {age }<18}(\text { Sailors })\right) \\
&
\end{aligned}
$

```
在这个示例中，我们最初将投影向下推，但无意中丢失了 "评级"，这意味着我们在后续连接中评估条件时无法使用这个字段。值得注意的是，在关系代数中，连接既是关联的，也是交换的。这种灵活性允许我们在不改变结果的情况下重新安排连接。例如，我们可以先在表 S 和表 T 之间执行连接，然后将结果与表 R 连接，或者，我们也可以先连接 R 和 S，然后再整合 T。无论哪种情况，最终结果都是一致的，这展示了这些关系代数特性在优化查询执行方面的强大功能。
In this example, we initially push the projection down but inadvertently lose the 'rating', which means we can't use this field when evaluating conditions in subsequent joins. It's important to note that in relational algebra, joins are both associative and commutative. This flexibility allows us to rearrange the joins without altering the outcome. For instance, we can first perform a join between tables S and T, and then join the result with table R, or alternatively, we could start by joining R and S and then integrate T. In either scenario, the final result remains consistent, showcasing the power of these relational algebra properties in optimizing query execution.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141906601.png" alt="image-20240420141906601" style="zoom:50%;" /> 

**1️⃣**Selection: 
$
\begin{aligned}
& \sigma_{\text {age }<18 \wedge \text { rating }>5} \text { (Sailors) } \\
& \leftrightarrow \sigma_{\text {age }<18}\left(\sigma_{\text {rating }>5}\right. \text { (Sailors)) } \\
& \leftrightarrow \sigma_{\text {rating }>5}\left(\sigma_{\text {age }<18}(\text { Sailors })\right) \\
&
\end{aligned}
$
**2️⃣**Projection:

1. No: $
   \pi_{\text {age,rating }}(\text { Sailors }) \leftrightarrow \pi_{\text {age}}\left(\pi_{\text {rating}}(\text { Sailors })\right)
   $

2. Yes: $
   \pi_{\text {age,rating }}(\text { Sailors }) \leftrightarrow \pi_{\text {age,rating }}\left(\pi_{\text {age,rating,sid }}(\text { Sailors })\right)
   $

```
考虑关系代数中的一种情况：我们有一个包含两个谓词的选择操作：年龄小于 18 岁和评分大于 5 分。这可以通过分别检查每个条件来执行，从而展示级联规则的应用。另外，由于关系代数的交换属性，我们也可以按照任意顺序应用这些条件，从而说明应用条件的顺序不会影响结果。这两种方法都能确保结果保持一致，突出了关系代数等价的灵活性和强大功能。
Consider a scenario in relational algebra where we have a selection operation with two predicates: age less than 18 and rating greater than five. This can be executed by checking each condition separately, demonstrating the application of cascading rules. Alternatively, we can apply these conditions in any order due to the commutative property of relational algebra, illustrating that the sequence in which conditions are applied does not impact the outcome. Both methods ensure that the result remains consistent, highlighting the flexibility and power of relational algebra equivalences.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420141906601.png" alt="image-20240420141906601" style="zoom:50%;" /> 

**1️⃣**Selection: 
$
\begin{aligned}
& \sigma_{\text {age }<18 \wedge \text { rating }>5} \text { (Sailors) } \\
& \leftrightarrow \sigma_{\text {age }<18}\left(\sigma_{\text {rating }>5}\right. \text { (Sailors)) } \\
& \leftrightarrow \sigma_{\text {rating }>5}\left(\sigma_{\text {age }<18}(\text { Sailors })\right) \\
&
\end{aligned}
$
**2️⃣**Projection:

1. No: $
   \pi_{\text {age,rating }}(\text { Sailors }) \leftrightarrow \pi_{\text {age}}\left(\pi_{\text {rating}}(\text { Sailors })\right)
   $

2. Yes: $
   \pi_{\text {age,rating }}(\text { Sailors }) \leftrightarrow \pi_{\text {age,rating }}\left(\pi_{\text {age,rating,sid }}(\text { Sailors })\right)
   $

```
接下来，让我们来看一个涉及投影的更复杂的例子。假设我们对数据集中的 "年龄 "和 "评分 "进行了投影，但却只对 "评分 "进行了一连串的投影。这会导致 "年龄 "数据丢失，从而无法在后续操作中引用 "年龄 "数据。这种情况清楚地表明，当属性在序列早期被消除时，投影并不总是等价的。为了在整个查询过程中有效管理数据属性，可以先预测 "年龄"、"评级 "和 "ID"，然后再应用一个保留 "年龄 "和 "评级 "的条件。这种方法可确保最终输出保持不变，从而强调了仔细考虑查询计划中每个操作的输出的必要性。
Next, let’s examine a more complex example involving projection. Suppose we project 'age' and 'rating' from a dataset but then attempt to apply a cascade of projections, focusing only on 'rating'. This results in a loss of the 'age' data, making it impossible to reference 'age' in subsequent operations. This scenario clearly shows that projections are not always equivalent when attributes are eliminated early in the sequence. To effectively manage data attributes throughout the query process, one could initially project 'age', 'rating', and 'ID', then apply a further condition that retains 'age' and 'rating'. This approach ensures that the final output remains unchanged, emphasizing the need to 🚗efully consider the output of each operation in a query plan.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143028677.png" alt="image-20240420143028677" style="zoom:50%;" /> 

**1️⃣**A projection commutes with a selection that only uses attributes retained by the projection

1. Yes

$
\begin{aligned}
& \pi_{\text {age, rating, sid }}\left(\sigma_{\text {age }<18 \wedge \text { rating }>5}(\text { Sailors })\right) \\
& \leftrightarrow \sigma_{\text {age }<18 \wedge \text { rating }>5}\left(\pi_{\text {age, rating, sid }}(\text { Sailors })\right)
\end{aligned}
$

```
投影的确可以与选择换算，但这只有在选择涉及到投影所保留的属性时才可行。在我们讨论的例子中，策略是在执行选择之前翻转顺序并应用投影。这里的关键是检查必要的属性（如年龄和评级）是否保留在投影列表中。幸运的是，在本例中，评分在投影中得到了保留，因此重新排列是有效的。这种方法表明，仔细规划投影中包含的属性可以实现灵活高效的查询优化。
Projection can indeed commute with a selection, but this is only feasible if the selection involves attributes that are preserved by the projection. In the example we are discussing, the strategy was to flip the order and apply projection before performing the selection. The crucial check here is whether the necessary attributes, like age and rating, are retained in the projection list. Fortunately, in this case, rating has been preserved in the projection, making this rearrangement valid. This approach shows how 🚗eful planning of attribute inclusion in projections can enable flexible and efficient query optimization.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143028677.png" alt="image-20240420143028677" style="zoom:50%;" /> 

**1️⃣**A projection commutes with a selection that only uses attributes retained by the projection

1. Yes

$
\begin{aligned}
& \pi_{\text {age, rating, sid }}\left(\sigma_{\text {age }<18 \wedge \text { rating }>5}(\text { Sailors })\right) \\
& \leftrightarrow \sigma_{\text {age }<18 \wedge \text { rating }>5}\left(\pi_{\text {age, rating, sid }}(\text { Sailors })\right)
\end{aligned}
$

2. No

$
\begin{aligned}
& \pi_{\text {age, rating, sid }}\left(\sigma_{\text {age }<18 \wedge \text { rating }>5}(\text { Sailors })\right) \\
& \leftrightarrow \sigma_{\text {age }<18 \wedge \text { rating }>5}\left(\pi_{\text {age, sid }}(\text { Sailors })\right)
\end{aligned}
$

```
下面是另一个常见的绊倒学生的例子--通过翻转并将投影推到序列的前面，试图利用等价原理。然而，这种做法会导致 "评级 "属性丢失。因此，由于 "等级 "不再可用，我们在进行连接时就无法在条件中应用它。这说明了一个关键点：在重新排序操作时，需要确保保留所有必要的属性，以避免导致某些条件无法检查，从而影响查询的正确性。
Here's another example that commonly trips up students—it involves an attempt to utilize the equivalence principle by flipping and pushing the projection ahead in the sequence. However, this maneuver results in the loss of the 'rating' attribute. Consequently, because 'rating' is no longer available, we are unable to apply it in conditions when performing joins. This illustrates a crucial point: the need to ensure that all necessary attributes are retained when reordering operations to avoid rendering certain conditions uncheckable and potentially compromising the query's correctness.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143310274.png" alt="image-20240420143310274" style="zoom:50%;" /> 

$
\begin{array}{ll}
R \bowtie(S \bowtie T) \equiv(R \bowtie S) \bowtie T & \text { (Associative) } \\
(R \bowtie S) \equiv(S \bowtie R) & \text { (Commutative) }
\end{array}
$​
These equivalences allow us to choose **different join orders**

```
关系代数为我们提供了一些有用的属性，如连接的关联性和交换性。这意味着我们可以灵活安排表达式中的连接操作。试想一下嵌套连接--最内层的括号先被计算。但是，利用关联性，我们可以选择先执行 S 和 T 之间的连接，然后将结果与 R 连接，或者反之亦然--顺序不会影响最终结果。同样，交换性允许我们交换连接的顺序（S 连接 R 与 R 连接 S 是一样的）。这种灵活性对于优化查询计划至关重要。不同的连接顺序会导致执行成本的显著变化。我们将深入探讨连接顺序和执行选择如何影响查询计划及其相关成本。
Relational algebra offers us some helpful properties like associativity and commutativity for joins. This means we have flexibility in how we arrange join operations within an expression.  Imagine nested joins – the innermost set of brackets gets evaluated first. But with associativity, we can choose to perform the join between S and T first, then join the result with R, or vice versa – the order doesn't affect the final outcome. Similarly, commutativity allows us to swap the order of joins (S join R is the same as R join S). This flexibility is crucial for optimizing query plans. Different join orders can lead to significant cost variations in execution. We'll delve deeper into how join order and implementation choices impact query plans and their associated costs.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143458457.png" alt="image-20240420143458457" style="zoom:50%;" /> 

**1️⃣**Converting selection + cross-product to join

$
\begin{aligned}
\sigma_{\text {S.sid }=\text { R.sid }} & (\text { Sailors } \times \text { Reserves }) \\
& \leftrightarrow \text { Sailors } \bowtie_{\text {S.sid }=\text { R.sid }} \text { Reserves }
\end{aligned}
$​

```
因此，从本质上讲，优化器要做的就是确保所有操作都遵守等价规则，从而保留信息。例如，我们可以考虑将不同的操作结合起来。在这里，我们基本上是在应用这些等价规则，以确保不会丢失任何信息。例如，带后续条件的交乘，本质上就是转化为自然连接。这一点至关重要，因为在 SQL 符号中，我们通常会指定表名和连接条件，这类似于带条件的交叉乘积。不过，我相信优化器有能力不执行代价高昂的交叉乘积。为什么呢？因为我了解关系代数的等价关系，而且我知道优化器会将此类操作改写成更有效的形式，比如自然连接。因此，有了这种理解，我相信优化器会尽可能生成最好的计划。
So, in essence, what the optimizer does is ensure that all operations adhere to the rules of equivalence, thereby preserving information. For instance, consider the case of combining different operations. Here, we're essentially applying these equivalence rules to ensure that no information is lost. Take, for example, the cross product with a subsequent condition, which is essentially transformed into a natural join. This is crucial because, in SQL notation, we often specify table names and join conditions, resembling a cross product with conditions. However, I'm confident in the optimizer's ability not to execute costly cross products. Why? Because I understand relational algebra equivalences, and I know the optimizer will rewrite such operations into more efficient forms, like a natural join. So, with this understanding, I trust that the optimizer will generate the best plan possible.
```
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143458457.png" alt="image-20240420143458457" style="zoom:50%;" /> 

**1️⃣**Converting selection + cross-product to join

$
\begin{aligned}
\sigma_{\text {S.sid }=\text { R.sid }} & (\text { Sailors } \times \text { Reserves }) \\
& \leftrightarrow \text { Sailors } \bowtie_{\text {S.sid }=\text { R.sid }} \text { Reserves }
\end{aligned}
$​
**2️⃣**Selection on just attributes of S commutes with $\text{R}\bowtie\text{S}$​

$
\begin{aligned}
\sigma_{S.age<18} & (\text{Sailors} \bowtie_{S.sid = R.sid} \text{Reserves})\\
& \leftrightarrow
(\sigma_{S.age<18} (\text{Sailors})) \bowtie_{S.sid = R.sid} \text{Reserves}
\end{aligned}
$

```
此外，我们还可以采用下推选择的策略，确保始终保留我们需要的属性。在这个示例中，我们不是先执行连接，然后再应用选择，而是将选择下推。也就是说，我们先检查条件，然后再执行连接。为什么要这样做呢？因为归根结底，在连接之前过滤数据会使操作更加高效。它减少了连接的大小，从而降低了成本。
Additionally, we can also employ the strategy of pushing down selection, ensuring that we always retain the attributes we need. This is an example where instead of initially performing a join and then applying selection, we push the selection down. This means we first check conditions and then perform the join. Why do we do this? Because ultimately, filtering the data before the join makes the operation more efficient. It reduces the size of the join, making it less costly.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420143458457.png" alt="image-20240420143458457" style="zoom:50%;" /> 

**1️⃣**Converting selection + cross-product to join

$
\begin{aligned}
\sigma_{\text {S.sid }=\text { R.sid }} & (\text { Sailors } \times \text { Reserves }) \\
& \leftrightarrow \text { Sailors } \bowtie_{\text {S.sid }=\text { R.sid }} \text { Reserves }
\end{aligned}
$​
**2️⃣**Selection on just attributes of S commutes with $\text{R}\bowtie\text{S}$​

$
\begin{aligned}
\sigma_{S.age<18} & (\text{Sailors} \bowtie_{S.sid = R.sid} \text{Reserves})\\
& \leftrightarrow
(\sigma_{S.age<18} (\text{Sailors})) \bowtie_{S.sid = R.sid} \text{Reserves}
\end{aligned}
$
**3️⃣**We can also “push down”projection (but be 🚗eful…)

$
\begin{aligned}
\Pi_{S.sname} & (\text{Sailors} \bowtie_{S.sid = R.sid} \text{Reserves})\\
& \leftrightarrow
 (\Pi_{S.sname} (\Pi_{sname, sid} (\text{Sailors})) \bowtie_{S.sid = R.sid} \Pi_{sid} (\text{Reserves}))
 \end{aligned}
$​

```
此外，我们还可以将相同的逻辑应用于下推投影。这样，我们从一开始就只保留我们需要的列。这样可以节省内存，大大降低成本。不过，我们必须有选择地保留哪些属性。例如，考虑一下学生们经常犯错的情况：如果我们的目标是只保留水手的姓名，然后将这个子集与后备集连接起来。如果我们压低投影，只保留姓名，就会丢失自然连接所需的 ID 属性。因此，在投影列表中也必须包含 ID。完成连接后，我们就可以只投影名称了。这有什么关系呢？因为通过投影缩小了规模，使用储备合并就会变得更有效率。这种方法会产生一个更好的计划，优化器会识别这个计划。
Furthermore, we can apply the same logic to push down projection. By doing so, we retain only the columns we need right from the start. This allows us to conserve memory and reduce costs significantly. However, it's crucial to be selective about which attributes we retain. For instance, consider the scenario where students often make mistakes: if we aim to keep only the names of sailors and then join this subset with reserves. If we push down projection and keep only the names, we lose the ID attribute necessary for a natural join. Therefore, it's essential to include the ID in the projection list as well. After completing the join, we can then project only the names. Why does this matter? Well, with a reduced size due to projection, merging with reserves becomes much more efficient. This approach yields a better plan, one that the optimizer will identify.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420144447913.png" alt="image-20240420144447913" style="zoom:50%;" /> 

•Overview
• Query optimization
**• Cost estimation**

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420144502935.png" alt="image-20240420144502935" style="zoom:50%;" /> 

**1️⃣**Query first broken into “blocks”

**2️⃣**Each block converted to relational algebra

**3️⃣**Then, for each block, several alternative **query plans** are considered

**4️⃣**Plan with lowest **estimated cost** is selected

```sql
SELECT S.sname FROM Reserves R, Sailors S
WHERE R.sid=S.sidAND R.bid=100 AND S.rating>5
```
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420144653043.png" alt="image-20240420144653043" style="zoom:50%;" />    

$\pi_{\text {(sname) }} \sigma_{(\text {bid }=100 \wedge \text { rating }>5)}(\text { Reserves } \bowtie \text { Sailors })$​

```
现在，让我们把注意力转移到成本优化上。我们之前已经介绍了转换数据块的过程，了解了它们的基本原理以及如何将它们转换成关系代数。我们利用等价访问来有效探索替代方案。
Now, let's shift our attention to cost optimization. We've previously covered the process of transforming blocks, understanding their fundamentals and how they're converted into relational algebra. We utilize equivalent access to explore alternatives effectively.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420144830500.png" alt="image-20240420144830500" style="zoom:50%;" /> 

Step 3&4

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420144946195.png" alt="image-20240420144946195" style="zoom:50%;" /> 

**3️⃣**What plans are considered?

**4️⃣**What is the cost of a plan?

```
And then uh we will now focus on uh these uh costing parts. OK.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420145007982.png" alt="image-20240420145007982" style="zoom: 50%;" /> 

For each plan considered, must estimate cost:

**1️⃣**Must **estimate size of result** for each operation in tree

- Use information about input relations (from the system catalogs), and apply rules **(discussed next)**

**2️⃣**Must **estimate cost** of each operation in plan tree

1. Depends on input 🚗dinalities
2. We’ve already discussed how to estimate the cost of operations (sequential scan, index scan, joins)
3. **Next time we will calculate the cost of entire plans…**

```
在估算每个计划的成本时，计算树中每个操作的结果大小至关重要。这是因为树代表了一系列操作，其中一个操作的输出将成为后续操作的输入。了解流入每个操作的图元数量对于完成这项任务至关重要。今天，我们将深入研究如何确定每个操作的结果大小。接下来，我们将使用上周介绍的公式估算计划中每个操作的成本。我们讨论了连接、嵌套循环连接、散列连接等各种操作，以及堆扫描和索引扫描等访问路径。在下一课中，我们将了解所有这些操作如何在构建计划时结合在一起。
When it comes to estimating the cost of each plan, it's crucial to calculate the result size for every operation within the tree. This is because the tree represents a sequence of operations, where the output of one operation becomes the input for the subsequent one. Understanding how many tuples are flowing into each operator is essential for this task. Today, we'll delve into determining the result size of each operation. Following this, we'll move on to estimating the cost of each operation in the plan using the formulas we covered last week. We discussed various operations such as joins—nested loop joins, hash joins, and others—along with access paths like heap scans and index scans. In our next session, we'll see how all of this comes together in constructing a plan.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420145221198.png" alt="image-20240420145221198" style="zoom:50%;" /> 

**1️⃣**To decide on the cost, the optimizer needs information about the relations and indexes involved. This information is stored in the system **catalogs**.

**2️⃣****Catalogs** typically contain at least:

–  # tuples **(NTuples)** and # pages **(NPages)** per relation

–  # distinct key values **(NKeys)** for eachindex (orrelation attribute)

–  low/high key values **(Low/High)** for each index (orrelation attribute)

–  Index height **(Height(I))** for each tree index

–  # index pages **(NPages(I))** for each index

**3️⃣**Statistics in catalogs are updated periodically 

```
在数据库管理领域，系统目录在优化查询性能方面发挥着举足轻重的作用。这些目录本质上是存储在数据库中的元数据集合，详细说明了数据库结构的各个方面。元数据包括数据元素的描述，如图元（'T'）的数量和每个关系中的页数。这些细节至关重要，因为它们能让优化器了解数据的物理布局和大小，而这对于计算选择、投影和连接的运行成本是不可或缺的。通过参考该目录，优化器可以利用有关数据存储和结构的精确信息做出明智的决策。
In the realm of database management, system catalogs play a pivotal role in optimizing query performance. These catalogs are essentially collections of metadata stored within the database, detailing various aspects of its structure. Metadata, to clarify, includes descriptions of data elements such as the number of tuples ('T') and the number of pages within each relation. These details are crucial as they inform the optimizer about the physical layout and size of the data, which are integral to calculating operational costs for selections, projections, and joins. By referencing this catalog, the optimizer can make informed decisions using precise information about data storage and structure.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420145221198.png" alt="image-20240420145221198" style="zoom:50%;" /> 

**1️⃣**To decide on the cost, the optimizer needs information about the relations and indexes involved. This information is stored in the system **catalogs**.

**2️⃣****Catalogs** typically contain at least:

–  # tuples **(NTuples)** and # pages **(NPages)** per relation

–  # distinct key values **(NKeys)** for eachindex (orrelation attribute)

–  low/high key values **(Low/High)** for each index (orrelation attribute)

–  Index height **(Height(I))** for each tree index

–  # index pages **(NPages(I))** for each index

**3️⃣**Statistics in catalogs are updated periodically 

```
系统目录会为每个关系及其属性维护特定的元数据，例如不同键值的数量，用键的数量来表示，它代表一个域内不同的值。例如，在 "水手 "表中，"排名 "属性的值范围为 1 到 10，因此，不同键值的域为 10。无论记录的实际数量是多少，这一指标都有助于了解数据的分布情况。此外，元数据还包括属性的最小值和最大值，对于索引，还保留了索引高度和索引页数等详细信息。在计算各种索引扫描（无论是聚类还是非聚类）的成本时，这些数字至关重要。值得注意的是，虽然这些数据会定期更新，而且并不总是完全准确，但它提供了一个重要的估算值，有助于评估不同的执行策略，以确定最具成本效益的方案。
The system catalog maintains specific metadata for each relation and its attributes, such as the number of distinct key values, denoted as the number of keys, which represent the distinct values within a domain. For example, consider the 'sailors' table where the 'ranking' attribute's values range from one to ten; thus, the domain of distinct keys is ten. This metric helps in understanding the distribution of data, irrespective of the actual number of records. Additionally, metadata includes the minimum and maximum values for attributes, and for indexes, details like index height and the number of index pages are kept. These figures are essential when calculating costs for various index scans, whether clustered or unclustered. It’s important to note that while this data is periodically updated and not always perfectly accurate, it provides a crucial estimate that aids in evaluating different execution strategies to identify the most cost-effective option.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420145615783.png" alt="image-20240420145615783" style="zoom:50%;" /> 

**1️⃣**Consider a query block:

```sql
SELECT attribute list
FROM relation list
WHERE predicate1 AND ... AND predicate_k
```

**2️⃣**Maximum number of tuples in the result is the **product** of the 🚗dinalities of relations in the FROM clause

**3️⃣****Reduction factor (RF)** associated with each predicate reflects the impact of the predicate in reducing the result size. RF is also called **selectivity.**

```
现在，让我们换个思路，重点计算关系代数表达式中每个操作的结果的估计大小。我们将逐块考虑每个操作。这里，我们将使用一个包含一系列谓词的 SELECT 子句作为示例。关键问题是：输出中最多可以包含多少个表符？对于 FROM 子句中的表序列，这相当于它们的心数（行数）的乘积。从本质上讲，这是假设所有表交叉乘积的粗略估计。不过，WHERE 子句中的每个谓词都起着过滤器的作用，可能会删除部分数据。这种过滤效果由选择性因子捕捉，它告诉我们在应用特定条件后将保留多少百分比的行。选择性在完善我们对结果大小的初始估计（可能会夸大）方面起着至关重要的作用。
Now, let's shift gears and focus on calculating the estimated size of the results for each operation in a relational algebra expression. We'll consider each operation on a block-by-block basis. Here, we'll use an example with a SELECT clause containing a sequence of predicates. The key question: what's the maximum number of tuples we can expect in the output?  For a sequence of tables in the FROM clause, this translates to the product of their 🚗dinalities (number of rows). In essence, this is a rough estimate assuming a cross product of all tables. However, each predicate in the WHERE clause acts as a filter, potentially removing a portion of the data. This filtering effect is captured by the selectivity factor, which tells us what percentage of rows will be kept after applying a specific condition. Selectivity plays a crucial role in refining our initial, potentially inflated, estimate of the result size.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420145740638.png" alt="image-20240420145740638" style="zoom:50%;" /> 

**1️⃣**Single table selection:

$
\text { ResultSize }=\mathrm{NTuples}(R) \prod_{i=1 . . n} R F_i
$​
**2️⃣**Joins (over k tables):

$
\text { ResultSize }=\prod_{j=1 . . k}  \text { NTuples }\left(R_j\right) \prod_{i=1 . . n} R F_i
$​​
**3️⃣**If there are no selections (no predicates), reduction factors are simply ignored, i.e. they are ==1

```
事实上，关系代数为我们提供了强大的等价关系，尤其是在连接方面。正如你提到的，连接既是关联的，也是交换的，这意味着我们可以重新安排它们的顺序，而不会改变最终结果。例如，我们可以先执行表 S 和表 T 之间的连接，然后将结果与表 R 连接，或者反过来先连接 R 和 S，然后再整合 T： 这些特性让我们可以灵活地尝试不同的计划，而且对查询性能的影响可能很大。连接顺序和实现方式的选择会导致计划成本的巨大差异，这突出了优化这些决策以实现最佳查询执行的重要性。
Indeed, relational algebra provides us with powerful equivalences, particularly concerning joins. As you mentioned, joins are both associative and commutative, meaning we can rearrange their order without altering the final result. For example, we can perform the join between tables S and T first and then join the result with table R, or vice versa—joining R and S first and then integrating T. Additionally, the order of the tables in a join operation doesn't matter: R join S is equivalent to S join R. These properties give us the flexibility to experiment with different plans, and the impact on query performance can be significant. The choice of join order and implementation can lead to substantial differences in plan cost, underscoring the importance of optimizing these decisions to achieve optimal query execution.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150042968.png" alt="image-20240420150042968" style="zoom:50%;" /> 

Depend on the type of the predicate:

| Condition                              | Reduction Factor (RF) Calculation                   |
| -------------------------------------- | --------------------------------------------------- |
| `Col = value`                          | `RF = 1/NKeys(Col)`                                 |
| `Col > value`                          | `RF = (High(Col) - value) / (High(Col) - Low(Col))` |
| `Col < value`                          | `RF = (val - Low(Col)) / (High(Col) - Low(Col))`    |
| `Col_A = Col_B (for joins)`            | `RF = 1/(Max(NKeys(Col_A), NKeys(Col_B)))`          |
| No information about Nkeys or interval | `RF = 1/10` (use a “magic number”)                  |

```
要优化查询，必须计算缩减因子，而缩减因子因应用于数据库列的条件类型而异。我们将这些条件分为几种类型，每种类型都会对结果大小产生不同的影响。第一种是相等条件，即列等于一个特定值，如 "评分等于 8"。另一种是比较条件，如 "年龄大于 50 "或 "年龄小于 50"。连接操作经常使用 "水手 ID 等于储备 ID "这样的条件。最后，在某些情况下，数据库缺乏谓词的特定元数据。在这种情况下，我们会使用一个通用的估计值，即 "1 大于 n"，在没有精确数据的情况下提供一个粗略但必要的近似值。
To optimize queries, it's essential to calculate the reduction factor, which varies depending on the type of condition applied to the database columns. We categorize these conditions into several types, each affecting the result size differently. The first type is the equality condition, where a column equals a specific value, such as 'rating equals 8'. Another type involves comparisons, such as 'age greater than 50' or 'age less than 50'. Join operations often use conditions like 'sailor ID equals reserve ID'. Lastly, there are situations where the database lacks specific metadata for a predicate. In such cases, a generic estimate, termed 'one over n', is used, providing a rough but necessary approximation when precise data is unavailable. 
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150042968.png" alt="image-20240420150042968" style="zoom:50%;" /> 

Depend on the type of the predicate:

| Condition                              | Reduction Factor (RF) Calculation                   |
| -------------------------------------- | --------------------------------------------------- |
| `Col = value`                          | `RF = 1/NKeys(Col)`                                 |
| `Col > value`                          | `RF = (High(Col) - value) / (High(Col) - Low(Col))` |
| `Col < value`                          | `RF = (val - Low(Col)) / (High(Col) - Low(Col))`    |
| `Col_A = Col_B (for joins)`            | `RF = 1/(Max(NKeys(Col_A), NKeys(Col_B)))`          |
| No information about Nkeys or interval | `RF = 1/10` (use a “magic number”)                  |

```
让我们深入探讨一下每种类型的条件是如何计算缩减因子的。对于相等条件（"列等于值"），缩减因子的计算公式是 1 除以不同键的个数，后者代表列中的唯一值。对于范围条件（如 "列大于值"），缩减因子是根据满足条件的值范围的比例得出的。当涉及到连接（"列 A 等于列 B"）时，系数是连接中任一列中不同键最大值的 1 倍。这些计算有助于精确估算每个条件对查询结果大小的影响。为进一步提供帮助，我将提供一份包含所有必要公式的小抄，无论您是否记住公式，都能轻松计算这些因子。
 Let’s delve deeper into how these reduction factors are calculated for each type of condition. For an equality condition ('column equals value'), the reduction factor is calculated as one divided by the number of distinct keys, which represents the unique values within a column. For range conditions, such as 'column greater than value', the reduction factor is derived from the proportion of the value range that meets the condition. When it comes to joins ('column A equals column B'), the factor is one over the maximum of the distinct keys in either column involved in the join. These calculations help in precisely estimating the impact of each condition on the result size of a query. To assist further, I will provide a cheat sheet with all the necessary formulas, enabling you to compute these factors easily, whether you memorize the formulas or not.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150042968.png" alt="image-20240420150042968" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420221311039.png" alt="image-20240420221311039" style="zoom:50%;" /> 

```
让我们考虑一下排名属性的质量，其中可能的值域范围从一到十。如果我们要寻找一个完全匹配的值，比如排名等于某个特定值，那么我们基本上就会把搜索范围缩小到域中 10 个可能值中的一个值。因此，这里的缩减系数是排名中不同关键字数量的 1 倍，即 1/10。这个缩减系数反映了一个基本比例，即在十个可能值中，我们只对一个特定值感兴趣。
Let's consider the quality of the ranking attribute, where the domain of possible values ranges from one to ten. If we're looking for an exact match, such as ranking equal to a specific value, we're essentially narrowing down our search to just one value out of the ten possible values in the domain. Therefore, the reduction factor here is one over the number of distinct keys in the ranking, which is 1/10. This reduction factor reflects the basic proportion that out of the ten possible values, we're interested in only one specific value.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150042968.png" alt="image-20240420150042968" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420221649974.png" alt="image-20240420221649974" style="zoom:50%;" /> 

```
举个例子，我们的条件涉及一个数值范围。举例说明，假设我们想选择一列大于某个值的行。在目录中，我们存储了可能值域的最低值和最高值。当我们遇到这种情况时，我们感兴趣的基本上是整个范围的某个比例。例如，如果我们要查找大于某个特定值的值，那么我们的比例就是（高值-特定值）除以（高值-低值）。这个比例代表我们感兴趣的范围的一部分。同样，如果条件是数值低于某个特定值，那么比例就是（特定值 - 低值）除以（高值 - 低值）。
Let's consider an example where our condition involves a range of values. To illustrate, let's assume we're interested in selecting rows where a column is greater than a certain value. In our catalog, we store the lowest and highest values for the domain of possible values. When we encounter this condition, we're essentially interested in a proportion of the entire range. For example, if we're looking for values greater than a specific value, our proportion would be (high value - specific value) divided by (high value - low value). This ratio represents the portion of the range that we're interested in. Similarly, if the condition is for values lower than a certain value, the proportion would be (specific value - low value) divided by (high value - low value).
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150042968.png" alt="image-20240420150042968" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420221649974.png" alt="image-20240420221649974" style="zoom:50%;" /> 

```
此外，我们还可以将这一概念扩展到任何范围条件，例如在两个给定值之间的特定范围内选择值。在这种情况下，相关比例就是（值 1 - 值 2）除以（高值 - 低值）。通过这些简单明了的数学计算，我们可以推导出精确反映各种范围条件下相关比例的公式，从而帮助优化查询。
Moreover, we can extend this concept to encompass any range condition, such as selecting values within a specific range between two given values. In this case, the proportion of interest would be (value1 - value2) divided by (high value - low value). These straightforward mathematical calculations allow us to derive formulas that accurately reflect the proportion of interest for various range conditions, aiding in query optimization.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150324213.png" alt="image-20240420150324213" style="zoom:50%;" /> 

**1️⃣**Sailors (S): 

NTuples(S) =1000, Nkeys(rating) = 10 interval [1-10],

age interval [0-100], Nkeys(sid)=1000

```sql
SELECT * FROM Sailors WHERE rating = 3 AND age > 50;
```

**2️⃣**Calculate result size:

```
让我们用一个简单的查询来实践一下新发现的知识。假设我们正在查询 "水手 "表中的数据，其中有两个谓词：等级等于 3 和年龄大于 50。以下是我们从系统目录中获得的信息："水手 "表中的图元数为 1000，"等级 "的不同键数在 1 到 10 的区间内为 10，"水手 ID "的键数为 1000。现在的问题是：如何计算结果大小？在揭示解决方案之前，我强烈建议大家先暂停一下，尝试自己解决这个问题。这种主动参与对于学习和保留我们所讲的概念至关重要。
Let's put our newfound knowledge into practice with a simple query. Imagine we're querying data from the 'sailors' table with two predicates: rating equal to three and age greater than 50. Here's the information we have from the system catalog: the number of tuples in the 'sailors' table is 1000, the number of distinct keys for 'rating' is 10 within the interval of one to 10, and the number of keys for 'sailor ID' is 1000. Now, the question is: how do we calculate the result size? Before I reveal the solutions, I strongly encourage you to pause here and attempt to solve it on your own first. This active engagement is crucial for learning and retaining the concepts we've covered.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$


<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420150324213.png" alt="image-20240420150324213" style="zoom:50%;" /> 

**1️⃣**Sailors (S): 

NTuples(S) =1000, Nkeys(rating) = 10 interval [1-10],

age interval [0-100], Nkeys(sid)=1000

```sql
SELECT * FROM Sailors WHERE rating = 3 AND age > 50;
```

**2️⃣**Calculate result size:

```
NTuples(S) = 1000
RF(rating) = 1/10 = 0.1
RF(age) = (100-50)/(100-0) = 0.5
ResultSize = NTuples(S)*RF(rating)*RF(age)
           = 1000*0.1*0.5= 50 tuples
```

```
好了，让我们来分析一下查询结果大小的计算方法。我们有一个包含两个谓词的单表选择：评分等于 3 和年龄大于 50。首先，我们计算 "评分 "的缩减因子。由于 "评分 "有 10 个不同的值，因此缩减因子是 10 减 1，等于 0.1 或 10%。这意味着我们关注的是 10%的可能值。接下来，我们计算 "年龄 "的折减系数。年龄大于 50 岁 "这一条件覆盖了 0 到 100 之间的一半区间，因此缩减因子为 0.5 或 50%。为了计算结果大小，我们将 "水手 "表中的图元数（1000 个图元）乘以每个谓词的缩减因子： 1000 * 0.1 * 0.5 = 50 个图元。这一逻辑适用于任何类似情况。按照这些公式，并考虑到不同键的数量和涉及的范围，我们就可以准确地确定任何查询的结果大小，无论它是否涉及连接、选择或其他操作。
Alright, let's break down the calculation of the result size for our query. We have a single table selection with two predicates: rating equal to three and age greater than 50. First, we calculate the reduction factor over 'rating'. Since 'rating' has 10 distinct values, the reduction factor is 1 over 10, which equals 0.1 or 10%. This means we're interested in 10% of the possible values. Next, we calculate the reduction factor over 'age'. The condition 'age greater than 50' covers half of the interval between 0 and 100, so the reduction factor is 0.5 or 50%. To find the result size, we multiply the number of tuples in the 'sailors' table (1000 tuples) by the reduction factors for each predicate: 1000 * 0.1 * 0.5 = 50 tuples. This logic applies to any similar case. By following these formulas and considering the number of distinct keys and the ranges involved, we can accurately determine the result size for any query, whether it involves joins, selections, or other operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$



<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420151611706.png" alt="image-20240420151611706" style="zoom:50%;" /> 

**1️⃣**What is query optimization/describe steps?

**2️⃣**Equivalence classes

**3️⃣**Result size estimation

**4️⃣**Important for Assignment 3 as well

```
因此，在本讲座中，最重要的是掌握这一过程的本质。你需要彻底理解每个步骤，了解每个阶段的内容，尤其是我们如何利用等价关系得出结论，以及我们利用等价关系的方法。确定等价关系--什么与什么等价--可以成为一道可靠的考题。当然，我们还将深入探讨结果大小的计算，这是在后续课程中占有重要地位的一个关键环节。这项技能还将在作业树中发挥重要作用，在作业树中，我们将扮演数据库管理员的角色，仔细评估我们的综合计划。感谢您的关注，我期待着我们的下次会面。
So, in this lecture, what's important to grasp is the essence of the process. You'll need to comprehend each step thoroughly, understanding what unfolds at each stage, particularly how we arrive at conclusions using equivalences, and the method we employ to leverage them. Identifying equivalences—what is equivalent to what—can make for a solid exam question. Naturally, we'll delve into calculating result sizes, a crucial aspect that will 🚗ry weight in subsequent sessions. This skill will also play a significant role in the assignment tree, where we take on the role of a database administrator, meticulously evaluating our comprehensive plans. Thank you for your attention, and I look forward to our next meeting.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420151911591.png" alt="image-20240420151911591" style="zoom:50%;" /> 
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
