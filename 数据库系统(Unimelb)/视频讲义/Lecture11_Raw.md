<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204519226.png" alt="image-20240403204519226" style="zoom: 33%;" /> 

```
大家好，欢迎回到数据库系统第 11 讲。今天，我们将深入探讨查询处理这一主题。我想给大家提个醒：本讲座可能是本系列中最具挑战性的一讲。我们将介绍各种新术语和一些公式。不过，我鼓励大家保持投入和耐心。在接下来的几讲中，我们将继续练习和解读这些概念，确保你能彻底熟悉它们。
Hello, everyone, and welcome back to our 11th lecture on database systems. Today, we'll delve deeper into the topic of query processing. I want to give you a heads-up: this lecture might be the most challenging one in the series. We're going to introduce a variety of new terms and some formulas. However, I encourage you to stay engaged and patient. Over the next few lectures, we'll continue to practice and unpack these concepts to ensure that you become thoroughly acquainted with them.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204228406.png" alt="image-20240403204228406" style="zoom: 33%;" /> 

```
在上一课中，我们讨论了数据库系统的组成部分，并强调查询处理模块是一个关键要素。该模块在决定如何以最有效的方式执行我们输入的查询方面起着关键作用。我们将在整个课程中探讨该模块中的多个组件。不过，今天我们的重点将放在执行器上。执行器是决定如何执行查询的每个步骤的关键组件。
In our last session, we discussed the components of database systems, highlighting the query processing module as a crucial element. This module plays a pivotal role in determining how a query we input is executed in the most efficient manner possible. We'll be exploring several components within this module throughout our course. Today, however, our focus will be on the executor. The executor is a critical component that dictates how each individual step of a query is executed.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204704994.png" alt="image-20240403204704994" style="zoom:33%;" /> 

```
首先，我将简要介绍我们输入查询时发生的情况。这个过程包括将我们的查询转化为查询计划。查询计划可以看作是一个蓝图或一系列步骤，概述了如何检索我们的数据。然后，我们将从算法的角度深入探讨如何执行该计划的每一步。在今天的讲座中，我们将重点讨论选择和预测。在下一课中，我们将专门讨论连接。如果你发现自己需要复习或额外的支持，我建议你参考教科书中的第 12 章和第 14 章。
First, I'll provide a brief overview of what happens when we input a query. This process involves transforming our query into something known as a query plan. A query plan can be thought of as a blueprint or a sequence of steps that outlines how to retrieve our data. We'll then delve into how each step of this plan can be executed from an algorithmic standpoint. For today's lecture, we will concentrate on selections and projections. In our next session, we will focus exclusively on joins. Should you find yourself needing a refresher or additional support, I encourage you to refer back to chapters 12 and 14 in our textbook.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403221722170.png" alt="image-20240403221722170" style="zoom:33%;" /> 

•Some database operations are EXPENSIVE

•DBMSs can greatly improve performance by being ‘smart’
– e.g., can speed up 1,000,000x over naïve approach

• Main weapons are:

1. clever implementation techniques for operators
2. exploiting ‘equivalencies’of relational operators
3. using cost models to choose among alternatives

```
了解查询处理和执行至关重要，这主要是因为数据库操作本身成本高昂。虽然访问单个元组的成本可能很低，但数据库交互通常涉及大量数据。一个稍微次优的操作，如果扩展到成百上千条记录，就会导致效率大幅下降。重要的是要记住，在庞大的数据集上进行操作的总成本可能非常可观。因此，即使是看似微小的低效率，也可能导致查询需要数天才能完成。要确保性能不仅是足够的，而且是最佳的，优化每项操作至关重要。这种优化的影响是巨大的，通常能将性能提高 100 万倍或更多。这种提升可能意味着将查询执行时间从数天缩短到几毫秒，这无疑是我们的目标。毕竟，没有人愿意为查询结果等待很长时间。
Understanding query processing and execution is crucial, primarily because database operations are inherently costly. While accessing a single tuple might be inexpensive, database interactions typically involve large volumes of data. A marginally suboptimal operation, when scaled to hundreds or thousands of records, can lead to a significant decrease in efficiency. It's vital to remember that the aggregate cost of operations over vast datasets can be substantial. Therefore, even a seemingly minor inefficiency can result in queries that take days to complete. Optimization of every operation is paramount to ensure that the performance is not just adequate but optimal. The impact of this optimization is dramatic—often improving performance by a factor of a million or more. This enhancement could mean reducing query execution time from days to mere milliseconds, which is, undeniably, our objective. After all, no one wants to wait an age for a query to yield results.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403221722170.png" alt="image-20240403221722170" style="zoom:33%;" /> 

•Some database operations are EXPENSIVE

•DBMSs can greatly improve performance by being ‘smart’
– e.g., can speed up 1,000,000x over naïve approach

• Main weapons are:

1. clever implementation techniques for operators
2. exploiting ‘equivalencies’of relational operators
3. using cost models to choose among alternatives

```
那么，数据库是如何实现如此高水平的查询优化的呢？有几种关键 "武器 "可供使用。首先是应用各种关系代数运算符的复杂实现技术。我们将探讨这些技术，并了解关系代数如何支持我们识别等价但更高效的查询计划。
So, how do databases achieve such levels of query optimization? There are a few key 'weapons' at their disposal. The first is the application of sophisticated implementation techniques for various relational algebra operators. We'll explore these techniques and see how relational algebra underpins our ability to identify equivalent, yet more efficient, query plans.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403221722170.png" alt="image-20240403221722170" style="zoom:33%;" /> 

**1️⃣**Some database operations are EXPENSIVE

**2️⃣**DBMSs can greatly improve performance by being ‘smart’

- e.g., can speed up 1,000,000x over naïve approach

**3️⃣**Main weapons are:

1. clever implementation techniques for operators
2. exploiting ‘equivalencies’of relational operators
3. using cost models to choose among alternatives

```
最后，数据库利用成本模型在无数可能的查询计划中进行导航。通过评估不同方法的成本，数据库可以选择最有效的执行路径。这不仅仅是要找到可行的解决方案，而是要在资源利用率和时间效率方面找到最佳方案。所选择的计划往往能以最低的成本获得最佳的性能，从而将原本枯燥冗长的流程转变为快速简化的操作。
Finally, databases utilize cost models to navigate through the myriad of possible query plans. By assessing the cost of different approaches, databases can select the most efficient execution path. This is not just about finding a workable solution; it's about finding the best possible one in terms of resource utilization and time efficiency. The selected plan is often the one that promises the best performance at the lowest cost, turning what could be an exhaustive and lengthy process into a swift and streamlined operation.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204838309.png" alt="image-20240403204838309" style="zoom: 33%;" /> 

```
当我们在平台上输入命令时，SQL 查询之旅的第一阶段就开始了，它将直接进入查询解析器的怀抱。与其他进行语法检查的编程语言一样，查询解析器也会仔细检查我们的查询，以确认其正确性。如果出现任何错别字或混乱，比如 "SELECT FROM WHERE "序列出错，解析器就会立即报告语法错误。此外，这一阶段并不局限于单纯的解析；数据库引擎还可能重写我们的查询以提高性能。还记得我们讨论过的 "IN "和 "NOT IN "子句以及连接的替代用法吗？虽然您可能会使用 "IN "来编写查询，但解析器可以智能地使用连接来重写查询，从而提高执行效率。
The first stage in our SQL query's journey begins when we input our command on the platform, leading it directly into the arms of the query parser. Much like other programming languages that conduct syntax checks, the query parser scrutinizes our query to confirm its correctness. Should there be any typos or disorder—like getting the sequence 'SELECT FROM WHERE' wrong—the parser will promptly report a syntax error. Furthermore, this stage isn't limited to mere parsing; the database engine may also rewrite our queries to enhance performance. Recall our discussion about 'IN' and 'NOT IN' clauses, and the alternative use of joins? Although you might write a query using 'IN', the parser could intelligently rewrite it using a join for more efficient execution.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204838309.png" alt="image-20240403204838309" style="zoom: 33%;" /> 

```
经过解析和可能的重写后，修改后的查询将进入查询优化器--基本上就是数据库的大脑，在这里做出关键决策。优化器的任务是检查执行查询的所有可行计划，将查询计划视为一系列步骤，或者说是关系运算符的堆叠，以形成一个完整的策略。我们将在接下来的讲座中深入探讨这些计划的具体细节。优化器使用关系代数原理来评估备选计划，并通过成本估算来确定最经济的方法。优化器选出最优计划后，就会将其转入最后的执行阶段。
Subsequent to parsing and potential rewriting, the modified query advances to the query optimizer—essentially the database's brain, where crucial decisions are made. The optimizer's task is to examine all feasible plans for executing the query, envisioning a query plan as a series of steps or, if you will, a stack of relational operators assembled to form a complete strategy. We will delve deeper into the specifics of these plans in upcoming lectures. The optimizer uses the principles of relational algebra to evaluate alternative plans and employs cost estimates to determine the most economical approach. Once the optimizer elects the optimal plan, it forwards it to the final phase of execution.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403204838309.png" alt="image-20240403204838309" style="zoom: 33%;" /> 

