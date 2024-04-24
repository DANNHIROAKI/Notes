```
Hello, everyone, and welcome back to our 14th lecture on database systems. Today, we'll be concluding our section on query optimization. This will also wrap up the comprehensive module discussing the execution of our queries. I trust that after this, you'll be eager to delve back into the material, as we transition to some lighter topics.
大家好，欢迎回到数据库系统第 14 讲。今天，我们将结束查询优化部分。这也将为我们讨论查询执行的综合模块画上句号。我相信，在这之后，你会迫不及待地重新钻研教材，因为我们将过渡到一些轻松的话题。
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152623708.png" alt="image-20240420152623708" style="zoom:50%;" /> 

This is one of several possible architectures; each system has its own slight variations.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152702871.png" alt="image-20240420152702871" style="zoom: 50%;" /> 

```
让我们先来简单回顾一下：在过去两周里，我们沉浸在查询处理模块中。我们探索了执行的复杂性，深入研究了各种算法以及数据库如何执行特定操作，如连接和选择。上次，我们研究了优化器如何使用缩减因子估算结果的大小。今天，我们将最终揭开哪些计划会被考虑以及它们背后的选择过程。本讲座是一个顶点，综合了我们在前五讲中讨论的所有内容。可以把它看作是一份综合指南，我们将所有关键点整合成一份统一的概述。
Let’s begin with a brief refresher: over the past two weeks, we've immersed ourselves in the query processing module. We've explored the intricacies of execution—diving into various algorithms and how databases 🚗ry out specific operations, like joins and selections. Last time, we examined how the optimizer estimates the size of results using reduction factors. Today, we'll finally uncover which plans are considered and the selection process behind them. This lecture serves as a capstone, synthesizing all the elements we've discussed across the previous five lectures. Think of it as a comprehensive guide, where we consolidate all the key points into a single, cohesive overview.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

**1️⃣**When enumerating alternative plans, there are two main cases:

1. Single-relation plans
2. Multiple-relation plans (joins)

```
在规划查询处理时，我们面对两种主要情况。第一种是单一关系（单表查询）方案，这种情况下我们仅涉及一个表，没有进行任何连接操作。另一种情况，也是更常见的情况，涉及多个关系或者多个表的连接操作，实际上，这种情况占据了大部分查询。针对单一关系的查询方案，我们首先要评估每一种可用的数据访问路径，并选择成本最低的一种。记住，无论是否存在索引，堆扫描（heap scan）始终是一种选择，这种方法从磁盘的第一页开始，直至最后一页。此外，每一个索引都可以是另一种数据访问路径，特别是当索引与选择谓词相匹配时，这将有助于减少搜索成本。
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

**2️⃣**For queries over a **single relation**:

1. Each availableaccess path (file scan / index) is considered, and the one with the **lowest estimated cost** is chosen
   - **Heap scan is always one alternative**
   - **Each index can be another alternative (if matching selection predicates)**
2. Other operations can be performed on top of access paths, but they typically don’t incur additional cost since they are done on the fly(e.g. projections, additional non-matching predicates)

```
对于针对单一关系的查询，我们需要考虑每个可用的访问路径（如文件扫描或索引），并选择估算成本最低的路径。这里需要记住，堆扫描总是一个备选方案，而每一个索引都可能是另一个选择，尤其是当索引与选择性谓词相匹配时。除了访问路径之外，还可以执行其他操作，如投影或额外的不匹配谓词，但这些通常不会增加额外成本，因为它们是即时完成的。这里的关键是了解不同的索引和谓词，以及如何区分哪些谓词是匹配的，哪些谓词将在后续过程中即时检查。这个区分是学生们常常感到困难的一个方面。
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420222557144.png" alt="image-20240420222557144" style="zoom:50%;" /> 

```
让我们来完善一下这个例子，使其更加清晰。假设我们有一个 SQL 查询： SELECT * FROM Table A WHERE A=5 AND B>6 AND C<8 AND D=10。在这个查询中，我们有四个条件（谓词）来过滤数据。每个条件，如 A=5、B>6、C<8 和 D=10，都可能有自己的缩减因子，表示每个条件对数据集的缩减程度。因此，我们需要考虑这四个缩减因子，以了解查询对检索数据大小的总体影响。
Let's refine that example to make it clearer. Imagine we have a SQL query: SELECT * FROM Table A WHERE A=5 AND B>6 AND C<8 AND D=10. In this query, we have four conditions (predicates) that filter the data. Each condition, such as A=5, B>6, C<8, and D=10, might have its own reduction factor, which indicates how much each condition reduces the data set. Thus, we need to consider these four reduction factors to understand the overall impact of the query on the size of the data retrieved.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420222844906.png" alt="image-20240420222844906" style="zoom:50%;" /> 

```
为简单起见，让我们考虑使用两个索引计算替代品成本的过程。假设我们在 A 和 B 列上有一个索引，在 A、C 和 D 列上有另一个索引。
Let's consider the process of costing alternatives with two indices for simplicity. Assume we have one index on columns A and B, and another on columns A, C, and D. For the sake of this example, let's also assume that both indices are clustered.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420222844906.png" alt="image-20240420222844906" style="zoom:50%;" /> 

```
我们要讨论的第一种成本计算策略是堆扫描。需要注意的是，在使用堆扫描时，我们可能使用的谓词并不重要。这是因为堆扫描中的数据是未分类的，因此我们需要扫描所有数据，而不考虑任何特定条件。
Now, focus on a single table, which we'll refer to as relation A. The first costing strategy we will discuss is a heap scan. The cost associated with a heap scan is essentially the number of pages in relation A. It's important to note that when employing a heap scan, the predicates we might have are irrelevant. This is because the data in a heap scan is unsorted, and therefore, we need to scan through all the data regardless of any specific conditions.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$



<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420223052665.png" alt="image-20240420223052665" style="zoom:50%;" /> 

```
现在，让我们来计算两个索引的相关成本，我们将其分别称为索引 I1 和索引 I2。索引 I1 是一个聚类索引，从索引 I1 开始，我们将使用聚类索引的特定公式。该公式是索引中的页数加上关系中的页数之和，然后将该和乘以适用的缩减因子的乘积。让我们来确定适用的缩减因子。对于涵盖 A 列和 B 列的索引 I1，该索引有助于高效查询 A 等于 5、B 等于 6 的条件。与 C 和 D 相关的谓词与该索引所覆盖的列不匹配，这意味着它们不会影响 I1 的成本计算。
Now, let's calculate the costs associated with two indices, which we'll refer to as index I1 and index I2. Starting with index I1, which is a clustered index, we'll use the specific formula for clustered indices. This formula is the sum of the number of pages in the index plus the number of pages in the relation, and then we multiply this sum by the product of the applicable reduction factors. Let's identify which reduction factors apply. For index I1, which covers columns A and B, this index aids in efficiently querying for conditions where A equals five and B equals six. Consequently, the reduction factor for A significantly impacts the cost calculation, and this is then multiplied by the reduction factor for B. The predicates related to C and D do not match the columns covered by this index, meaning they do not influence the cost calculation for I1.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420152750219.png" alt="image-20240420152750219" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420223231656.png" alt="image-20240420223231656" style="zoom: 50%;" /> 

```
现在我们来考虑第二个索引的成本计算，我们称之为索引 I2。使用的公式与 I1 相同：索引 I2 中的页数加上相关关系中的页数，再乘以适用的缩减系数的乘积。对于索引 I2（索引列 A、C 和 D），适用的缩减因子是 A、C 和 D 的缩减因子。这些因子至关重要，因为它们决定了索引根据指定条件缩小结果范围的效果。因此，这些因素会增加使用指数 I2 的成本。另一方面，该索引不包括列 B 上的任何条件。因此，B 上的条件将在查询执行过程中 "即时 "评估，不会影响使用索引 I2 的初始成本估算。
Let's now consider the cost calculation for the second index, which we'll call index I2. The formula used is identical to that for I1: the number of pages in index I2 plus the number of pages in the associated relation, all multiplied by the product of the applicable reduction factors. For index I2, which indexes columns A, C, and D, the applicable reduction factors are those for A, C, and D. These factors are crucial as they determine how effectively the index narrows down the results based on the specified conditions. Therefore, these are the factors that will contribute to the cost of using index I2. On the other hand, any condition on column B is not covered by this index. Consequently, the condition on B will be evaluated 'on the fly' during query execution and does not affect the initial cost estimation for using index I2.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420153112133.png" alt="image-20240420153112133" style="zoom:50%;" /> 

**1️⃣**Sequential (heap) scan of data file:

- **Cost = NPages(R)**

**顺序（堆）扫描数据文件**：
   顺序扫描或全表扫描是一种简单但通常成本较高的数据检索方法，整个表会一页页地被读取。顺序扫描的成本等于关系 $R$ 的页数，表示为 $ \text{Cost} = \text{NPages}(R) $。当没有合适的索引可用于查询优化时，会使用此方法。

 A sequential or full table scan is a straightforward yet often costly data retrieval method where the entire table is read disk-page by disk-page. The cost of a sequential scan is equal to the number of pages of the relation $R$, denoted as $\text{Cost} = \text{NPages}(R) $. This method is used when no suitable indexes are available for query optimization.
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420153112133.png" alt="image-20240420153112133" style="zoom:50%;" /> 

**2️⃣**Index selection over a primary key (just a single tuple):

- **Cost(B+Tree)=Height(I)+1**, Height is the index height
- **Cost(HashIndex)= ProbeCost(I)+1**, ProbeCost(I)~1.2

**基于主键的索引选择**：
   基于主键查询时，我们需要考虑两种类型的索引：B+树和哈希索引。通过 B+树索引访问记录的成本由索引的高度决定，表示为 $ \text{Cost(B+Tree)} = \text{Height}(I) + 1 $。相比之下，对于哈希索引，成本涉及大约为1.2的探测成本，这是因为一个桶内可能有多个页面，从而导致 $ \text{Cost(HashIndex)} = \text{ProbeCost}(I) + 1 $。

When querying based on a primary key, we have two types of indexes to consider: B+Tree and HashIndex. The cost of accessing a record via a B+Tree index is determined by the height of the index, denoted as $\text{Cost(B+Tree)} = \text{Height}(I) + 1 $. In contrast, for a HashIndex, the cost involves a probe cost of approximately 1.2 due to potentially multiple pages within a bucket, leading to $\text{Cost(HashIndex)} = \text{ProbeCost}(I) + 1 $.
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420153112133.png" alt="image-20240420153112133" style="zoom:50%;" /> 

**3️⃣**Clustered index matching one or more predicates:

- **Cost(B+Tree)=(NPages(I) + ==NPages(R)==)***$\prod\limits_{i=1 . . n} R F_i$
- **Cost(HashIndex)= NPages(R)***$\prod\limits_{i=1 . . n} R F_i * 2.2$​

**聚集索引匹配一个或多个谓词**：
   对于数据物理存储顺序与索引顺序一致的聚集索引，成本计算涉及索引页面数和匹配谓词的数据页面数。这表示为 $ \text{Cost(B+Tree)} = (\text{NPages}(I) + \text{NPages}(R)) \times \prod_{i=1}^{n} RF_i $，其中 $ RF_i $ 是基于谓词的降低因子。在类似情况下，哈希索引的成本略有增加，以考虑额外的读取操作，导致 $ \text{Cost(HashIndex)} = \text{NPages}(R) \times \prod_{i=1}^{n} RF_i \times 2.2 $。

For a clustered index, where the data is physically stored in the order of the index, the cost calculation involves both the number of index pages and the number of data pages that match the predicate. This is expressed as $\text{Cost(B+Tree)} = (\text{NPages}(I) + \text{NPages}(R)) \times \prod_{i=1}^{n} RF_i $, where $RF_i $ are the reduction factors based on the predicates. The cost for a HashIndex in a similar scenario increases slightly to account for additional reads, leading to $\text{Cost(HashIndex)} = \text{NPages}(R) \times \prod_{i=1}^{n} RF_i \times 2.2 $.
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420153112133.png" alt="image-20240420153112133" style="zoom:50%;" /> 

**4️⃣**Non-clustered index matching one or more predicates:

- **Cost(B+Tree)=(NPages(I) + ==NPages(R)==)***$\prod\limits_{i=1 . . n} R F_i$
- **Cost(HashIndex)= NTuples(R)***$\prod\limits_{i=1 . . n} R F_i * 2.2$​

**非聚集索引匹配一个或多个谓词**：
   对于非聚集索引，数据并非按索引顺序顺序存储，因此在检索匹配谓词的数据时会产生额外成本。B+树的公式与聚集索引相似，但涉及整个关系的数据检索：$ \text{Cost(B+Tree)} = (\text{NPages}(I) + \text{NPages}(R)) \times \prod_{i=1}^{n} RF_i $。相比之下，哈希索引的成本是基于元组数量而非页面计算的，因此为 $ \text{Cost(HashIndex)} = \text{NTuples}(R) \times \prod_{i=1}^{n} RF_i \times 2.2 $。

 With non-clustered indexes, data isn't stored sequentially as per the index, so additional costs are incurred when retrieving data matching the predicates. The formula for B+Tree remains similar to that of a clustered index but involves the entire relation for data retrieval: $\text{Cost(B+Tree)} = (\text{NPages}(I) + \text{NPages}(R)) \times \prod_{i=1}^{n} RF_i $. In contrast, the cost for a HashIndex is calculated based on the number of tuples rather than pages, hence $\text{Cost(HashIndex)} = \text{NTuples}(R) \times \prod_{i=1}^{n} RF_i \times 2.2 $.
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420155536745.png" alt="image-20240420155536745" style="zoom:50%;" /> 

**0️⃣**Let’s say that Sailors(S) has 500 pages, 40000 tuples, NKeys(rating) = 10

```sql
SELECT S.sidFROM Sailors S WHERE S.rating=8
```

$\text{Result size = (1/NKeys(rating)) * NTuples(S) = (1/10)*40000 =4000 tuples}$

**1️⃣**If we have **I(rating)**, NPages(I) = 50:

1. Clustered index:

   $\text{Cost = (1/NKeys(rating))*(NPages(I)+NPages(S))=(1/10)*(50+500) = 55 I/O}$

2. Unclustered index:

   $\text{Cost = (1/NKeys(rating))*(NPages(I)+NTuples(S))=(1/10)*(50+40000) = 4005 I/O}$

```
在本例中，我们要检查的是从 "水手 "表中选取的内容，其中 "等级 "等于 8。该表由 500 页中的 40,000 行组成，其中 "等级 "有 10 个不同的值。首先，我们根据这些值计算预期结果大小，估计约为 4000 行。接下来，让我们考虑在该表中建立两个假设索引：一个是 "rating "索引，另一个是 "sid "索引。在讨论中，我们假定不清楚这些索引是聚类的还是非聚类的。通常情况下，在实际操作中，你会知道每个索引的类型，并相应地使用适当的计算方法。如果 "评级 "索引是聚簇索引，并且跨越 50 个页面，那么计算得出的 I/O 操作数将大大少于非聚簇索引，后者会因为必须访问更多数据页面而导致 I/O 数大大增加。了解这些区别至关重要，因为它们会极大地影响数据库操作的性能。
In this example, we're examining a selection from the 'sailors' table where 'rating' equals 8. The table consists of 40,000 rows across 500 pages, with 'rating' having 10 distinct values. Initially, we calculate the expected result size based on these values, estimating around 4,000 rows.Next, let's consider two hypothetical indexes on this table: one on 'rating' and another on 'sid'. For this discussion, we'll assume that it's unclear whether these indexes are clustered or unclustered. Typically, in practice, you would know the type of each index and use the appropriate calculation method accordingly. However, for illustrative purposes, I'll demonstrate both calculations.If the 'rating' index is clustered and spans 50 pages, the calculation would yield a significantly reduced number of I/O operations compared to an unclustered index, which would result in a much higher I/O count due to the necessity of accessing more data pages. Understanding these distinctions is crucial as they greatly influence the performance of database operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420155536745.png" alt="image-20240420155536745" style="zoom:50%;" /> 