```
选定的查询计划将交给所谓的查询计划评估器或执行器--我将交替使用这些术语，以便您熟悉不同文本中的不同术语。这个执行器负责执行计划，以最有效的方式执行每个步骤和关系运算符--无论是选择、投影还是连接。在今天的讲座中，我们将从执行的角度阐明 "最佳方式 "的含义，确保我们理解数据库如何将我们的查询转化为既迅速又精确的操作。
The chosen query plan is handed off to what's known as the query plan evaluator, or executor—terms I'll use interchangeably to acquaint you with the varying nomenclature found across different texts. This executor is responsible for 🚗rying out the plan, executing each step and relational operator—be it selection, projection, or join—in the most effective manner. In today's lecture, we'll clarify what "the best possible way" entails from an execution standpoint, ensuring that we understand how a database translates our queries into actions that are both swift and precise.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403222424752.png" alt="image-20240403222424752" style="zoom: 33%;" /> 

**1️⃣**We will consider how to implement:

1. Selection(σ) Selects a subset of rows from relation

2. Projection(π) Deletes unwanted columns from relation

3. Join($\bowtie$​) Allows us to combine two relations

**2️⃣**Operators can be then be composed creating query plans

```
在本课程中，我们将重点讨论如何高效地实现三种基本数据库操作：选择、投影和连接。虽然还有其他操作（如聚合），但我们集中讨论的这三个操作不仅是最常见的，也是最耗费资源的。在这些领域中，优化可以产生重大影响，因此也吸引了大量关注。我们将探讨这些操作符的执行策略。请记住，这些是我们将以各种方式组合起来构建综合查询计划的构件。
In this course, we're going to focus on the efficient implementation of three fundamental database operations: selection, projection, and join. While there are additional operators, like aggregation, the three we are concentrating on are not only the most common, but also the most resource-intensive. These are the areas where optimization can have a significant impact, and consequently, they attract considerable attention. We will explore the execution strategies for these operators. Remember, these are the building blocks that we will combine in various ways to construct a comprehensive query plan.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403205202228.png" alt="image-20240403205202228" style="zoom:33%;" /> 

```
对于那些希望深入研究选择的人来说，我们的数据库管理教科书第 14 章是一个很好的资源。值得一提的是，这本教科书并不是本课程的必修课。它所涵盖的主题比我们当前学习所需的内容更加详细。尽管如此，如果您的好奇心被激发出来，或者您对这些主题有浓厚的兴趣并希望加深理解，我强烈建议您参考这些章节。它们能让你更透彻地理解我们正在讨论的材料。
For those of you looking to delve deeper into selections, Chapter 14 of our database management textbook is an excellent resource. It's worth mentioning, though, that the textbook isn't compulsory for this course. It does cover topics in greater detail than what we'll need for our current studies. Nonetheless, if your curiosity is piqued or if you have a keen interest in these subjects and wish to broaden your understanding, I highly encourage you to consult these chapters. They can provide you with a more thorough comprehension of the material we're discussing.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403222903025.png" alt="image-20240403222903025" style="zoom:33%;" /> 

Sailors (sid: integer, sname: string, rating: integer, age: real)
Reserves (sid: integer, bid: integer, day: dates, rname: string)

**1️⃣**Sailors (S):

1. Each tuple is 50 bytes long, 80 tuples per page, 500 pages
2. N = NPages(S) = 500, $p_S$​=NTuplesPerPage(S) = 80
3. NTuples(S) = 500*80 = 40000

**2️⃣**Reserves (R):

1. Each tuple is 40 bytes long, 100 tuples per page, 1000 pages
2. M= NPages(R) = 1000, $p_R$​=NTuplesPerPage(R) =100
3. NTuples(R) = 100000

```
在本讲座中，我们将重温我们熟悉的 "水手 "和 "储备 "示例。为了强化我们的示例，让我们考虑一些具体细节："水手 "表每页包含 80 个图元，共 500 页。这相当于在 "水手 "关系中包含了 40,000 个图元。同时，"储备 "表每页包含 100 个图元，分布在 1,000 页中，总计 100,000 个图元。在我们进一步探索数据库示例时，这些数字将非常有用。
During this lecture, we'll revisit our familiar examples involving 'sailors' and 'reserves'. To enhance our example, let's consider some specific details: the 'sailors' table contains 80 tuples per page, across a total of 500 pages. This amounts to 40,000 tuples within the 'sailors' relation. Meanwhile, the 'reserves' table holds 100 tuples per page, spread over 1,000 pages, which sums up to 100,000 tuples in total. These figures will be instrumental as we explore our database examples further.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403225646438.png" alt="image-20240403225646438" style="zoom:33%;" /> 

**1️⃣**Of the form: $\sigma{_\text{R.attr op value}}(R)$

**2️⃣**E.g. 

```sql
SELECT * FROM Reserves R WHERE R.BID > 20;
```

**3️⃣**The best way to perform a selection depends on:

1. available indexes/access paths
2. expected size of the result (number of tuples and/or number of pages)

```
关系代数中的简单选择用希腊符号 Sigma (σ) 表示，后面是应用于表的条件。在 SQL 中，我们使用 SELECT 语句来阐述这一概念，该语句用于指定表中的列，同时使用 WHERE 子句来根据特定条件过滤结果。执行选择操作的效率取决于可用的索引或访问路径--它们可以是堆文件、排序文件或各种索引结构，如聚簇、非聚簇、散列或基于树的索引。
Simple selections in relational algebra are denoted by the Greek symbol Sigma (σ), followed by a condition applied to a table. In SQL, we articulate this concept using the SELECT statement to specify columns from a table, accompanied by a WHERE clause to filter the results based on a certain condition. The efficiency of executing a selection operation hinges on the available indexes or access paths—these could be heap files, sorted files, or various index structures like clustered, non-clustered, hash, or tree-based indexes.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403225646438.png" alt="image-20240403225646438" style="zoom:33%;" /> 

**1️⃣**Of the form: $\sigma{_\text{R.attr op value}}(R)$

**2️⃣**E.g. 

```sql
SELECT * FROM Reserves R WHERE R.BID > 20;
```

**3️⃣**The best way to perform a selection depends on:

1. available indexes/access paths
2. expected size of the result (number of tuples and/or number of pages)

```
执行选择的最佳策略不仅取决于可用的索引，还取决于结果的预期大小，即查询将返回的元组数量。这种预期大小会影响操作的成本，因为访问单个图元与访问数百或数百万个图元所产生的费用大不相同。这就是一个类似于鸡和蛋的两难问题：理想情况下，如果我们事先知道结果的大小，就可以选择最有效的执行方法。然而，这些信息只有在查询运行后才会显示出来，对于优化来说为时已晚。为了解决这个问题，数据库使用估算技术来近似估计结果大小和每个操作的相关成本。在本讲座和接下来的讲座中，我们将探讨这些估算如何指导选择查询的最佳执行策略。
The optimal strategy for executing a selection is not only determined by the indexes available but also by the expected size of the result—essentially, the number of tuples the query will return. This expected size affects the operation's cost, as accessing a single tuple versus hundreds or millions will incur vastly different expenses. Herein lies a dilemma akin to the chicken and egg scenario: ideally, if we knew the result size in advance, we could choose the most efficient execution method. However, this information is only revealed after the query runs, which is too late for optimization. To tackle this, databases use estimation techniques to approximate the result size and the associated cost for each operation. Over the course of this lecture and the following ones, we'll explore how these estimations guide the selection of the best execution strategy for a query.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403230955369.png" alt="image-20240403230955369" style="zoom:33%;" /> 

**1️⃣**Size of result approximated as: 
$$
\text{size of relation * Π(reductionfactors)}
$$
**2️⃣**Reduction factor is usually called **selectivity**. It estimates what portion of the relation will qualify for the given predicate, i.e. satisfy the given condition.

1. This is estimated by the optimizer (will be taught next week)
2. E.g. 30% of records qualify, or 5% of records qualify

```
让我们深入探讨一下如何估算单个表上选择操作的结果大小。我们可以用整个关系的大小乘以所谓的还原因子的乘积来近似估算结果大小。缩减因子也称为选择性因子，用于估算符合给定条件的表的比例。这就为我们提供了一个合理的近似值，即有多少图元将符合条件并包含在最终结果集中。
Let's delve into how we can estimate the result size of a selection operation on a single table. The result size can be approximated by taking the size of the entire relation and multiplying it by the product of what we call reduction factors. Reduction factors, also known as selectivity factors, estimate the fraction of the table that meets a given condition. This gives us a reasonable approximation of how many tuples will qualify and be included in the final result set.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403225646438.png" alt="image-20240403225646438" style="zoom:33%;" /> 

**1️⃣**Of the form: $\sigma{_\text{R.attr op value}}(R)$

**2️⃣**E.g. 

```sql
SELECT * FROM Reserves R WHERE R.BID > 20;
```

**3️⃣**The best way to perform a selection depends on:

1. available indexes/access paths
2. expected size of the result (number of tuples and/or number of pages)

```
在我们的 WHERE 子句中--它代表 SQL 中的一个选择--数据库将估算每个谓词的缩减因子。从根本上说，这就是预测哪部分数据将满足指定条件，从而通过操作输出。这种估计是了解查询将返回多少图元的核心。
In the context of our WHERE clause—which represents a selection in SQL—the database will estimate the reduction factor for each predicate present. This is fundamentally about predicting what portion of the data will satisfy the specified conditions and thus be output by the operation. This estimation is at the core of understanding how many tuples will be returned by the query.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403230955369.png" alt="image-20240403230955369" style="zoom:33%;" /> 

**1️⃣**Size of result approximated as: 
$$
\text{size of relation * Π(reductionfactors)}
$$
**2️⃣**Reduction factor is usually called **selectivity**. It estimates what portion of the relation will qualify for the given predicate, i.e. satisfy the given condition.

1. This is estimated by the optimizer (will be taught next week)
2. E.g. 30% of records qualify, or 5% of records qualify