**2️⃣**If we have an $\text{I(sid), NPages(I)= 50}$​

1. Cost = ?, Result size = ?
2. Would have to retrieve all tuples/pages. With a **clustered** index, the **cost is 50+500**, with **unclustered** index, **50+40000**

**3️⃣**Doing a file scan:

- $\text{Cost = NPages(S) = 500}$​

```
让我们考虑这样一个场景：我们有一个涵盖 50 页的“水手 ID”索引。 我们可能会问的第一个问题是如何计算使用该指数的成本。 值得注意的是，在这种特定情况下，没有降低因子来降低访问索引的成本。 这意味着缩减因子实际上是1，表明使用该索引不会减少操作的成本。如果这是一个聚集索引，我们可以使用一定的公式计算成本。 然而，即使索引是聚集的，与其他选项相比，成本也可能相对较高，因为我们没有从数据访问的任何减少中受益。尽管成本很高，但此类索引在现实场景中仍然有用。 例如，如果这是一个B+树索引，我们只需遍历叶子节点，就可以按排序顺序检索数据。 即使没有谓词来过滤记录，这也是有利的。 有时，数据库可能会出于此原因选择使用此索引，即使它不是减少 I/O 操作的最具成本效益的选项。 最后，不要忽视堆扫描选项，这一点很重要。 学生经常忘记这是一种可行的选择，但它始终存在，有时甚至是最便宜的选择。 最终，数据库优化器将在执行查询的可用策略中选择最具成本效益的方法。 此示例说明单关系查询计划中的决策如何根据操作的特定要求和约束而变化。
Let's consider a scenario where we have an index on 'sailor ID' that covers 50 pages. The first question we might ask is how to calculate the cost of using this index. It's important to note that in this specific case, there are no reduction factors to lower the cost of accessing the index. This means the reduction factor is effectively one, indicating that using this index will not reduce the cost of the operation.If this were a clustered index, we could calculate the cost using a certain formula. However, even if the index is clustered, the cost is likely to be relatively high compared to other options because we are not benefiting from any reduction in data access.Despite the high cost, such indices can still be useful in real-life scenarios. For example, if this is a B+ tree index and we simply traverse through the leaf nodes, we can retrieve data in a sorted order. This can be advantageous even if there are no predicates to filter the records. Sometimes, databases might choose to use this index for that reason, even if it's not the most cost-effective option for reducing I/O operations. Finally, it's essential not to overlook the option of a heap scan. This is often forgotten by students as a viable alternative, but it's always present and sometimes it turns out to be the cheapest option. Ultimately, the database optimizer will select the most cost-effective approach among the available strategies for executing a query. This example illustrates how decision-making in single-relation query plans can vary depending on the specific requirements and constraints of the operation.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420160803961.png" alt="image-20240420160803961" style="zoom:50%;" /> 

**1️⃣**Steps:

1. Select orderof relations
   - E.g. SxRxB, or SxBxRor RxSxB…
   - maximum possible orderings = N!
2. For each join, select **join algorithm**
   - E.g. Hash join, Sort-Merge Join…
3. For each input relation, select access method
   - Heap Scan, or various index alternatives

```
在处理多相关计划，尤其是涉及连接的计划时，过程会变得复杂得多。以下是优化器处理这些查询的三个步骤：

首先，优化器确定关系的顺序。它会系统地枚举所有可能的关系序列。例如，如果我们考虑涉及表 Sr、B 和 R 的三表连接，优化器会探索这些表的每一种排列，如 Sr-B-R、S-B-R、R-S-B 等。请记住，可能的排序总数是以表的数量为基础的阶乘性质--不过记住这个数字并不是必要的；它只是为了提供一个规模概念。

接下来，对于确定的每个有序序列，优化器必须为每个连接操作选择合适的连接实现。选项可能包括散列连接、排序合并连接或嵌套循环连接等。

最后，优化器会决定计划底部每个输入关系的访问方法。可供选择的方法多种多样，从简单的堆扫描到更复杂的索引扫描，包括聚类索引或带有辅助索引的堆扫描。每种方法对性能和效率都有各自的影响。

When dealing with multi-relation plans, particularly those involving joins, the process becomes significantly more complex. Here's how the optimizer handles these queries in three steps:

First, the optimizer determines the order of the relations. It systematically enumerates all possible sequences of the relations. For instance, if we consider a three-table join involving tables Sr, B, and R, the optimizer explores every permutation of these tables, such as Sr-B-R, S-B-R, R-S-B, and so forth. Remember, the total number of possible orderings is factorial in nature based on the number of tables—though it's not essential to memorize this number; it's just to give an idea of the scale.

Next, for each ordered sequence identified, the optimizer must choose the appropriate join implementation for each join operation. The options might include hash joins, sort-merge joins, or nested loops joins, among others.

Finally, the optimizer decides on the access method for each input relation at the bottom of the plan. The choices vary from simple heap scans to more complex indexed scans, including clustered indexes or heap scans with auxiliary indexes. Each method has its own implications for performance and efficiency.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420160803961.png" alt="image-20240420160803961" style="zoom:50%;" /> 

**2️⃣**Q: How many plans are there for a query over N relations? Back-of-envelope calculation:

1. With 3 join algorithms, I indexes per relation: 

   $\text { \# plans } \approx[N !]^*\left[3^{(N-1)}\right]{}^*\left[(\mathrm{I}+1)^{\mathrm{N}}\right]$

2. Suppose N = 3, I = 2: 

   $\text { plans } \approx 3 ! * 3^2 * 3^3 = 1458\text{ plans}$

3. This is just for illustration –you don’t need to remember this

``` 
在这个例子中，我们来探讨数据库查询优化器的任务复杂性，通过查看它在处理多个关系的查询时会考虑多少种计划。请记住，我即将分享的公式很复杂，但你不需要记住它。我的目的仅仅是为了突出这个数字的庞大规模。在处理N个关系时，优化器首先考虑这些关系的所有可能排序，这等于N的阶乘（N!），代表所有排列。接下来，对于N个表，有N-1个连接。例如，如果有三个表，就有两个连接。假设有三种可能的连接算法——堆扫描（hash scan）、嵌套循环（nested loops）和散列连接（hash join），不同连接计划的数量是3的N-1次幂。此外，如果有I个索引，优化器会为每个表考虑I+1个可能的访问路径（额外的路径考虑了堆扫描），总共是(I+1)^N个可能的访问路径。综合起来，即使在只有三个表的简单情况下，优化器评估的潜在执行计划数量也是惊人的，大约有1458种不同的计划。再次强调，这里的重点不是记住公式，而是理解数据库优化器常规处理的规模之大。
In this example, let's explore the complexity of the database query optimizer's task by looking at the number of plans it will consider for a query over multiple relations. Remember, the formula I'm about to share is complex, but you don't need to memorize it. My aim is simply to highlight the sheer scale of the number. When dealing with N relations, the optimizer first considers all possible orders of these relations, which amounts to N! (N factorial), representing all permutations. Next, for N tables, there are N-1 joins. For instance, with three tables, there are two joins. Assuming there are three possible join algorithms—hash scan, nested loops, and hash join—the number of different join plans is 3^(N-1). Additionally, if there are I indices available, the optimizer considers I+1 possible access paths for each table (the additional path accounts for a heap scan), leading to (I+1)^N possible access paths overall. Putting it all together, even for a simple scenario with just three tables, the number of potential execution plans the optimizer evaluates is staggering, amounting to approximately 1458 different plans. Again, the focus here isn't on memorizing the formula but on understanding the magnitude of what database optimizers handle routinely
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420162505917.png" alt="image-20240420162505917" style="zoom:50%;" /> 

**1️⃣**As number of joins increases, number of alternative plans grows rapidly 

→==need to restrict search space==

**2️⃣**Fundamental decision in System R (first DBMS): **only left-deep join trees** are considered

- Left-deep trees allow us to generate all **fully pipelined** plans

  - Intermediate results are not written to temporary files

    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420224518730.png" alt="image-20240420224518730" style="zoom: 67%;" /> 
    
     

```
正如我们所观察到的，优化器的任务相当复杂，随着关系数量的增加，备选方案的数量也呈指数级增长。因此，优化器需要限制搜索空间。IBM 开发的第一个关系 DBMS System R 做出了一个开创性的决定，即只考虑左深连接树。这一决定后来被商业数据库普遍采用，主要是因为它能有效降低计算成本。左深计划可以生成完全流水线式的计划，在这种计划中不存储中间结果；一个操作的输出直接作为下一个操作的输入。这就是流水线流程的含义--不断向上游推送数据，如前面的示例所示。查看这些计划时，只有左侧显示的计划符合左深树的条件，因为每个操作的输出都是后续操作的左输入。这些计划是完全流水线式的；因此，当我们执行 "在一个 T 上连接 B "这样的连接操作时，数据会立即被推送到下一个连接操作，依此类推，而不会存储这些操作的结果。
As we've observed, the task of the optimizer is quite complex, and as the number of relations increases, the number of alternative plans grows exponentially. Therefore, the optimizer needs to restrict the search space. A seminal decision made by System R, the first relational DBMS developed by IBM, was to consider only left-deep join trees. This decision has since been universally adopted by commercial databases, primarily because it effectively reduces computational costs. Left-deep plans enable the generation of completely pipelined plans where no intermediate results are stored; the output of one operation is directly pushed as the input to the next. This is what a pipelined process entails—continuously pushing data upstream, as illustrated in a previous example. When reviewing the plans, only the one shown on the left qualifies as a left-deep tree because the output of each operation serves as the left input for the subsequent operation. These plans are fully pipelined; thus, as we execute a join operation like 'join B over one T,' it is immediately pushed further up to the next join, and so on, without ever storing the results of these operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420162505917.png" alt="image-20240420162505917" style="zoom:50%;" />

**1️⃣**As number of joins increases, number of alternative plans grows rapidly 

→==need to restrict search space==

**2️⃣**Fundamental decision in System R (first DBMS): **only left-deep join trees** are considered

- Left-deep trees allow us to generate all **fully pipelined** plans

  - Intermediate results are not written to temporary files

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420162614698.png" alt="image-20240420162614698" style="zoom: 50%;" /> 

```
与 "连接 B "这样的左深树相反，图中的其他计划并不是左深连接树。右边的计划被归类为丛生计划，中间的计划则是一种混合计划。为了有效地缩小搜索空间，这类计划从一开始就会被丢弃。
In contrast to the left-deep tree like 'join B,' the other plans depicted are not left-deep join trees. The plan on the right is classified as bushy, and the one in the middle represents a sort of hybrid. To reduce the search space effectively, these types of plans are dis🚗ded right from the start.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164003754.png" alt="image-20240420164003754" style="zoom:50%;" /> 

```sql
SELECT S.sname, B.bname, R.day
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid
```

Let’s assume:

**1️⃣**Two join algorithms to choose from:

1. Hash-Join
2. NL-Join (page-oriented)

**2️⃣**Clustered B+Tree index: $\text { I(R.sid); NPages }(\text{I})=50$

**3️⃣**No other indexes

**4️⃣**$\text {S: NPages }(S)=500, \text { NTuplesPerPage }(S)=80$

**5️⃣**$\text { R: NPages }(R)=1000, \text { NTuplesPerPage }(R)=100$​

**6️⃣**$\text {B: NPages }(B)=10$

**7️⃣**$100 \mathrm{R} \bowtie \mathrm{S} $​tuples fit on a page

```
现在，让我们一起来枚举这个简单例子中的所有计划，在这个例子中，我们有一个三表联接，基本上有两个联接谓词。为了简单起见，在这个例子中，我假设只有哈希连接和嵌套循环连接两种选择，这样可以稍微缩小搜索空间。但实际上，排序合并连接通常也是可用的。我还假设只有一个关于预留卖家 ID 的索引，不存在其他索引。此外，我这里还有关于我的页面的信息。
Let's now enumerate all the plans together in this straightforward example, where we have a three-table join with essentially two join predicates. For simplicity, in this example, I'll assume the only alternatives are hash join and nested loops join, just to slightly reduce my search space. Although in reality, sort merge joins are also typically available. I'll also assume there's just one index on a reserved seller ID and no other indices exist. Additionally, I have information about my pages here.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164536139.png" alt="image-20240420164536139" style="zoom:50%;" /> 

```sql
SELECT S.sname, B.bname, R.day
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid
```

**1️⃣**Enumerate **relation orderings**: ==* Prune plans with cross-products immediately!==

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164613612.png" alt="image-20240420164613612" style="zoom:50%;" /> 

```
让我们从第一步开始：列举关系排序。有三种关系--S、R 和 B，可能的排列组合是 S-R-B、S-B-R、R-S-B、R-B-S、B-S-R 和 B-R-S。有六种可能的排列方式，分别对应三个阶乘或六个阶乘。其中，你会发现有两种方案略有不同；例如，这里的连接 BS 实际上代表的是交叉积。从查询中可以看出，其原因在于 S 和 R 之间的连接是自然连接，而 R 和 B 之间缺少连接谓词来合并它们，因此必须进行交叉积，而交叉积的代价非常昂贵。因此，在数据库系统中，交叉乘积通常是避免的，这样的计划从一开始就会被剪掉。优化器甚至不会考虑这些计划，从而有效地将它们排除在考虑范围之外。
Let's start with the first step: enumerating relation orderings. With three relations—S, R, and B—the possible permutations are S-R-B, S-B-R, R-S-B, R-B-S, B-S-R, and B-R-S. There are six possible orderings, corresponding to three factorial, or six. Among these, you'll notice two plans that differ slightly; for example, a join BS here actually represents a cross product. The reason for this, as you can observe in the query, is that while the join between S and R is a natural join, R and B lack join predicates to merge them, necessitating a cross product, which is, as you might recall, very expensive. Consequently, in database systems, cross products are generally avoided, and such plans will be pruned right from the start. The optimizer won't even consider these plans, effectively eliminating them from consideration.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164636947.png" alt="image-20240420164636947" style="zoom:50%;" /> 

```sql
SELECT S.sname, B.bname, R.day
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid
```

**2️⃣**Enumerate **join algorithm** choices:

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164710233.png" alt="image-20240420164710233" style="zoom:50%;" />  + do the same for other plans

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164726676.png" alt="image-20240420164726676" style="zoom:50%;" /> 

```
枚举出这些关系排序后，下一步就是决定为每个连接使用哪种连接算法。在本例中，我将选项限制为嵌套循环和散列连接。对于一棵特定的树，可能的组合包括嵌套循环后嵌套循环、哈希连接后嵌套循环、嵌套循环后哈希连接以及哈希连接后哈希连接。这些是这棵树的选项，其他树也需要评估类似的组合。
Once we have enumerated these relation orderings, the next step is to decide which join algorithm to use for each of these joins. In this example, I'm limiting the options to nested loops and hash joins. For one particular tree, the possibilities include combinations like nested loops followed by nested loops, hash join followed by nested loops, nested loop followed by hash join, and hash join followed by hash join. These are the options for this tree, and similar combinations need to be evaluated for all other trees as well.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164800094.png" alt="image-20240420164800094" style="zoom:50%;" /> 

```sql
SELECT S.sname, B.bname, R.day
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid
```

**3️⃣**Enumerate **access method** choices:

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164828833.png" alt="image-20240420164828833" style="zoom: 50%;" /> + do same for other plans

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420164846871.png" alt="image-20240420164846871" style="zoom:50%;" /> 

```
最后，在选择了连接算法后，下一步就是决定对每个表使用哪种访问路径。这涉及确定每个表的访问方式，是堆扫描还是索引扫描，以及其他方法。因此，可能的访问路径包括通过堆扫描或使用 R 上的索引访问所有表，而其他表 S 和 B 则由于没有其他索引而继续使用堆扫描。
Finally, after selecting the join algorithm choices, the next step is to decide which access paths to use for each table. This involves determining how each individual table is accessed, whether by heap scan or index scan, among other methods. In this scenario, my choices are simplified because the only index I have is on relation R. Therefore, the possible access paths include all tables being accessed via heap scan or using the index on R while the other tables, S and B, continue with heap scans due to the absence of other indices.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> 

**1️⃣**Code

```sql
SELECT S.sname, B.bname, R.day
FROM Sailors S, Reserves R, Boats B
WHERE S.sid = R.sid AND R.bid = B.bid
```

$$
\begin{aligned}
& S: \text { NPages }(S)=500, \text { NTuplesPerPage }(S)=80 \\
& R:  \text { NPages }(R)=1000, \text { NTuplesPerPage }(R)=100 \\
& B: \text { NPages }(B)=10
\end{aligned}
$$

$100 R \bowtie S$​​tuples fit on a page, All 3 relations are Heap Scan

```
优化器的目标是枚举所有这些计划，然后计算每个计划的成本，就像我们将要做的那样。让我们关注屏幕上方的计划，它由两个嵌套循环组成，使用堆扫描连接。计算成本所需的信息显示在屏幕右侧。请记住，成本是以从磁盘读取的页数来衡量的。通过查看计划，我们可以看到两个嵌套循环连接。为了计算它们的成本，我们从计划的底部开始计算--这里是执行的起点。如果你还记得我前面提到的，在执行过程中，图元会被推向上游；这就是我们处理查询的方式。
The optimizer's goal is to enumerate all these plans and then calculate the cost of each one, just as we're about to do. Let’s focus on the plan at the top of the screen, which consists of two nested loops joins using heap scans. The information required for cost calculation is displayed on the right-hand side of the screen. Remember, the cost is measured in terms of the number of pages read from disk. Examining the plan, we see two nested loops joins. To calculate their costs, we start from the bottom of the plan—this is where execution begins. If you recall what I mentioned earlier, tuples are pushed upstream during execution; this is how our queries are processed.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" alt="image-20240420170912351" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$

```
第一步是使用适当的成本公式计算嵌套循环连接的成本，假定它是面向页面的嵌套循环连接。对于这种类型的连接，成本公式是外层表的页数加上外层表的页数乘以内层表的页数。这里，表 S 的页数为 500 页，表 R 的页数为 1000 页，因此 I/O 操作的成本超过 500,000 次。这个计算结果是第一次操作的成本。现在的挑战是确定后续嵌套循环连接的成本。然而，复杂性在于不知道左侧输入--该输入实际上是前一个操作的结果。要继续操作，我必须计算这次操作的结果大小，以了解将被推上的图元的数量，这对于确定下一次连接的输入至关重要。这种计算方法凸显了左深计划在查询执行过程中的重要性。
The first step is to calculate the cost of this nested loops join using the appropriate cost formula, assuming it's a page-oriented nested loops join. For this type of join, the cost formula is the number of pages of the outer table plus the number of pages of the outer table multiplied by the number of pages of the inner table. Here, the number of pages is 500 for table S and 1000 for table R, resulting in a cost of over 500,000 I/O operations. This calculation represents the cost of the first operation. The challenge now is to determine the cost of the subsequent nested loops join. However, the complexity lies in not knowing the left input—this input is actually the result of the previous operation. To proceed, I must calculate the result size of this operation to understand the number of tuples that will be pushed up, which is critical for determining the input for the next join. This calculation highlights the importance of left-deep plans in the query execution process.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" alt="image-20240420170912351" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$
2. **(SxR)xB**: 
   - $\text{Result size (SxR) = 40000*100000 *1/40000= 100000 tuples = 1000 pages}$​


```
让我们用以前学过的公式计算表 S 和表 R 之间连接的结果大小。计算涉及左右输入的页数，再乘以连接的两列中最大不同键数的倒数。为了确定相关列，我们查看 S 和 R 之间的联合条件，其中涉及列 SI 和 RSI。SI 是 "水手 "表的主键，即水手 ID。由于水手 ID 是主键，所以最大的不同键数与水手 ID 中的记录数相对应。虽然 "储备 "表可能包含多达 100,000 条记录，但作为外键的水手 ID 最多只能有 40,000 个不同的值，因为一名水手可以预订多个储备，但仍能通过唯一的水手 ID 进行识别。这种区分对于计算联合性能至关重要，通常需要密切关注。
Let's calculate the result size of the join between tables S and R using the formula we've previously learned. This calculation involves the number of pages of the left and right inputs, multiplied by the reciprocal of the maximum number of distinct keys in either of the two columns being joined. To identify the relevant columns, we look at the joint condition between S and R, which involves columns SI and RSI. SI is the primary key of the 'sailors' table, known as sailor ID. The maximum number of distinct keys corresponds to the number of records in sailor ID, given it's the primary key. While the 'reserves' table may contain up to 100,000 records, the sailor ID as a foreign key can only have a maximum of 40,000 distinct values since a sailor can book multiple reserves but still be identified by a unique sailor ID. This distinction is vital for calculating the joint performance and often requires close attention.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" alt="image-20240420170912351" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$
2. **(SxR)xB**: 
   - $\text{Result size (SxR) = 40000*100000 *1/40000= 100000 tuples = 1000 pages}$​

```
通过上述计算和考虑，我们发现连接会产生 100,000 个图元。根据每页可容纳 100 个数据元组的规定，这一操作将产生 1,000 页。这 1000 个页面将被推送，作为下一个嵌套循环连接的输入。这一步对于推进查询执行计划的各个阶段至关重要，它展示了数据库操作之间的相互作用，以及在计算数据库查询的成本和效率时了解底层数据结构的重要性。
Given these calculations and considerations, we find that the join results in 100,000 tuples. With the specification that each page can hold 100 tuples, this equates to 1,000 pages that result from this operation. These 1,000 pages will be pushed up to serve as the input for the next nested loops join. This step is crucial in advancing through the stages of the query execution plan, demonstrating the interaction between database operations and the significance of understanding the underlying data structure when calculating the cost and efficiency of database queries.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" alt="image-20240420170912351" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$

2. **(SxR)xB**: 
   - $\text{Result size (SxR) = 40000*100000 *1/40000= 100000 tuples = 1000 pages}$
   - $\text{Cost(xB) = 1000 + 1000*10 = 10000}$


```
要计算第二个嵌套循环连接的成本，我们可以考虑直接应用公式。我们将第一次连接输出的 1,000 页乘以关系 B 的 1,010 页，得出一个可观的数字。不过，需要注意的一个重要方面是，我们使用的是左深计划。左深计划的本质是将一个操作的输出直接用作后续操作的输入，而不存储中间结果。由于采用了流水线方式，这种方法从根本上允许我们放弃与首次读取下一个操作的左输入相关的成本。这种高效的数据流处理方式是左深连接计划的主要优势之一，它可以优化处理过程，最大限度地减少不必要的数据处理成本。
To calculate the cost for the second nested loop join, let's consider applying the formula straightforwardly. We would take the 1,000 pages from the output of the first join and multiply this by the 1,010 pages of relation B, resulting in a substantial number. However, an important aspect to keep in mind is that we are utilizing left-deep plans. The essence of left-deep plans is that the output from one operation is directly used as the input for the subsequent operation, without storing intermediate results. This approach essentially allows us to dis🚗d the cost associated with the first read of the left input for the next operation, thanks to pipelining. This efficient handling of data flows is one of the key advantages of left-deep join plans, optimizing processing and minimizing unnecessary data handling costs.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" alt="image-20240420170912351" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$

2. **(SxR)xB**: 

   - $\text{Result size (SxR) = 40000*100000 *1/40000= 100000 tuples = 1000 pages}$
   - $\text{Cost(xB) = 1000 + 1000*10 = 10000}$

   <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165450197.png" alt="image-20240420165450197" style="zoom:50%;" /> 

   ==Already read –left deep plans apply pipelining==

3. $\text{Total Cost = 500 + 500*1000 + 1000 * 10 = 510500 I/O}$​

```
在我们的方法中，由于左深计划固有的流水线特性，我们总是舍弃第一次读取的成本。因此，第二个嵌套循环连接的成本只占我们计算的第二部分。因此，这次连接的总页数为 10,000 页。然后，整个计划的总成本就是这些单独操作的总和，最后得出成本总计，有效说明了我们的查询优化策略的效率。
In our approach, we always dis🚗d the cost of the first read due to the pipelining inherent in left-deep plans. Thus, the cost for the second nested loops join solely comprises the second part of our calculations. This results in a total of 10,000 pages for this join. The overall cost of the entire plan is then simply the sum of these individual operations, culminating in a final tally of costs, effectively illustrating the efficiency of our query optimization strategy.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$

2. **(SxR)xB**: 

   - $\text{Result size (SxR) = 100000*40000 *1/40000= 100000 tuples = 1000 pages}$

```
让我们深入探讨这一实践，因为它包含了我们在过去五次讲座中讨论过的概念，尤其是关于成本计算的概念。这一次，我们要评估的是一个不同的计划，它看起来与之前的计划非常相似，主要的变化是在上面添加了一个哈希连接。与之前的方法相同，我们使用相同的公式从嵌套循环连接开始。此外，确定后续哈希连接的输入也至关重要。通过参考之前操作的结果大小（我们之前计算过），我们知道输入量为 1,000 页。这一基础步骤对于理解不同的连接方法及其相关成本如何影响整个查询计划至关重要。
Let’s dive deeper into this practice as it encapsulates the concepts we've discussed in the last five lectures, particularly around costing. This time, we're evaluating a different plan that looks quite similar to the previous one, with the key change being a hash join on top. Starting off in the same manner as before, we begin with a nested loops join using the same formula. Additionally, it's crucial to determine the input for the subsequent hash join. By referencing the result size from the previous operation, which we calculated earlier, we know this input amounts to 1,000 pages. This foundational step is critical for understanding how the different join methods and their associated costs influence the overall query plan.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165025200.png" alt="image-20240420165025200" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420165035271.png" alt="image-20240420165035271" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170912351.png" style="zoom:50%;" /> 

**2️⃣****Calculating cost:**

1. **SxR**: $\text{Cost (SxR) = 500 + 500*1000 = 500500}$

2. **(SxR)xB**: 

   - $\text{Result size (SxR) = 100000*40000 *1/40000= 100000 tuples = 1000 pages}$
   - $\text{Cost(xB) = 3*1000 + 3*10 = 2*1000 + 3*10 = 2030}$

   <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420170823369.png" alt="image-20240420170823369" style="zoom:50%;" /> 

   ==Already read once –left deep plans apply pipelining==

3. $\text{Total Cost = 500 + 500*1000 + 2*1000+ 3*10 = 502530 I/O}$

```
在应用散列连接公式时，我们通常会将其计算为左边输入的三倍加上右边输入的三倍。然而，由于本计划使用的是左深树结构，流水线的效果允许我们放弃第一次读取的成本。因此，公式调整为左输入的 2 倍加右输入的 3 倍。这一修正解决了学生中常见的误解，他们可能会认为由于流水线的作用，左输入的成本可以完全舍弃。然而，我们必须明白，只有第一次读取的代价才会被消除。在散列连接的情况下，过程涉及创建数据分区，并将其暂时存储回磁盘，然后再次读取这些分区与右输入合并。因此，我们不能完全丢弃左侧输入，只能跳过第一次读取。按照这种理解，总成本是以这些调整后的操作之和来计算的，这反映了我们对流水线条件下散列连接所涉及的成本有了更细致的了解。
When applying the hash join formula, typically we would calculate it as three times the left input plus three times the right input. However, since this plan uses a left-deep tree structure, the effect of pipelining allows us to dis🚗d the cost of the first read. Consequently, the formula adjusts to two times the left input plus three times the right input. This correction addresses a common misunderstanding among students, who might think that the left input cost can be entirely dis🚗ded due to pipelining. However, it's essential to understand that only the cost of the first read is eliminated. In the case of a hash join, the process involves creating partitions of the data, which are temporarily stored back on disk, and then those partitions are read again to merge with the right input. Therefore, we cannot fully dis🚗d the left input; only the first read is skipped. Following this understanding, the total cost is calculated as the sum of these adjusted operations, reflecting a more nuanced appreciation of the costs involved in hash joins under pipelining conditions.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" />  

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171055179.png" alt="image-20240420171055179" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171102975.png" alt="image-20240420171102975" style="zoom:50%;" /> 

$\begin{aligned}
& S: \text { NPages }(S)=500, \text { NTuplesPerPage }(S)=80 \\
& R:  \text { NPages }(R)=1000, \text { NTuplesPerPage }(R)=100 \\
& B: \text { NPages }(B)=10
\end{aligned}$​

Calculating cost: ==Cost (P3) = ? Cost (P4) = ?==

```
现在，我有一项任务交给你们。请花一点时间暂停一下，计算一下这两个计划的成本。虽然我知道这看起来很有挑战性，但参与这项练习对于巩固你们的学习成果至关重要。我们的目标是通过应用我们讨论过的概念，将你肤浅的理解转化为深刻的洞察力。因此，请继续按下暂停键，深入计算，真正掌握这些运算的机械原理。这一练习将有助于巩固我们迄今为止所涉及的知识，加深对查询优化的理解。
Now, I have a task for you. Please take a moment to pause here and calculate the cost of these two plans. While I understand it might seem challenging, engaging with this exercise is crucial for reinforcing your learning. We aim to transform your superficial understanding into profound insight by applying the concepts we've discussed. So, go ahead and press pause, delve into the calculations, and really try to grasp the mechanics of these operations. This exercise will help cement the knowledge we've covered so far and deepen your understanding of query optimization.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420235957086.png" alt="image-20240420235957086" style="zoom:50%;" /> 

```
让我们一起来完成 Petri 计划。对于这个哈希连接，我们计算的成本是左边输入的三倍加上右边输入的三倍。左边的输入是关系 S，相当于 500 的三倍。右侧输入为关系 R，则为 1000 的三倍。通常情况下，我们会使用计算器计算精确值，但为了保持讨论的流畅性，并将重点放在过程而不是具体数字上，我们暂时不进行这一步骤。这种方法有助于我们理解散列连接方案中涉及的基础成本。
Let's walk through the Petri plan together. The first calculation involves the cost of the hash join between S and R. For this hash join, we calculate the cost as three times the left input plus three times the right input. With the left input being relation S, this equates to three times 500. The right input, relation R, is three times 1000. Typically, we would calculate the precise value using a calculator, but to keep our discussion flowing and focus on the process rather than specific numbers, we'll proceed without that step for now. This approach helps us understand the foundational costs involved in this hash join scenario.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421000203229.png" alt="image-20240421000203229" style="zoom:50%;" /> 

```
这包括计算 S 中的数据元组数乘以 R 中的数据元组数，再根据 C ID 列中不同键数的倒数进行调整。这一步至关重要，因为它确定了嵌套循环连接需要处理的数据量，让我们更清楚地了解该连接对系统的计算需求。
Next, we move on to the nested loops join, but first, we need to determine the result size of the join between S and R. This involves calculating the number of tuples in S multiplied by the number of tuples in R, adjusted by the reciprocal of the number of distinct keys in the C ID column, a number we've previously computed. This step is crucial as it establishes the volume of data that the nested loops join will need to process, providing us with a clearer picture of the computational demand this join will place on the system.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421000438160.png" alt="image-20240421000438160" style="zoom:50%;" /> 

```
好了，让我们来分解一下这里的计算。我们用 40,000 乘以 100,000 再除以 40,000。这样得出的缩减系数就是 100,000 个图元。根据计算结果，一个页面可以容纳数百个这样的图元。因此，这相当于大约 1,000 页。
OK, let's break down the calculation we have here. We multiply 40,000 by 100,000 and then divide by 40,000. This results in a reduction factor that yields 100,000 tuples. According to the results, hundreds of such tuples can fit on a single page. Therefore, this equates to approximately 1,000 pages.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421000606824.png" alt="image-20240421000606824" style="zoom:50%;" /> 

```
该计算与后续嵌套循环连接的左输入有关。在正常情况下，与 B 嵌套循环连接的费用是 1,000 加 1,000 乘以 10。但是，由于这是一个左深树结构，只考虑了初始部分。因此，该操作的总费用为 10,000。
This calculation pertains to the left input for the subsequent nested loops join. Under normal circumstances, the cost of a nested loops join with B would be 1,000 plus 1,000 multiplied by 10. However, due to this being a left deep tree structure, only the initial part is considered. Consequently, the total cost of this operation amounts to 10,000.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171027821.png" alt="image-20240420171027821" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421000703912.png" alt="image-20240421000703912" style="zoom: 50%;" /> 

```
最后，整个工厂的成本 P3 是指第一部分，即这个的三倍加上 1000 的三倍，再加上这个，再加上 1000。
And then finally, cost of entire plant P three is uh the first part which is three times this plus three times 1000 then plus this, then 1000. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421000852622.png" alt="image-20240421000852622" style="zoom:50%;" /> 

```
现在让我们来看看 P4 计划，我将简要介绍一下该计划。底层保持不变。我们的成本是信噪比，这个数字我们已经遇到过三次了。结果大小也与我们之前计算的一致。为了提高效率，我将重复使用这个数字。最后，我们进行第二次哈希连接。
Let's now move on to plan P4, which I'll outline quickly. The bottom layer remains unchanged. We have the cost of SNR, a figure we've encountered three times already. The result size is also consistent with what we've previously calculated. For the sake of efficiency, I will reuse this number. Finally, we then proceed to the second hash join.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421001056367.png" alt="image-20240421001056367" style="zoom:50%;" /> 

```
好了，在第二个哈希连接（涉及流水线处理）中，与 B 相关的成本计算为左边输入大小的 2 倍加上右边输入大小的 3 倍。具体来说，就是 1,000 的 2 倍加上 10 的 3 倍。将这些相加，就是计划 P4 的最终成本。只需快速计算即可，虽然数学很简单，但对计算过程至关重要。因此，我们计算出 2 乘以 1,000 再加上 3 乘以 10，就得出了整个计划的成本。
OK, the cost associated with B in the second hash join, which involves pipeline processing, is calculated as two times the size of the left input, plus three times the size of the right input. Specifically, it's two times 1,000 plus three times 10. Summing these up gives us the final cost for plan P4. Just to quickly compute, it’s simple math but crucial to the process. Thus, we calculate two times 1,000 plus three times 10, which totals the cost of the entire plan.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171206720.png" alt="image-20240420171206720" style="zoom:50%;" /> 

$\begin{aligned}
& S: \text { NPages }(S)=500, \text { NTuplesPerPage }(S)=80 \\
& R:  \text { NPages }(R)=1000, \text { NTuplesPerPage }(R)=100 \\
& B: \text { NPages }(B)=10, \text{NTuplesPerPage(B) = 10}\\
& SMJ : \text{2 passes, RxB: 10 tuplesPerPage}\\
& I(S.sid); NPages(I) = 50\\
\end{aligned}$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171355685.png" alt="image-20240420171355685" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171402248.png" alt="image-20240420171402248" style="zoom:50%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171409373.png" alt="image-20240420171409373" style="zoom:50%;" /> 

```
好了，现在让我们来看看这三个有趣的计划。我强烈建议你深入练习这些计算。一旦你开始学习一些计划，流水线处理和丢弃等概念就会成为你的第二天性。在这里，我将介绍三个有趣的计划供大家探索。凭借目前所学到的知识，你完全有能力执行这些操作。在你尝试过这些示例后，我稍后会与你分享解决方案。请特别注意我们在使用索引扫描后进行短合并连接的情况。在本讲座的前半部分，我们已经暗示了使用索引的好处，有时基本上可以免费获得索引。我希望你能发现这个练习的吸引力，我期待着稍后与你分享结果。
OK, let's now look at these three interesting plans. I highly encourage you to delve deeper into practicing these calculations. Once you start working through a few plans, concepts like pipeline processing and dis🚗ding will become second nature to you. Here, I’m presenting three intriguing plans for you to explore. With the knowledge you've gained so far, you're well-equipped to perform these operations. I'll share solutions later, after you’ve attempted these examples. Pay particular attention to the scenario where we use an index scan followed by a short merge join. The benefits of using indexes, which can sometimes be obtained essentially for free, were hinted at earlier in this lecture. I hope you find this exercise engaging, and I look forward to sharing the results with you later.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420171431581.png" alt="image-20240420171431581" style="zoom:50%;" /> 

**1️⃣**Understand plan enumeration and cost various plans
**2️⃣**Important for Assignment 3 as well

```
最后，我们已经了解了查询计划是如何枚举和计算成本的，从而完成了对查询优化器作用的讨论。我们已经彻底了解了查询优化器的作用以及它是如何完成任务的。这些知识在你的职业生涯中将会特别有用，尤其是当你遇到运行时间过长或行为不可预测的问题查询时。在实验中，我们将教你如何检查查询计划，执行一些成本分析，并考虑其他方法来发现和解决问题。您可能会发现，建议使用不同的索引可以大大提高查询的性能，使其更高效地运行。从本质上讲，我们正在涉足数据库管理员的角色，这将是你们第三次作业的重点。感谢各位的关注，我期待着下一堂课的到来。
So, to wrap up, we've now learned how query plans are enumerated and subsequently costed, completing our discussion on the role of the query optimizer. We've thoroughly covered what it does and how it accomplishes its tasks. This knowledge will be particularly useful in your professional life, especially when you encounter problematic queries that run too long or behave unpredictably. In your labs, we will teach you how to examine the query plan, perform some cost analysis, and consider alternative approaches to identify and solve issues. You may find that suggesting a different index could dramatically enhance the query's performance, making it run much more efficiently. Essentially, we're dabbling in the role of a database administrator, and this will be the focus of your third assignment. Thank you for your attention, and I look forward to our next session.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