```
我在这里解释一下估算缩减系数的过程，一旦掌握了窍门，这个过程就会非常直观。今天，我将引导你建立这种直觉，以便你能自如地使用这些概念和公式。让我们以 "水手 "表为例，查找排名为 8 的水手。如果排名属性可以取 1 到 10 之间的任何值，那么就有 10 个可能的排名值。现在，如果我们专门搜索排名为 8 的水手，那么这个谓词的缩减因子就是 1/10，即 0.1，因为在 10 个可能的排名值中，我们只对其中一个感兴趣。这意味着我们估计该选择将返回十分之一的数据。随着研究的深入，我们将对其他谓词应用类似的逻辑。
I'm here to explain the process of estimating reduction factors, which is quite intuitive once you get the hang of it. I'll guide you through developing this intuition today so that you'll be comfortable using these concepts and formulas going forward. Let's take an example from our 'sailors' table where we're looking for sailors with a ranking of eight. If the ranking attribute can take on any value between one and ten, there are ten possible values for ranking. Now, if we're searching specifically for a ranking of eight, the reduction factor for this predicate would be 1/10, or 0.1, because out of the ten possible ranking values, we're only interested in one. This means we estimate that the selection will return a tenth of the data. We'll apply similar logic to other predicates as we progress in our studies.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403232037091.png" alt="image-20240403232037091" style="zoom:33%;" /> 

**1️⃣**With no index, unsorted:

1. Must scan the whole relation, i.e. perform Heap Scan
2. **Cost = Number of Pages of Relation, i.e. NPages(R)**
3. **Example**: Reserves cost(R)= 1000 IO (1000 pages)

```
在数据存储在堆中且没有索引的情况下，选择操作必须扫描整个关系，也称为堆扫描。这是因为，在没有索引和数据未排序的情况下，我们必须遍历每一页，以确保没有潜在的匹配关系被忽略。这种堆扫描的成本相当于关系中的页数，因为每一页都需要检查。
In situations where our data is stored in a heap and we don't have an index, a selection operation necessitates scanning the entire relation, also known as a heap scan. This is because, without an index and with data that is unsorted, we must traverse every page to ensure no potential match is overlooked. The cost of such a heap scan equates to the number of pages in the relation since every page needs to be inspected.
```

**2️⃣**With no index, but file is sorted:

1. cost **= binary search cost + number of pages containing results**
2. **Cost = log2(NPages(R))** + (==RF==*NPages(R))
3. **Example**: Reserves cost(R)= 10 I/O + (==RF==*NPages(R))

```
在没有索引的情况下处理排序数据时，我们可以利用数据的排序特性来执行二进制搜索。这种方法大大缩小了搜索空间，使我们只需关注有可能找到合格记录的部分。这里的成本包括定位初始合格记录的二进制搜索，以及包含后续结果的页数。找到第一个符合条件的元组后，我们就可以高效地遍历已排序的数据，收集其余结果。
When dealing with sorted data in the absence of an index, we can leverage the sorted nature of the data to perform a binary search. This approach significantly narrows down the search space, allowing us to focus only on the part where a qualifying record is likely to be found. The cost here includes the binary search to locate the initial qualifying record plus the number of pages that contain the subsequent results. After finding the first qualifying tuple, we can efficiently traverse through the sorted data to gather the rest of the results.
```

**3️⃣**With an index on selection attribute

1. Use index to find qualifying data entries,
2. Then retrieve corresponding data records
3. Discussed next….

```
在选择属性上建立索引会改变游戏规则。我们使用索引将符合条件的数据项归零，然后从表中获取相应的数据记录。索引选择提供了一种更有针对性的方法，减少了我们需要访问的页面数量。我们将在接下来的讲座中进一步探讨这种情况。
Having an index on a selection attribute changes the game. We use the index to zero in on the qualifying data entries and then fetch the corresponding data records from the table. The indexed selection offers a more targeted approach, reducing the number of pages we need to access. We'll explore this scenario further in the upcoming sections of our lecture.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403232702772.png" alt="image-20240403232702772" style="zoom: 50%;" /> 

```
让我们来看看这样一种情况：我们有一个排序过的数据集，共有 8 页，记录按特定属性（比如年龄）排序。如果我们要搜索年龄大于 20 岁的记录，我们首先要进行二进制搜索，找到包含符合条件记录的第一页。假设搜索结果显示，符合条件的记录是从标有 "20 "的页面开始的。我们就可以确定这部分操作的成本为页数的对数基 2，对于 8 页来说就是 log_2(8)。
Let's examine a scenario where we have a sorted dataset across eight pages, with records ordered by a particular attribute—say, age. If we're searching for records where the age is greater than 20, we'd start with a binary search to locate the first page containing the qualifying records. Suppose the search indicates that records meeting our criteria begin on a page labeled with a '20'. We then determine the cost of this part of the operation as the logarithm base 2 of the number of pages, which for eight pages would be log_2(8).
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403232702772.png" alt="image-20240403232702772" style="zoom: 50%;" /> 

```
一旦我们找到了所需记录开始的页面，我们就可以使用缩减因子来估算预计能检索到的数据量。如果缩减因子表明 25% 的数据（或 0.25 的因子）将满足条件，我们就可以将其乘以总页数，从而得出有多少页将包含结果。在我们的案例中，0.25 乘以 8 页等于 2 页结果。之所以能准确预测，是因为数据已经排序；下面 25% 的记录将是我们想要的。由于排序顺序的原因，值 "20 "不可能出现在这一段之前或之后。数据库使用成本公式来估算执行此类选择操作所需的工作量，就是基于这一原则。
Once we've found the page where our desired records begin, we'll use the reduction factor to estimate the portion of data we expect to retrieve. If our reduction factor suggests that 25% of the data—or a factor of 0.25—will meet the condition, we multiply this by the total number of pages to find out how many pages will contain the results. In our case, 0.25 times eight pages equals two pages of results. This is possible to predict with accuracy because the data is sorted; the following 25% of records will be the ones we want. There's no chance of the value '20' appearing before or after this segment because of the sort order. This principle underlies the cost formula that databases use to estimate the effort required to perform such a selection operation.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403232037091.png" alt="image-20240403232037091" style="zoom:33%;" />

**1️⃣**With no index, unsorted:

1. Must scan the whole relation, i.e. perform Heap Scan
2. **Cost = Number of Pages of Relation, i.e. NPages(R)**
3. **Example**: Reserves cost(R)= 1000 IO (1000 pages)

**2️⃣**With no index, but file is sorted:

1. cost **= binary search cost + number of pages containing results**
2. **Cost = log2(NPages(R))** + (==RF==*NPages(R))
3. **Example**: Reserves cost(R)= 10 I/O + (==RF==*NPages(R))

**3️⃣**With an index on selection attribute

1. Use index to find qualifying data entries,
2. Then retrieve corresponding data records
3. Discussed next….

```
当我们有了索引，流程就会发生很大的变化。我们利用索引迅速找出符合标准的第一条记录。通过索引确定符合条件的记录后，我们再检索堆中存储的相应数据。这一步骤与我们之前关于聚类和非聚类索引的讨论如出一辙，它们在决定我们如何快速高效地访问所需数据方面起着至关重要的作用。
When we have indexes available, the process changes significantly. We use the index to swiftly pinpoint the first record that meets our criteria. After identifying the qualifying records through the index, we then retrieve the corresponding data stored in the heap. This step echoes our previous discussion on clustered and non-clustered indices, which play a pivotal role in determining how quickly and efficiently we can access the desired data.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233144289.png" alt="image-20240403233144289" style="zoom:33%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233208963.png" alt="image-20240403233208963" style="zoom:50%;" /> 

```
以下是对搜索如何使用索引进行操作的快速回顾，可以作为回顾。 我们从索引结构的根开始搜索，然后向下遍历到叶子，在叶子中找到感兴趣的数据条目。 一旦找到，这些条目就会引导我们找到数据页上存储的实际数据。 索引主要有两种类型：聚集索引和非聚集索引。 在聚集索引中，数据条目的顺序与数据记录的物理顺序相对应——两者的排序方式类似。 相反，对于非聚集索引，虽然索引中的数据条目已排序，但此顺序与存储中数据记录的物理顺序不匹配。
Here’s a quick review of how searches operate with indexes, which might serve as a refresher. We initiate the search from the root of the index structure and traverse down to the leaves, where we find the data entries of interest. Once located, these entries lead us to the actual data stored on the data pages. There are two main types of indices: clustered and non-clustered. In a clustered index, the order of the data entries corresponds with the physical order of the data records—both are sorted similarly. Conversely, with non-clustered indices, while the data entries in the index are sorted, this order does not match the physical order of the data records in storage.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233800621.png" alt="image-20240403233800621" style="zoom:33%;" /> 

**1️⃣**Cost depends on the number of qualifying tuples

**2️⃣**Clustering is important when calculating the total cost

**3️⃣**Steps to perform

1. Find qualifying data entries:
   - Go through the index: height typically small, 2-4 I/O in case of B+tree, 1.2 I/Oin case of hash index (negligibleif many records retrieved)
   - Once data entries are reached, go through data entries one by one and lookup corresponding data records (in the data file)
2. Retrieve data records (in the data file)

**4️⃣**Cost:

1. Clusteredindex: **Cost = (NPages(I) +** ==NPages==**(R))*RF**
2. Unclusteredindex: **Cost = (NPages(I) +** ==NTuples==**(R))*RF**

```
使用索引的查询成本取决于索引是否聚集。 搜索过程可以被认为包括两个主要阶段。 最初，我们从根到叶遍历索引树来定位数据条目——这些是指向包含查询结果的元组的指针。 然后，我们从数据文件中获取相应的记录。
The cost of a query utilizing an index depends on whether the index is clustered or not. The search process can be thought of as comprising two main phases. Initially, we traverse the index tree from the root to the leaves to locate the data entries—these are the pointers directing us to the tuples containing our query results. Then, we fetch the corresponding records from the data file.

对于聚集索引，通过考虑索引中的页数（用“I”表示）加上关系中的页数（用“R”表示）来估计成本，然后将总数乘以减少量 因素。 相反，在处理非聚集索引时，成本是通过索引中的页数加上关系中的元组数来计算的 - 每个乘以缩减因子以反映选择性搜索。
For a clustered index, the cost is estimated by considering the number of pages in the index (denoted by 'I') plus the number of pages in the relation (denoted by 'R'), with the total then being multiplied by the reduction factor. In contrast, when dealing with a non-clustered index, the cost is calculated by taking the number of pages in the index plus the number of tuples in the relation—each multiplied by the reduction factor to reflect the selective search.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233144289.png" alt="image-20240403233144289" style="zoom:33%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233208963.png" alt="image-20240403233208963" style="zoom:50%;" />  

```
对于图左侧描述的场景，使用聚集索引，搜索操作的成本通过索引页的数量来估计，并通过缩减因子进行调整。 这说明了数据是有序的：一旦找到第一个相关记录，我们就可以推断后续记录是相邻的并且属于我们的搜索。 同样的原则也适用于数据记录本身，因为它们是与索引一起排序的。 如果我们以搜索 3000 名学生为例，每页包含 1000 条记录，那么我们只需要检查索引中的三页和数据文件中的三页。 我们的搜索非常高效，因为我们确信没有任何结果会超出这个连续范围。
For the scenario depicted on the left side of the diagram, with a clustered index, the cost of a search operation is estimated by the number of index pages, adjusted by the reduction factor. This accounts for the fact that the data is ordered: once the first relevant record is located, we can infer that the subsequent records are adjacent and pertain to our search. The same principle applies to the data records themselves since they are sorted in tandem with the index. If we take the example where we are searching for 3000 students and each page holds a thousand records, we'd only need to examine three pages in the index and three pages in the data file. Our search is efficient because we're certain that no results will lie outside this contiguous range.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233144289.png" alt="image-20240403233144289" style="zoom:33%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403233208963.png" alt="image-20240403233208963" style="zoom:50%;" />  

```
相反，对于非聚集索引，如右侧所示，虽然索引本身已排序，但数据记录并未排序。 我们仍然对索引页的数量应用缩减因子，但对于每个符合条件的元组，我们可能会面临额外的 I/O 操作。 这是因为与这些索引条目对应的记录可能分散在各个页面上。 回想一下我们上一课中的示例，如果我们有 3000 名学生，并且每个学生都可能存储在不同的页面上，那么我们可能需要执行 3000 个单独的 I/O 操作来访问所有必要的记录。
In contrast, with a non-clustered index, as shown on the right side, while the index itself is sorted, the data records are not. We still apply the reduction factor to the number of index pages, but for each qualifying tuple, we may face an additional I/O operation. This is due to the fact that the records corresponding to these index entries could be scattered across various pages. Recalling the example from our previous session, if we have 3000 students and each one is potentially stored on a different page, we may need to perform 3000 separate I/O operations to access all the necessary records.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403234426948.png" alt="image-20240403234426948" style="zoom:33%;" /> 

**1️⃣**Example: Let’s say that 10% of Reserves tuples qualify, and let’s say that index occupies 50 pages

**2️⃣**RF = 10% = 0.1, NPages(I) = 50, NPages(R) = 1000, NTuplesPerPage(R) = 100

**3️⃣**Cost:

1. Clusteredindex:
   - Cost = (NPages(I) + NPages(R))*RF
   - Cost = (50+ 1000)*0.1 = ==105 (I/O)== (Cheapest access path)
2. Unclusteredindex:
   - Cost = (NPages(I) + NTuples(R))*RF
   - Cost = (50+ 100000)*0.1 = ==10005==(I/O)
3. Heap Scan:
   - Cost = NPages(R) = 1000 (I/O)

``` 
以我们的“reserves”表为例，我们将计算与聚集索引和非聚集索引以及堆扫描相关的成本。 假设我们有一个导致缩减系数为 10% 的谓词，并且我们的索引跨越 50 页。 对于聚集索引，成本涉及索引中的页数与关系中的页数之和，然后乘以缩减因子。 此计算产生 105 个 I/O 操作。 对于非聚集索引，成本是通过将索引中的页数与关系中的元组总数相加，再乘以缩减因子来确定的，结果是 10,005 次 I/O 操作。
Using our 'reserves' table as an example, we'll calculate the costs associated with both a clustered and an unclustered index, as well as a heap scan. Assume we have a predicate that results in a reduction factor of 10%, and our index spans 50 pages. For a clustered index, the cost involves the sum of the number of pages in the index and the number of pages in the relation, which is then multiplied by the reduction factor. This calculation yields 105 I/O operations. When it comes to an unclustered index, the cost is determined by adding the number of pages in the index to the total number of tuples in the relation, again multiplied by the reduction factor, resulting in 10,005 I/O operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403234426948.png" alt="image-20240403234426948" style="zoom:33%;" /> 

**1️⃣**Example: Let’s say that 10% of Reserves tuples qualify, and let’s say that index occupies 50 pages

**2️⃣**RF = 10% = 0.1, NPages(I) = 50, NPages(R) = 1000, NTuplesPerPage(R) = 100

**3️⃣**Cost:

1. Clusteredindex:
   - Cost = (NPages(I) + NPages(R))*RF
   - Cost = (50+ 1000)*0.1 = ==105 (I/O)== (Cheapest access path)
2. Unclusteredindex:
   - Cost = (NPages(I) + NTuples(R))*RF
   - Cost = (50+ 100000)*0.1 = ==10005==(I/O)
3. Heap Scan:
   - Cost = NPages(R) = 1000 (I/O)

```
无论我们是否有索引，堆扫描始终是一个可用的选项，其中通过扫描存储在磁盘上的所有页面来访问数据。 这种方法总是有一定的成本，我们将与索引访问的成本进行比较，以确定更经济的选择。 在这种情况下，优化器将选择聚集索引来执行查询，因为它在 105 个 I/O 操作时成本最低，因此是最便宜且最有效的访问路径。
Regardless of whether we have indexes or not, a heap scan is always an available option, where data is accessed by sweeping through all pages stored on disk. This approach will always have a certain cost, against which we compare the cost of indexed accesses to determine the more economical choice. In this instance, the optimizer would select the clustered index for executing the query since it has the lowest cost at 105 I/O operations, thus being the cheapest and most efficient access path.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403234842604.png" alt="image-20240403234842604" style="zoom:33%;" /> 

**1️⃣**Typically queries have multiple predicates (conditions)

**2️⃣****Example**: day<8/9/94 AND rname=‘Paul’AND bid=5 AND sid=3

**3️⃣**B-tree index matches(a combination of) predicates that involve only attributes in a **prefix of the search key**

1. Index on <a, b, c> matches predicates on: (a,b,c), (a,b) and(a)
2. Index on <a, b, c> matchesa=5 AND b=3, but will notused to answer b=3
3. This implies that only reduction factors of predicates that are part of the prefix will be used to determine the cost (they are called matching predicates (or primary conjuncts))

```
在处理查询选择条件时，必须通过评估匹配的谓词数量来评估索引如何优化查询。 查询通常带有多个条件。 例如，查询可以包含有关日期、名称、船 ID 和水手 ID 的谓词。 当索引加速涉及与搜索键前缀中的属性匹配的谓词的查询时，索引特别有用。 回想一下，搜索键是构建索引的一组属性。
When dealing with query selection conditions, it’s essential to assess how an index can optimize the query by evaluating the number of predicates that match. Queries often come with multiple conditions. For example, a query could include predicates on the day, name, boat ID, and sailor ID. An index is particularly useful when it accelerates queries involving predicates that match the attributes in the prefix of the search key. Recall that a search key is a set of attributes on which an index is built.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403234842604.png" alt="image-20240403234842604" style="zoom:33%;" /> 

**1️⃣**Typically queries have multiple predicates (conditions)

**2️⃣****Example**: day<8/9/94 AND rname=‘Paul’AND bid=5 AND sid=3

**3️⃣**B-tree index matches(a combination of) predicates that involve only attributes in a **prefix of the search key**

1. Index on <a, b, c> matches predicates on: (a,b,c), (a,b) and(a)
2. Index on <a, b, c> matchesa=5 AND b=3, but will notused to answer b=3
3. This implies that only reduction factors of predicates that are part of the prefix will be used to determine the cost (they are called matching predicates (or primary conjuncts))

```
如果在属性 A、B 和 C 上创建索引，则它可以有效地支持对 A、B、C 或包含 A 的任意组合进行谓词的查询，因为 A 是前缀的开头。 但是，需要注意的是，<a, b, c> 上的索引不会加快仅查找条件 B = 3 的查询，因为单独的 B 并不构成前缀。 但如果条件包括A = 5 AND B = 3，则该对形成前缀并且可以使用索引。 这些匹配谓词的缩减因子（也称为主连接）将有助于减少和确定查询成本。 这种主要连接词的概念可能会出现在文献或在线资源中的各种术语下，尽管它可能非常抽象。
If an index is created on attributes A, B, and C, it can efficiently support queries with predicates on A, B, C, or any combination of these where A is included, since A is the start of the prefix. However, it's crucial to note that an index on <a, b, c> would not expedite a query seeking only condition B = 3, as B alone does not constitute a prefix. But if the conditions include A = 5 AND B = 3, this pair forms a prefix and the index can be used. The reduction factors for these matching predicates, also known as primary conjuncts, will be instrumental in reducing and determining the cost of the query. This concept of primary conjuncts may appear under various terms in literature or online resources, even though it can be quite abstract.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403210937748.png" alt="image-20240403210937748" style="zoom: 33%;" /> 

```
考虑我们有一个建立在 A 和 B 列上的索引，这意味着在 A 的每组中，数据首先按 A 排序，然后按 B 排序。因此，如果 A 为 1，则 B 的范围可以从 1 到 7、2 到 8，并且 依此类推，遵循索引指定的顺序。 该索引允许我们通过直接跳转到 A 为 3 的条目来高效地执行带有 A=3 等谓词的查询。该索引对于 A=1 AND B>2 等复合条件也很有用。 通过查找 A 为 1 的记录，我们可以扫描排序后的 B 值，以快速找到大于 2 的记录。
Consider we have an index built on columns A and B, meaning the data is ordered firstly by A and then by B within each group of A. So if A is 1, then B could range from 1 to 7, 2 to 8, and so on, following the order dictated by the index. This index allows us to efficiently execute a query with a predicate such as A=3 by directly jumping to the entries where A is 3. The index is also beneficial for a compound condition like A=1 AND B>2. By finding the records where A is 1, we can then scan through the sorted B values to quickly locate those greater than 2.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403210937748.png" alt="image-20240403210937748" style="zoom: 33%;" /> 

```
但是，索引对于仅指定 B=1 的查询没有帮助，因为数据不仅仅按 B 排序。如果我们要在索引中查找 B 为 1 的位置，我们不仅会找到它 位于已排序 B 值的开头，但可能分散在各处，因为主排序键 A 可以具有许多关联的 B 值。
However, the index doesn't assist with a query that only specifies B=1, because the data isn't sorted solely by B. If we were to look for where B is 1 within the index, we’d find it not only at the beginning of the sorted B values but potentially scattered throughout, since A, the primary sort key, can have many associated B values.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403210937748.png" alt="image-20240403210937748" style="zoom: 33%;" /> ```

```
在计算访问数据的成本时，当条件仅适用于 A 时，将使用 A 的缩减因子。如果条件同时包含 A 和 B，则将考虑它们的缩减因子。 但对于仅涉及 B（不是前缀）的条件，索引对降低成本没有贡献。 在查询包含条件A=1、B>3、C=7的场景下，索引辅助定位A、B条件。 对于不包含在索引中的 C，稍后将数据提取到内存中以供进一步处理时执行检查。 因此，成本中考虑了A和B的折减系数，但不考虑C。
In calculating the cost of accessing data, the reduction factor for A is used when the condition is solely on A. If the condition includes both A and B, both their reduction factors are considered. But for a condition solely on B, which isn't a prefix, the index doesn’t contribute to cost reduction. In a scenario where the query includes conditions A=1, B>3, and C=7, the index assists in locating A and B conditions. For C, which is not included in the index, the check is performed later when the data is fetched into memory for further processing. Hence, the reduction factors of A and B are considered in the cost, but not C.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403235426384.png" alt="image-20240403235426384" style="zoom:33%;" /> 

**1️⃣**Find the **cheapest access path**

- An index or file scan with the **least estimated page I/O**

**2️⃣**Retrieve tuples using it

- **Predicates that match** this index reduce the number of tuples retrieved (and impact the cost)

**3️⃣**Apply the predicates that **don’t match** the index (if any) later on

1. These predicates are used to dis🚗d some retrieved tuples, but do not affect number of tuples/pages fetched (nor the total cost)
2. In this case selection over other predicates is said to be done “on-the-fly”

```
当使用多个谓词执行选择时，目标是确定最具成本效益的访问路径。 这可以是索引或完整文件扫描，并且所选方法应该具有最低估计的页面访问次数，这相当于检索元组所需的 I/O 操作。 正如我所解释的，与索引对齐的谓词可以显着减少我们需要获取的元组数量。
When executing a selection with multiple predicates, the goal is to determine the most cost-effective access path. This could be an index or a full file scan, and the chosen method should have the lowest estimated number of page accesses, which equates to I/O operations needed to retrieve the tuples. As I've explained, predicates that align with the index can significantly reduce the number of tuples we need to fetch.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403235426384.png" alt="image-20240403235426384" style="zoom:33%;" /> 

**1️⃣**Find the **cheapest access path**

- An index or file scan with the **least estimated page I/O**

**2️⃣**Retrieve tuples using it

- **Predicates that match** this index reduce the number of tuples retrieved (and impact the cost)

**3️⃣**Apply the predicates that **don’t match** the index (if any) later on

1. These predicates are used to dis🚗d some retrieved tuples, but do not affect number of tuples/pages fetched (nor the total cost)
2. In this case selection over other predicates is said to be done “on-the-fly”

```
在我们通过索引检索数据并将其加载到内存中（想象一下将数据从磁盘移动到内存进行处理的过程）之后，我们就可以应用与索引不匹配的任何谓词。 例如，在我们的场景中，C 上的谓词将在这个阶段进行评估。 此步骤称为“即时”处理，因为它发生在内存中，因此不会增加 I/O 操作成本。 请记住，我们主要根据 I/O 操作来量化成本。
After we retrieve the data via the index and load it into memory—picture the process of moving data from disk to memory for processing—we can then apply any predicates that don't match the index. For instance, in our scenario, the predicate on C would be assessed at this stage. This step is referred to as 'on-the-fly' processing because it occurs in memory and thus does not add to the I/O operation cost. Remember, we quantify cost primarily in terms of I/O operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404000039172.png" alt="image-20240404000039172" style="zoom:33%;" /> 

**1️⃣**Example: day < 8/9/94 AND bid=5 AND sid=3

**2️⃣**A B+ tree index on day can be used;

1. RF = RF(day)
2. Then, bid=5and sid=3 must be checked for each retrieved tuple on the fly

**3️⃣**Similarly, a hash index on <bid, sid> could be used;

1. $\Pi{}RF$​= RF(bid)*RF(sid)
2. Then, day<8/9/94must be checkedon the fly

**4️⃣**How about a B+treeon <rname,day>? (Y/N)

**5️⃣**How about a B+treeon <day, rname>? (Y/N)

**6️⃣**How about a Hash index on <day, rname>? (Y/N)

```
让我们看一些例子，了解如何在面对多个谓词时计算最便宜的访问路径。 假设我们有一个关于“day”属性的 B 树索引。 在这种情况下，我们可以确定索引的缩减因子来计算成本。 缩减系数将纳入我们的指数成本公式中。 “船 ID”和“水手 ID”的谓词将在稍后的动态处理过程中应用，因此它们不会影响初始成本。
Let’s look at some examples of how to calculate the cheapest access path when faced with multiple predicates. Suppose we have a B-tree index on the ‘day’ attribute. In this case, we can determine the index's reduction factor to calculate the cost. The reduction factor will be incorporated into our cost formula for the index. The predicates for ‘boat ID’ and ‘sailor ID’ will be applied afterward, during the on-the-fly processing, so they won't contribute to the initial cost.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404000039172.png" alt="image-20240404000039172" style="zoom:33%;" /> 

**1️⃣**Example: day < 8/9/94 AND bid=5 AND sid=3

**2️⃣**A B+ tree index on day can be used;

1. RF = RF(day)
2. Then, bid=5and sid=3 must be checked for each retrieved tuple on the fly

**3️⃣**Similarly, a hash index on <bid, sid> could be used;

1. $\Pi{}RF$= RF(bid)*RF(sid)
2. Then, day<8/9/94must be checkedon the fly

**4️⃣**How about a B+treeon <rname,day>? (Y/N)

**5️⃣**How about a B+treeon <day, rname>? (Y/N)

**6️⃣**How about a Hash index on <day, rname>? (Y/N)

```
另一方面，如果我们在“船”和“水手 ID”上有哈希索引，我们将使用“船 ID”和“水手 ID”的缩减因子来降低成本。 但是，将动态检查“day”谓词，因为它与哈希索引的条件不匹配。 要变得熟练，必须练习确定哪些谓词可用于降低索引操作的成本。
On the other hand, if we have a hash index on ‘boat’ and ‘sailor ID’, we'll use the reduction factors for both ‘boat ID’ and ‘sailor ID’ to decrease the cost. However, the predicate on ‘day’ will be checked on the fly because it doesn’t match the hash index's criteria. To become proficient, it's essential to practice identifying which predicates can be used to reduce the cost of index operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404000039172.png" alt="image-20240404000039172" style="zoom:33%;" /> 

**1️⃣**Example: day < 8/9/94 AND bid=5 AND sid=3

**2️⃣**A B+ tree index on day can be used;

1. RF = RF(day)
2. Then, bid=5and sid=3 must be checked for each retrieved tuple on the fly

**3️⃣**Similarly, a hash index on <bid, sid> could be used;

1. $\Pi{}RF$= RF(bid)*RF(sid)
2. Then, day<8/9/94must be checkedon the fly

**4️⃣**How about a B+treeon <rname,day>? (Y/N)

**5️⃣**How about a B+treeon <day, rname>? (Y/N)

**6️⃣**How about a Hash index on <day, rname>? (Y/N)

```
将其视为练习：决定是否可以利用这些索引来加快查询执行以及应用哪些缩减因子。 具体来说，仔细查看“day”和“name”的哈希索引。 尽管这些是主要连接的一部分，但哈希索引实际上并不能加速对“天”等范围条件的分析，因为如前所述，哈希索引仅对于相等条件而言是最佳的。
Consider this as a practice exercise: decide whether these indexes can be leveraged to expedite query execution and which reduction factors would apply. Specifically, take a close look at the hash index on ‘day’ and ‘name’. Even though these are part of the primary conjuncts, a hash index doesn’t actually accelerate the analysis for a range condition like ‘day’ because, as discussed previously, hash indexes are optimal for equality conditions only.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403211155291.png" alt="image-20240403211155291" style="zoom:33%;" /> 

```
让我们把注意力转向投影的操作。 在我们深入研究这个概念之前，我想先做一个思考练习。 您可能还记得，投影是从关系中选择相关属性子集并根据关系代数消除结果中任何重复项的过程。 考虑一下我们如何完成这项任务：可以应用哪些技术来从值数组中删除重复项？ 我鼓励您暂停片刻并集思广益。 思考可能的方法并提出一些想法。 这是应用您所学到的知识并增强您对数据库操作的理解的绝佳机会。
Let's turn our attention to the operation of projection. I'd like to present a thought exercise before we delve deeper into the concept. Projection, as you may recall, is the process of selecting a subset of relevant attributes from a relation and, in accordance with relational algebra, eliminating any duplicates in the result. Consider how we might accomplish this task: what techniques can be applied to remove duplicates from an array of values? I encourage you to pause for a moment and brainstorm. Think about possible methods and come back with some ideas. This is an excellent opportunity to apply what you've learned and to enhance your understanding of database operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404000618858.png" alt="image-20240404000618858" style="zoom:33%;" /> 

**1️⃣**Issue with projection is removing duplicates

```sql
SELECT DISTINCT R.sid, R.bid FROM Reserves R
```

**2️⃣**Projection can be done based on hashingor sorting

```
让我们深入研究一下识别数组中重复值的策略。一种有效的方法是对数组进行排序。通过排序，我们可以确保任何重复值都被邻近放置，从而简化检测过程。另外，我们还可以使用哈希函数，这让人想起哈希表和索引背后的机制。这种方法可以确保相同的值或重复数据被分配到同一个数据桶中，从而让我们可以专注于特定的子集，而不是整个数据集来进行重复数据验证。这种技术大大缩小了我们的搜索范围，是数据库管理中常用的一种高效方法，可通过哈希或排序消除重复数据。值得注意的是，当我们希望在 MySQL 中删除重复数据时，必须明确使用DISTINCT子句，因为 MySQL 不会自动为我们删除重复数据。
Let's delve into the strategies for identifying duplicates within an array of values. One effective method is to sort the array. By sorting, we ensure that any duplicates will be positioned adjacently, simplifying the detection process. Alternatively, we can employ a hash function, reminiscent of the mechanisms behind hash tables and indexes. This approach ensures that identical values, or duplicates, are allocated to the same bucket, allowing us to focus on a specific subset rather than the entire dataset for duplicate verification. This technique significantly reduces the scope of our search, making it an efficient method commonly utilized in database management to eliminate duplicates through either hashing or sorting. It's important to note that when we wish to remove duplicates in MySQL, for instance, we must explicitly use the DISTINCT clause, as MySQL does not automatically remove duplicates for us.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210223720.png" alt="image-20240404210223720" style="zoom: 33%;" /> 

**1️⃣**Basic approach is to use sorting

1. Scan R, extract only the neededattributes

2. Sort the result set (typically using external merge sort)

3. Remove adjacentduplicates

   <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210401602.png" alt="image-20240404210401602" style="zoom:50%;" /> 

```
要执行基于排序的投影操作，我们首先要对关系进行扫描，分离出感兴趣的属性，这是投影的本质。在分离出这些属性后，我们的下一步就是对它们进行排序，重点是检测任何相邻的重复数据。例如，假设我们根据年龄和薪水这两个属性对数据进行排序。数据排序完成后，我们的任务就是检查相邻条目是否存在重复。这个过程虽然简单明了，但却能确保如果存在重复项，它们将紧紧相邻。
To perform a projection operation based on sorting, we first undertake a scan of the relation to isolate the attributes of interest, which is the essence of projection. After isolating these attributes, our next step is to sort them, focusing specifically on detecting any adjacent duplicates. For instance, consider a scenario where we are sorting data based on two attributes: age and salary. Once the data is sorted, our task is to examine adjacent entries for duplicates. This process, though straightforward, ensures that if duplicates are present, they will be immediately adjacent.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210223720.png" alt="image-20240404210223720" style="zoom: 33%;" /> 

**1️⃣**Basic approach is to use sorting

1. Scan R, extract only the neededattributes

2. Sort the result set (typically using external merge sort)

3. Remove adjacentduplicates

   <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210401602.png" alt="image-20240404210401602" style="zoom:50%;" /> 

```sq
然而，数据量的巨大带来了严峻的挑战。通常情况下，数据存放在磁盘页面上，而可用于处理的内存却非常有限。由于所有处理都是在内存中进行的，因此关键问题是当数据超过可用内存容量时如何进行排序。这是数据库需要解决的一个常见问题。However, a critical challenge arises due to the volume of data. Typically, data resides on disk pages, whereas the memory available for processing is significantly more limited. Since all processing occurs in memory, the pivotal question is how to perform sorting when the data exceeds available memory capacity. This is a common problem that databases need to address.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210223720.png" alt="image-20240404210223720" style="zoom: 33%;" /> 

**1️⃣**Basic approach is to use sorting

1. Scan R, extract only the neededattributes

2. Sort the result set (typically using external merge sort)

3. Remove adjacentduplicates

   <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404210401602.png" alt="image-20240404210401602" style="zoom:50%;" /> 

```
为了解决这个问题，数据库采用了一种称为外部排序的技术，它是处理超出内存限制的大型数据集的一种巧妙而实用的解决方案。对于那些对算法情有独钟的人来说，外部排序背后的原理相当耐人寻味，值得简单解释一下，以掌握其基本原理。
To manage this, databases employ a technique known as external sorting, which is an ingenious and practical solution for handling large datasets that exceed memory constraints. For those with an affinity for algorithms, the rationale behind external sorting is quite intriguing and merits a brief explanation to grasp its basic principles.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403213709995.png" alt="image-20240403213709995" style="zoom:33%;" /> 

```
在实际操作中，当处理存储在磁盘上无数数据页中的大型数据集时，我们会遇到一个很大的限制：计算机系统中的 RAM 容量有限，而所有处理过程都是在 RAM 中进行的。为了启动排序，我们必须将数据从磁盘传输到内存中。然而，当数据集超过 RAM 的容量时，这就带来了挑战，在这个简化的例子中，RAM 一次只能容纳三页数据。
In practical terms, when dealing with large datasets stored across numerous data pages on a disk, we encounter a significant constraint: the limited size of RAM in our computer systems where all processing occurs. To initiate sorting, we must transfer the data from the disk into memory. However, this poses a challenge when the dataset exceeds the capacity of RAM, which in this simplified example, can only accommodate three pages of data at a time.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403213709995.png" alt="image-20240403213709995" style="zoom:33%;" /> 

```
解决这个问题的办法是将较大的数据集分割成适合内存的较小部分。我们首先将前三页加载到 RAM 中，对其进行分类，然后将其返回磁盘，为下一组页面释放内存。重复这一步骤，一次一批，直到我们遍历整个数据集。在外部排序的情况下，这些遍历被称为 "运行"，每次运行都会产生原始数据集的部分排序段。
The solution to this problem is to segment the larger dataset into smaller portions that fit into memory. We begin by loading the first three pages into RAM, sorting them, and then returning them to the disk to free up memory for the next set of pages. This step is repeated, one batch at a time, until we have traversed the entire dataset. Each of these passes, known in the context of external sorting as a 'run', results in partially sorted segments of the original dataset.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403213709995.png" alt="image-20240403213709995" style="zoom:33%;" /> 

```
在随后的阶段，我们从每个排序段中提取第一页，并将其合并。通过对这些合并的页面进行排序并将其返回磁盘，我们可以确保在这个子集中排序出尽可能小的值。这一迭代过程涉及对数据的多次传递或 "运行"，最终会得到一个完全排序的数据集。通过这种被称为 "排序-合并 "操作的技术，我们将排序任务分解为易于管理的部分，从而巧妙地规避了内存限制。
In the subsequent phase, we take the first page from each sorted segment and merge them. By sorting these combined pages and returning them to the disk, we ensure that we have the smallest possible values sorted within this subset. This iterative process, which involves multiple passes or 'runs' over the data, eventually leads to a fully sorted dataset. Through this technique, known as the sort-merge operation, we cleverly circumvent the memory limitation by breaking down the sorting task into manageable parts.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211110066.png" alt="image-20240404211110066" style="zoom:33%;" /> 

****1️⃣**If data does not fit in memory do several passes**

**2️⃣**Sort runs: Make each B pages sorted (called runs)

**3️⃣**Merge runs: Make multiple passes to merge runs

1. Pass 2: Produce runs of length B(B-1) pages  ==We will let you know==
2. Pass 3: Produce runs of length B(B-1)$^2$​ pages ==how many passes there are==
3. ......................
4. Pass P: Produce runs of length B(B-1)$^P$​pages

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211310984.png" alt="image-20240404211310984" style="zoom:50%;" /> 

```
外部合并排序是一种强大的算法，设计用于对超出系统内存容量的数据进行排序。当内存无法容纳数据时，我们会对其进行多次处理。最初，我们创建的排序段被称为 "运行"。然后有条不紊地合并这些运行段，每次从每个段中合并一页，以获得完全排序的数据集。这个过程涉及多轮合并，数据排序所需的精确次数取决于复杂的数学原理，其中考虑到数据集的大小和可用内存。
The external merge sort is a robust algorithm designed for sorting data that exceeds the capacity of the system's memory. When data cannot be accommodated in RAM, we perform multiple passes over it. Initially, we create sorted segments known as 'runs'. These runs are then methodically merged, one page at a time, from each segment to achieve a fully sorted dataset. The process involves several rounds of merging, and the precise number of passes required to sort the data depends on complex mathematical principles, which take into account the size of the dataset and the available memory.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211110066.png" alt="image-20240404211110066" style="zoom:33%;" /> 

****1️⃣**If data does not fit in memory do several passes**

**2️⃣**Sort runs: Make each B pages sorted (called runs)

**3️⃣**Merge runs: Make multiple passes to merge runs

1. Pass 2: Produce runs of length B(B-1) pages  ==We will let you know==
2. Pass 3: Produce runs of length B(B-1)$^2$​ pages ==how many passes there are==
3. ......................
4. Pass P: Produce runs of length B(B-1)$^P$​pages

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211310984.png" alt="image-20240404211310984" style="zoom:50%;" /> 

```
在本课程中，我们将为您提供必要合格次数背后的复杂数学知识，从而简化这一过程。这意味着您只需将给定的信息应用到规定的公式中即可。对于那些对算法细节有浓厚兴趣的人，教科书第 13 章将对其进行详尽讨论。不过，本课程并不要求您深入了解外部合并排序的机制。我们的重点将放在算法的成本计算方面，这对数据库至关重要。稍后，我将演示计算该算法运行成本所需的具体公式。
For the purposes of this course, the intricate mathematics behind the number of necessary passes will be provided to you, simplifying the process. This means you'll only have to apply the given information into the prescribed formula. For those who have a keen interest in the algorithmic details, they are thoroughly discussed in Chapter 13 of the textbook. However, a deep understanding of the mechanics of external merge sort is not required for this course. Our focus will be on the costing aspect of the algorithm, which is crucial for databases. Shortly, I will demonstrate the specific formula that you will need to use for calculating the cost of running this algorithm.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

![image-20240403214301313](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214301313.png)

```
# buffer pages in memory B = 4, each page 2 records, sorting on a single attribute (just showing the attribute value)
```

```
To grasp the external merge sort, let's consider a small-scale example where our memory can hold four pages, and each page includes two records. We're sorting based on a single attribute, represented here by one attribute value. Despite having a multitude of pages in the input file, our limitation is the four pages that memory can simultaneously accommodate. We start by sorting the first four pages, creating a series of 'sorted runs'. We then continue this process until all the pages are sorted into these runs. In the subsequent phase, we take one page from each run and sort them in memory. Since the runs are already sorted, we know the smallest value is at the forefront, ensuring we're working with positive values. This process repeats: pulling data from the disk, sorting it in memory, and outputting the result, effectively sorting the entire data set through these iterative memory-disk exchanges.
为了理解外部合并排序，让我们考虑一个小规模的例子：我们的内存可以容纳四个页面，每个页面包括两条记录。我们根据单个属性进行排序，这里用一个属性值来表示。尽管输入文件中有许多页面，但我们的限制是内存只能同时容纳四个页面。我们首先对前四页进行排序，创建一系列 "排序运行"。然后，我们继续这个过程，直到所有页面都被排序到这些运行中。在随后的阶段，我们从每个运行中抽取一页，并在内存中进行排序。由于已经对运行进行了排序，我们知道最小值位于最前面，从而确保我们处理的是正值。这个过程不断重复：从磁盘中提取数据，在内存中排序，然后输出结果，通过这些内存-磁盘的迭代交换，有效地对整个数据集进行排序。
```

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214417654.png" alt="image-20240403214417654" style="zoom: 39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214501611.png" alt="image-20240403214501611" style="zoom: 39%;" />  

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214624220.png" alt="image-20240403214624220" style="zoom:39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214645394.png" alt="image-20240403214645394" style="zoom:39%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214719266.png" alt="image-20240403214719266" style="zoom:39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214840734.png" alt="image-20240403214840734" style="zoom:39%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214925145.png" alt="image-20240403214925145" style="zoom: 39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403214944541.png" alt="image-20240403214944541" style="zoom:39%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403215019391.png" alt="image-20240403215019391" style="zoom:39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403215036463.png" alt="image-20240403215036463" style="zoom:39%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403215056351.png" alt="image-20240403215056351" style="zoom:39%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240403215115209.png" alt="image-20240403215115209" style="zoom:39%;" /> 

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211857988.png" alt="image-20240404211857988" style="zoom:33%;" /> 

**1️⃣**Sorting with external sort:

1. Scan R, extract only the needed attributes
2. Sort the result set using EXTERNAL SORT
3. Remove adjacent duplicates

**2️⃣**Cost = 

```sql
ReadTable+              -- Read the entire table and keep only projected attributes
WriteProjectedPages+    -- Write pages with projected attributes to disk
SortingCost+            -- Sort pages with projected attributes with external sort
ReadProjectedPages      -- Read sorted projected pages to dis🚗d adjacent duplicates
```

****3️⃣**WriteProjectedPages**= NPages(R)* PF

****4️⃣**PF: Projection Factor** says how much are we projecting, ratio with respect to all attributes (e.g. keeping ¼ of attributes, or 10% of all attributes)

**5️⃣**$\text{SortingCost= 2*NumPasses*ReadProjectedPages}$​

The 2 represents: Every time we read and write

 ```\
 谈到使用外部合并排序的投影操作，我们的重点转移到了成本评估上。执行投影的成本主要由三个步骤决定。首先，读取数据并提取投影的相关属性。提取后，对属性进行排序。排序完成后的最后一步是删除相邻的重复数据。这些步骤共同定义了通过外部合并排序评估投影成本所涉及的任务。
 Turning to the projection operation using external merge sort, our focus shifts to cost evaluation. The cost of performing a projection is delineated by three primary steps. Initially, data is read and the relevant attributes for the projection are extracted. Following this extraction, the attributes are sorted. The final step, once sorting is complete, involves the removal of adjacent duplicates. These steps collectively define the tasks involved in assessing the cost of a projection via external merge sort.
 ```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211857988.png" alt="image-20240404211857988" style="zoom:33%;" />

**1️⃣**Sorting with external sort:

1. Scan R, extract only the needed attributes
2. Sort the result set using EXTERNAL SORT
3. Remove adjacent duplicates

**2️⃣**Cost = 

```sql
ReadTable+              -- Read the entire table and keep only projected attributes
WriteProjectedPages+    -- Write pages with projected attributes to disk
SortingCost+            -- Sort pages with projected attributes with external sort
ReadProjectedPages      -- Read sorted projected pages to dis🚗d adjacent duplicates
```

****3️⃣**WriteProjectedPages**= NPages(R)* PF

****4️⃣**PF: Projection Factor** says how much are we projecting, ratio with respect to all attributes (e.g. keeping ¼ of attributes, or 10% of all attributes)

**5️⃣**$\text{SortingCost= 2*NumPasses*ReadProjectedPages}$​

The 2 represents: Every time we read and write

```
使用外部合并排序的投影成本包括将整个表从磁盘读取到内存，然后将投影属性写回磁盘的操作。然后再次读取投影页面进行排序，这就需要我们考虑外部排序的成本。
The cost of a projection using external merge sort includes the operations of reading the entire table from disk to memory and then writing the projected attributes back to disk. This is followed by reading the projected pages again for sorting, which requires us to account for the cost of the external sort.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404211857988.png" alt="image-20240404211857988" style="zoom:33%;" />

**1️⃣**Sorting with external sort:

1. Scan R, extract only the needed attributes
2. Sort the result set using EXTERNAL SORT
3. Remove adjacent duplicates

**2️⃣**Cost = 

```sql
ReadTable+              -- Read the entire table and keep only projected attributes
WriteProjectedPages+    -- Write pages with projected attributes to disk
SortingCost+            -- Sort pages with projected attributes with external sort
ReadProjectedPages      -- Read sorted projected pages to dis🚗d adjacent duplicates
```

****3️⃣**WriteProjectedPages**= NPages(R)* PF

****4️⃣**PF: Projection Factor** says how much are we projecting, ratio with respect to all attributes (e.g. keeping ¼ of attributes, or 10% of all attributes)

**5️⃣**$\text{SortingCost= 2*NumPasses*ReadProjectedPages}$​

The 2 represents: Every time we read and write

```
投影因子在将投影页面写回磁盘时发挥作用。它反映了投影后保留的列的比例，类似于缩减因子。例如，25% 的投影系数表示我们保留了四分之一的列。排序的成本由外部排序的成本决定，外部排序的成本是排序次数乘以投影页数的两倍，体现了每次排序所需的读写操作。
The projection factor comes into play when writing projected pages back to disk. It reflects the proportion of columns retained post-projection, similar to a reduction factor. For instance, a 25% projection factor indicates that we're keeping a quarter of the columns. The cost of sorting is encapsulated by the cost of the external sort, which is two times the number of passes multiplied by the number of projected pages, embodying the read and write operations necessary for each pass.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404213412688.png" alt="image-20240404213412688" style="zoom:33%;" /> 

**1️⃣**Example: Let’s say that we project ¼ of all attributes, and let’s say that we have 20 pages in memory

**2️⃣**PF = 1/4 = 0.25, NPages(R) = 1000

**3️⃣**With 20 memory pages we can sort in 2 passes

**4️⃣**Cost =

```
ReadTable+
WriteProjectedPages+
SortingCost+
ReadProjectedPages
= 1000 + 0.25 * 1000 + 2*2*250 + 250 = 2500 (I/O)
```

```
本例演示了如何应用公式计算两次排序操作的成本，重点是数据投影。假设我们有一个在内存中存储了 1000 页的表。为了优化排序，我们投影了四分之一的属性，从而有效地缩小了表的大小。计算过程如下：总成本包括读取整个表的成本，加上写入投影页的成本，加上排序成本，加上读取投影页以消除重复的成本。具体来说，该公式可细分为读表成本（1000 页）、写入投影页的成本（0.25 * 1000 页）以及两次通过次数乘以读取投影页的成本。这一策略只投影了四分之一的属性，从而大大减少了数据集，简化了排序过程，便于最后一步的重复消除。
This example demonstrates the application of a formula to calculate the cost of a two-pass sort operation with a focus on data projection. Assume we have a table stored across 1000 pages in memory. To optimize the sort, we project a quarter of the attributes, effectively reducing the table's size. The calculation proceeds as follows: the total cost comprises the cost of reading the entire table, plus the cost of writing the projected pages, plus the sorting cost, plus the cost of reading the projected pages to eliminate duplicates. Specifically, the formula breaks down into the cost of reading the table (1000 pages), adding the cost of writing the projected pages (0.25 * 1000 pages), and twice the number of passes multiplied by the cost of reading the projected pages. This strategy significantly reduces the dataset by projecting only a quarter of the attributes, thereby streamlining the sorting process and facilitating the final step of duplicate elimination.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404213849558.png" alt="image-20240404213849558" style="zoom:33%;" /> 

**1️⃣**Hashing-based projection

1. Scan R, extract only the neededattributes
2. Hash data into buckets
   - Apply hash function h1to choose one of B output buffers
3. Remove adjacentduplicates from a bucket
   - 2 tuples from different partitions guaranteed to be distinct

```
除了排序，我们还可以使用散列法进行投影，其原理与此类似。这种方法不是进行排序，而是将数据分隔成多个数据桶，然后对这些数据桶进行扫描，找出并删除重复数据。这一过程在成功识别和消除重复数据后结束。
In addition to sorting, we can apply projection using hashing, which operates on a similar principle. Rather than sorting, the method involves segregating the data into buckets and then scanning these buckets to locate and remove duplicates. This process concludes upon the successful identification and elimination of duplicates.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214239521.png" alt="image-20240404214239521" style="zoom:33%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214253121.png" alt="image-20240404214253121" style="zoom:67%;" /> 

```
这种基于散列的投影法需要一次一页地处理一个大表。每一页的数据都通过散列函数定向到相应的数据桶。随后，对这些数据桶进行传递，消除重复数据。不过，这一过程也并非没有挑战，这与排序过程中遇到的挑战如出一辙。具体来说，当输入大小超过内存容量时，数据桶也无法装入可用内存，这就造成了很大的障碍。
This projection, based on hashing, involves processing a large table one page at a time. Each page's data is directed to its corresponding bucket through a hashing function. Subsequently, a pass over these buckets eliminates duplicates. However, the process is not without its challenges, mirroring those encountered in sorting. Specifically, when the input size exceeds memory capacity, the buckets also cannot fit into the available memory, presenting a significant obstacle.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214458947.png" alt="image-20240404214458947" style="zoom:33%;" /> 

**1️⃣**Partitiondata into B partitions with h1 hash function

**2️⃣**Load each partition, hash it with another hash function (h2) and eliminate duplicates

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214527586.png" alt="image-20240404214527586" style="zoom:50%;" /> 

```
当存储桶超过内存容量时，外部散列功能就会发挥作用，允许多次散列。起初，该过程反映了标准方法：读取数据，然后应用散列函数将数据分配到相应的分区中。考虑到存储桶无法完全驻留在内存中的限制，将数据卸载到磁盘成为一种可行的解决方案，从而促进分区的创建。这里的假设是，内存只能容纳每个数据桶的一个页面，这就足够了。分区完成后，下一步是单独重新加载每个分区，并应用新的哈希函数将数据分发到数据桶中，然后进行重复检查。
When buckets exceed memory capacity, external hashing comes into play, allowing for hashing across multiple passes. Initially, the process mirrors the standard approach: data is read, and a hash function is applied to allocate data into corresponding partitions. Given the constraint that a bucket cannot fully reside in memory, offloading to disk becomes a viable solution, facilitating the creation of partitions. The assumption here is that memory can only accommodate one page per bucket, which suffices. After partitioning, the next step involves reloading each partition individually and applying a new hash function to distribute data into buckets, where duplicate checks are then performed.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214654716.png" alt="image-20240404214654716" style="zoom:50%;" /> 

**1️⃣**Partitioning phase:

1. Read R using one input buffer
2. For each tuple:
   - Dis🚗d unwanted fields
   - Apply hash function h1to choose one of B-1 output buffers
3. Result is B-1 partitions (of tuples with no unwanted fields)
   - 2 tuples from different partitions guaranteed to be distinct

**2️⃣**Duplicate elimination phase:

1. For each partition
   - Read it and build an in-memory hash table
     - using hash function h2(<> h1) on all fields
   - while dis🚗ding duplicates
2. If partition does not fit in memory
   - Apply hash-based projection algorithm recursively to this partition (we will not do this…)

```
数据处理过程分为两个主要步骤：首先是分区，将数据分散到各个分区并存储到磁盘上。随后是消除重复阶段，在这一阶段中，每次处理一个分区。采用新的散列函数在内存中创建新的数据桶，便于检查每个相应数据桶中的重复数据。使用新的哈希函数而不是重复使用原来的哈希函数是有战略意义的。使用相同的哈希函数会导致整个分区被重新定位到一个内存桶中，这是一种低效且昂贵的操作。我们的目的是进一步分割数据，因此在此阶段引入了新的散列函数。
The data handling process is divided into two primary steps: initially, partitioning, where data is distributed into partitions and stored on disk. Following this, the duplicate elimination phase occurs, during which one partition is processed at a time. A new hash function is applied to create new buckets in memory, facilitating the inspection of each corresponding bucket for duplicates. The rationale behind employing a new hash function rather than reusing the original one is strategic. Utilizing the same hash function would result in the entire partition being relocated to a single memory bucket, an inefficient and costly operation. The aim is to segment the data further, hence the introduction of a new hash function at this stage.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404215130208.png" alt="image-20240404215130208" style="zoom:33%;" /> 

```
Cost =       
      ReadTable+
      WriteProjectedPages+
      ReadProjectedPages
      
Our example:
Cost = 
      ReadTable+
      WriteProjectedPages+
      ReadProjectedPages
     = 1000 + 0.25 * 1000 + 250 = 1500 (I/O)
      
```

```
对于那些对算法感兴趣的人来说，我已经演示了外部散列，但了解其内部工作原理对我们的目的来说并不重要；请放心，我们不会对此进行审问。不过，最重要的是掌握与这一操作相关的成本--基于散列的推算--这里将概述这一成本。
For those intrigued by the algorithmic aspect, I've demonstrated external hashing, but it's not essential to understand its inner workings for our purposes; rest assured, there will be no interrogation on this. What is crucial, however, is grasping the cost associated with this operation—the projection based on hashing—and that cost is outlined here.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404215130208.png" alt="image-20240404215130208" style="zoom:33%;" /> 

```sql
Cost =       
      ReadTable+            -- Read the entire table and project attributes
      WriteProjectedPages+  -- Write projected pages into corresponding partitions
      ReadProjectedPages    -- Read partitions one by one, create another hash table and dis🚗d duplicates within a bucket
      
Our example:
Cost = 
      ReadTable+
      WriteProjectedPages+
      ReadProjectedPages
     = 1000 + 0.25 * 1000 + 250 = 1500 (I/O)
      
```

```
成本包括读取整个表以预测属性，然后将这些预测页写入相应的分区。首先，我们读取所有数据以创建分区。之后，我们逐个处理分区，生成另一个哈希表，并消除每个桶中的重复数据。具体来说，成本细分如下：读取表的成本为 1000。写入投影页（占 1000 的 25%）产生 250 个投影页。这个过程会建立分区，然后读取这 250 个预计页面构成后续成本，最终总成本为 1500。值得注意的是，尽管执行了大量的逻辑操作（分区和数据操作），但只有读取和写入的直接操作对计算成本有贡献。
The cost involves reading the entire table to project attributes and then writing these projected pages into corresponding partitions. Initially, we read all the data to create partitions. Following this, we process partitions one by one, generate another hash table, and eliminate duplicates within each bucket. Specifically, the cost breakdown is as follows: reading the table incurs a cost of 1000. Writing projected pages, which amounts to 25% of 1000, results in 250 projected pages. This process establishes partitions, and then reading these 250 projected pages constitutes the subsequent cost, culminating in a total cost of 1500. It's important to note, despite the extensive logical operations performed—partitioning and data manipulation—only the direct actions of reading and writing contribute to the calculated cost.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214458947.png" alt="image-20240404214458947" style="zoom:33%;" /> 

**1️⃣**Partitiondata into B partitions with h1 hash function

**2️⃣**Load each partition, hash it with another hash function (h2) and eliminate duplicates

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404214527586.png" alt="image-20240404214527586" style="zoom:50%;" /> 

```
好的,从字面上看，我们要把多少页内容从这里带入我的记忆中，再从记忆中放回这里。
All right. What contributes is literally how many pages we will bring from this into me memory and put it back from memory into this. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404215130208.png" alt="image-20240404215130208" style="zoom:33%;" /> 

```
Cost =       
      ReadTable+
      WriteProjectedPages+
      ReadProjectedPages
      
Our example:
Cost = 
      ReadTable+
      WriteProjectedPages+
      ReadProjectedPages
     = 1000 + 0.25 * 1000 + 250 = 1500 (I/O)
      
```

```
这一点在这些相当简单的公式中得到了真正的体现，在查询处理模块的其余部分中，这些公式也将保持简单。好的。
And this is really reflected in those fairly simple formulas and the formulas are going to remain as simple for the rest of this query processing module. All right.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404220037290.png" alt="image-20240404220037290" style="zoom: 33%;" /> 

**1️⃣**Understand the logic behind relational operators

**2️⃣**Learn alternatives for selections and projections (for now)

- Be able to calculate the cost of alternatives

**3️⃣**Important for Assignment 3 as well

```
今天讲座的主旨是让大家掌握评估各种计划和运营商成本的必要技能。虽然我们探讨了一系列主题，但我们的主要重点是了解如何评估预测和选择过程的效率。在接下来的课程中，我们将深入探讨加盟的复杂性，随后学习计算整个计划的成本，以确定最有效的策略。这一练习模拟了优化人员的角色，反映了数据库管理员在现实世界中负责提高性能和简化分析的职责。这也将是你们第三次作业的主题。感谢您的关注，我期待着我们的下一堂课。
The essence of today's lecture was to equip you with the skills necessary for evaluating the cost of various plans and operators. While we've explored a range of topics, our primary focus has been on understanding how to assess the efficiency of projection and selection processes. In our upcoming sessions, we will delve into the complexities of joins and subsequently learn to calculate the costs of entire plans to identify the most effective strategies. This exercise simulates the role of an optimizer, mirroring the real-world responsibilities of a database administrator tasked with enhancing performance and streamlining analysis. This will also be the theme of your third assignment. Thank you for your attention, and I look forward to our next session.
```





