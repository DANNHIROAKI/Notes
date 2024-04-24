$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404220947396.png" alt="image-20240404220947396" style="zoom: 50%;" /> 

```
欢迎回到数据库系统系列讲座的第十二讲。在今天的课程中，我们将继续探索查询处理模块。回顾一下，在上一讲中，我们深入探讨了数据库系统的执行器组件，它负责执行预定义的操作序列。我们还了解了访问路径，重点是选择和投影的执行。今天的讨论将转向连接。废话不多说，让我们进入主题。
Welcome back to the twelfth lecture in our database systems series. In today’s session, we’ll continue exploring the query processing module. To recap, in our previous lecture, we delved into the executor component of database systems, which is responsible for executing a predefined sequence of operations. We also looked at access paths, focusing on the execution of selections and projections. Today's discussion will pivot to joins. Let’s dive into this topic without further ado.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404221141035.png" alt="image-20240404221141035" style="zoom: 25%;" /> 

```
让我们来探讨一下使用假想键（我们将其称为 "ID"）连接两个表的任务。假设我们在一个表中遇到了 12 这样的 ID 值，而在另一个表中遇到了相应的匹配值。我们的目标是识别这些匹配对。一种直接的方法是按顺序扫描两张表。具体来说，我们首先检查左表中的第一条记录，然后将其与右表中的所有记录进行比较，找出匹配项。如果没有找到匹配的记录，我们就继续扫描左表中的下一条记录，并重复这一过程。例如，在检查 ID 值 "2 "时，我们会发现一条匹配记录并生成相应记录。这种方法适用于所有记录，如 "73 "和 "4"，以找到所有潜在的匹配记录。虽然这种技术很有效，但需要注意的是，它在处理大型数据集时效率较低。
Let's explore the task of joining two tables using an imaginary key, which we'll refer to as 'ID'. Suppose we encounter ID values like 12 in one table and corresponding matches in another. Our objective is to identify these matching pairs. One straightforward approach is to sequentially scan both tables. Specifically, we start by examining the first record in the left table and compare it against all records in the right table to find matches. If no match is found, we proceed to the next record in the left table and repeat the process. For instance, upon checking the ID value '2', we discover a match and generate the corresponding record. This method is applied to all records, such as '73' and '4', to find all potential matches. While this technique is effective, it's important to note its inefficiency for large datasets.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404221240442.png" alt="image-20240404221240442" style="zoom: 25%;" /> 

```
提高效率的另一种策略是对两个输入表进行排序。通过以升序排列左表和右表的数据，我们可以简化搜索过程。在对两个表进行排序后，我们可以同时对它们进行遍历，无需进行穷举比较就能迅速识别匹配项。例如，如果右表中的最小值是 "2"，而我们在左表中遇到的是 "1"，我们就可以立即得出结论，"1 "不匹配，无需进一步比较。这种方法大大减少了计算量。在两个表中都匹配到 "2 "时，我们会继续进行比较，直到比较结果超出潜在匹配范围，表明该数据段的搜索完成。此外，除了排序之外，哈希算法也能减少不必要的计算，利用哈希表加快匹配过程。
An alternative strategy to improve efficiency involves sorting both input tables. By organizing the data in ascending order for both the left and right tables, we enable a more streamlined search process. With both tables sorted, we can simultaneously traverse them, swiftly identifying matches without exhaustive comparison. For example, if the smallest value in the right table is '2' and we encounter a '1' in the left table, we can immediately conclude there's no match for '1' and proceed without further comparisons. This approach significantly reduces the computational effort. Upon matching a '2' in both tables, we continue until the comparisons exceed the range of potential matches, indicating completion of the search for that segment. Moreover, beyond sorting, implementing hashing offers a similar reduction in unnecessary computations, leveraging a hash table to expedite the matching process.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404221325899.png" alt="image-20240404221325899" style="zoom: 25%;" /> 

```
为了进一步提高搜索效率，另一种实用的技术是使用模函数进行分组。例如，应用四则运算将数字分为四类：0、1、2 和 3： 0、1、2 和 3。这种分类在比较的两边都有反映。因此，像 4 这样的数字会被放入 0 号桶，1 会被放入 1 号桶，以此类推，而 7 则会因其模乘结果而被放入 3 号桶。这种方法同样适用于右侧的数据集，建立了一组从 0 到 3 的平行数据桶。例如，在右侧数据集中，8（模 0）被分配到 0 桶，7（模 3）被分配到 3 桶，2（模 2）被分配到 2 桶，还有一个桶是空的。这种方法只对相应的数据桶进行潜在匹配比较，大大减少了不必要的比较。例如，虽然 4 和 8（在不同的数据桶中）不匹配，但在另一个数据桶中却能找到匹配，这就展示了这种方法是如何简化搜索过程的。这种高效的技术不仅适用于这项任务，在数据库操作中也很常用，本讲座将对此进行讨论。
To further enhance the search efficiency, another practical technique involves using a modulo function for bucketing. For instance, applying a modulo four operation categorizes numbers into four buckets: 0, 1, 2, and 3. This categorization is mirrored on both sides of the comparison. Consequently, a number like 4 is placed in bucket 0, 1 in bucket 1, and so on, with 7 ending up in bucket 3 due to its modulo result. This method is equally applied to the dataset on the right side, establishing a parallel set of buckets numbered 0 through 3. For example, in the right dataset, 8 (modulo 0) is allocated to bucket 0, 7 (modulo 3) to bucket 3, and 2 (modulo 2) to bucket 2, with one bucket remaining empty. By comparing only the corresponding buckets for potential matches, this approach significantly minimizes unnecessary comparisons. For instance, while 4 and 8 (in separate buckets) do not match, a match is found in another bucket, showcasing how this method streamlines the search process. Such efficient techniques are not only practical for this task but are also commonly utilized in database operations, as will be discussed in this lecture.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404221431567.png" alt="image-20240404221431567" style="zoom:50%;" /> 

```
因此，今天我们将介绍三种类型的关节点，即嵌套循环、关节点、Salmer 关节点和哈希关节点。这些内容将在数据库管理系统一书的第 14 章中介绍。
So today we will cover three types of joints, namely nested loops, joint, salmer joint, and hash joints. And these are covered in chapter 14 of our database management systems book. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404221833073.png" alt="image-20240404221833073" style="zoom: 50%;" /> 

**1️⃣**Are very common and can be veryexpensive (cross product in the worst case)

**2️⃣**There are many implementation techniques for join operations

**3️⃣**Join techniques we will cover:

1. Nested-loops join
2. Sort-merge join
3. Hash join

```
需要提醒的是，数据库之所以专注于优化连接操作，是因为连接操作本身成本很高。执行交叉乘积（实质上是将每个元素与其他元素结合在一起）的效率非常低，尤其是对于大规模数据库而言。为了降低这种低效率，数据库采用了各种优化技术，我们今天就来探讨一下。这些技术包括排序、散列和循环等。所讨论的每个示例都旨在促使人们思考这些策略如何能显著降低计算开销，从而突出优化在数据库管理中的重要性。
As a reminder, the reason why databases focus on optimizing join operations is due to their inherently high cost. Performing a cross product, which essentially combines each element with every other element, proves to be highly inefficient, particularly for large-scale databases. To mitigate this inefficiency, databases employ various optimization techniques, as we will explore today. These techniques include sorting, hashing, and looping, among others. Each example discussed is designed to prompt consideration of how these strategies can significantly reduce computational overhead, highlighting the importance of optimization in database management.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404222936219.png" alt="image-20240404222936219" style="zoom: 50%;" /> 

Example:

```sql
SELECT * FROM Reserves R1, Sailors S1 WHERE R1.sid=S1.sid
```

**1️⃣**In algebra: $R\bowtie{S}$ They are very common and need to be 🚗efully optimized.

**2️⃣**R X S is large; so, R X S followed by a selection is inefficient.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404223237248.png" alt="image-20240404223237248" style="zoom:50%;" /> 

**3️⃣**Join is associative and commutative:

1. AxB== BxA
2. Ax(BxC)==(AxB)xC

**4️⃣**Cost metric : Number of pages; Number of I/O

```
在深入研究 SQL 和关系代数领域时，表示连接操作的方法至关重要。有几种方法可以表达连接：使用自然连接、内部连接，或者像屏幕上显示的那样，列出要连接的表，用逗号分隔，并在 WHERE 子句中指定连接条件。虽然后者可能类似于交叉乘积，但数据库在解析阶段会将其解释为自然连接，尤其是当连接的表中有共同属性时--在我们的示例中表示为 sid。
When delving into the realm of SQL and relational algebra, the way we notate a join operation is crucial. There are a few methods to express joins: using a natural join, an inner join, or as displayed on the screen, by listing the tables to be joined, separated by a comma, and specifying the join condition in the WHERE clause. Though the latter may resemble a cross product, databases interpret this during the parsing phase as a natural join, particularly when there's a common attribute in the joined tables—denoted as sid in our example.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404222936219.png" alt="image-20240404222936219" style="zoom: 50%;" /> 

Example:

```sql
SELECT * FROM Reserves R1, Sailors S1 WHERE R1.sid=S1.sid
```

**1️⃣**In algebra: $R\bowtie{S}$ They are very common and need to be 🚗efully optimized.

**2️⃣**R X S is large; so, R X S followed by a selection is inefficient.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404223237248.png" alt="image-20240404223237248" style="zoom:50%;" /> 

**3️⃣**Join is associative and commutative:

1. AxB== BxA
2. Ax(BxC)==(AxB)xC

**4️⃣**Cost metric : Number of pages; Number of I/O

```
最常见的连接类型是表共享一个共同属性，关系代数用 R 和 S 之间的交集符号来表示。潜在的低效源于这样一个事实，即 R x S 表示的交乘会产生大量输出，从而使操作成本高昂。然而，自然连接实质上是一种复合运算符，理论上可以通过交叉乘积、选择和投影的序列来实现，从而简化了连接过程。
The most prevalent type of join is the one where tables share a common attribute, which relational algebra denotes with an intersection symbol between R and S. Given its frequency, database systems are heavily optimized for this type of join. The potential inefficiency stems from the fact that the cross product—indicated by R x S—tends to yield an extensive output, making it a costly operation. However, natural joins streamline the process by essentially acting as a composite operator, theoretically achievable through a sequence of a cross product, followed by selection and projection.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404222936219.png" alt="image-20240404222936219" style="zoom: 50%;" /> 

Example:

```sql
SELECT * FROM Reserves R1, Sailors S1 WHERE R1.sid=S1.sid
```

**1️⃣**In algebra: $R\bowtie{S}$ They are very common and need to be 🚗efully optimized.

**2️⃣**R X S is large; so, R X S followed by a selection is inefficient.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404223237248.png" alt="image-20240404223237248" style="zoom:50%;" /> 

**3️⃣**Join is associative and commutative:

1. AxB== BxA
2. Ax(BxC)==(AxB)xC

**4️⃣**Cost metric : Number of pages; Number of I/O

```
在连接操作中，"左 "和 "右 "输入具有特定的含义。左 "输入（在本例中为 R）被称为 "外 "表，而 "右 "输入（S）被称为连接的 "内 "表。重要的是，不要把这个术语与连接的类型（如内连接或左/右外连接）混为一谈；在这里，"外 "和 "内 "只是区分表在连接语法中的位置。
In the context of join operations, the terms 'left' and 'right' inputs acquire specific meanings. The 'left' input, or R in our case, is referred to as the 'outer' table, and the 'right' input, S, is known as the 'inner' table of the join. It's important not to conflate this terminology with the types of joins, such as inner joins or left/right outer joins; here, 'outer' and 'inner' simply distinguish the position of tables in the join syntax.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404222936219.png" alt="image-20240404222936219" style="zoom: 50%;" /> 

Example:

```sql
SELECT * FROM Reserves R1, Sailors S1 WHERE R1.sid=S1.sid
```

**1️⃣**In algebra: $R\bowtie{S}$ They are very common and need to be 🚗efully optimized.

**2️⃣**R X S is large; so, R X S followed by a selection is inefficient.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404223237248.png" alt="image-20240404223237248" style="zoom:50%;" /> 

**3️⃣**Join is associative and commutative:

1. AxB== BxA
2. Ax(BxC)==(AxB)xC

**4️⃣**Cost metric : Number of pages; Number of I/O

```
联接本身具有关联性和交换性，这意味着操作中表的顺序不会影响结果，A 与 B 的联接结果与 B 与 A 的联接结果相同。这种关联特性允许数据库灵活地重新安排连接操作，以构建最有效的执行计划。最后，这些操作的成本总是以所涉及的页面数或 I/O 操作数来量化，这也是我们评估连接性能的一贯标准。
Joins are inherently associative and commutative, meaning the order of tables in the operation doesn't affect the result—A joined with B will yield the same result as B joined with A. Likewise, when dealing with multiple joins, the sequence of operations can be varied without altering the outcome. This associative property allows databases the flexibility to rearrange join operations to construct the most efficient execution plan. In the end, the cost of these operations is invariably quantified by the number of pages or I/O operations involved, which remains our consistent metric for assessing the performance of joins.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224211936.png" alt="image-20240404224211936" style="zoom:50%;" /> 

Sailors (sid: integer, sname: string, rating: integer, age: real)
Reserves (sid: integer, bid: integer, day: dates, rname: string)

**1️⃣**Sailors (S):

1. 80 tuples per page, 500 pages
2. NPages(S) = 500, NTuplesPerPage(S) = 80
3. NTuples(S) = 500*80 = 40000

**4️⃣**Reserves (R):

1. 100 tuples per page, 1000 pages
2. NPages(R) = 1000, NTuplesPerPage(R) =100
3. NTuples(R) = 100000

```
因此，我将采用我在过去几次关于水兵和后备役的讲座中使用的相同方案，即水兵有 500 页，后备役有 1000 页。
So I will use the same scheme I used in the past couple of lectures of sailors and reserves where we have 500 pages in sailors and 1000 pages in reserves. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404222936219.png" alt="image-20240404222936219" style="zoom: 50%;" /> 

Example:

```sql
SELECT * FROM Reserves R1, Sailors S1 WHERE R1.sid=S1.sid
```

**1️⃣**In algebra: $R\bowtie{S}$ They are very common and need to be 🚗efully optimized.

**2️⃣**R X S is large; so, R X S followed by a selection is inefficient.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404223237248.png" alt="image-20240404223237248" style="zoom:50%;" /> 

**3️⃣**Join is associative and commutative:

1. AxB== BxA
2. Ax(BxC)==(AxB)xC

**4️⃣**Cost metric : Number of pages; Number of I/O

```
提醒一下，当我们使用自然连接、等价连接时，我们会查找特定的列来搜索匹配记录。因此，想象一下在这种情况下，我在第一列上进行连接，然后搜索匹配的记录。
And to remind you when we are joining those with natural join, equi join, we will look for a particular column where we are searching for the matches. So imagine in this case, uh I'm joining over the first column and searching for the matching records. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224613298.png" alt="image-20240404224613298" style="zoom:50%;" /> 

**1️⃣**For each tuple in the outerrelation R, we scan the entire innerrelation S

Pseudo code:(伪代码)

```pseudocode
foreachtuple r in Rdo
	foreachtuples in Sdo
		if ri== sjthen add <r, s> to result
```

![image-20240404224709718](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224709718.png)  

**2️⃣**Cost:

```sql
Cost (SNJL) = NPages(Outer) + NTuples(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (SNLJ)= 1000+100*1000*500 = 50001000 (I/O)
```

```
简单的嵌套循环连接是一种基本但重要的数据库操作。这个过程需要扫描 "外层 "关系中的每个元组，然后与 "内层 "关系中的每个元组进行比较，找出匹配的元组。在伪代码中，这表示为两个嵌套循环，其中外层关系 R 中的每个元组 r 都与内层关系 S 中的每个元组 s 进行比较。
The simple nested loops join is an elementary but crucial database operation. The process entails scanning each tuple in the 'outer' relation, which is then compared against every tuple in the 'inner' relation to find matches. In pseudocode, this is represented as two nested loops where each tuple r in the outer relation R is compared with each tuple s in the inner relation S. If a match is found according to the join condition, the tuple pair <r, s> is added to the result set.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224613298.png" alt="image-20240404224613298" style="zoom:50%;" /> 

**1️⃣**For each tuple in the outerrelation R, we scan the entire innerrelation S

Pseudo code:(伪代码)

```pseudocode
foreachtuple r in Rdo
	foreachtuples in Sdo
		if ri== sjthen add <r, s> to result
```

![image-20240404224709718](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224709718.png)  

**2️⃣**Cost:

```sql
Cost (SNJL) = NPages(Outer) + NTuples(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (SNLJ)= 1000+100*1000*500 = 50001000 (I/O)
```

```
为了直观地说明这一点，请看左侧输入（以蓝色表示，标记为 R），每条记录都要与右侧输入（即 "内部 "输入）中的所有记录进行核对。这一操作会重复进行，依次检查左侧输入中的每个元组，并对每个元组的右侧输入进行全面扫描。
To visualize this, consider the left input, depicted in blue and labeled as R, where each record is checked against all the records in the right input, the 'inner' input. This operation is repeated, progressing sequentially through each tuple of the left input and conducting a full scan of the right input for each one.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224613298.png" alt="image-20240404224613298" style="zoom:50%;" /> 

**1️⃣**For each tuple in the outerrelation R, we scan the entire innerrelation S

Pseudo code:(伪代码)

```pseudocode
foreachtuple r in Rdo
	foreachtuples in Sdo
		if ri== sjthen add <r, s> to result
```

![image-20240404224709718](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404224709718.png)  

**2️⃣**Cost:

```sql
Cost (SNJL) = NPages(Outer) + NTuples(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (SNLJ)= 1000+100*1000*500 = 50001000 (I/O)
```

```
这种连接的成本指标由从磁盘读取的页数决定。左侧输入（即外部关系）会被精确遍历一次，相当于其总页数。至于右边的输入，即内部关系，则要对外部关系的每个元组遍历一次。在数学上，这表示为外部关系中的元组数乘以内部关系中的页数。在我们的示例中，外层关系中有 1000 个页面，内层关系中有 500 个页面，外层关系中有 100 个元组，输入这些数字后，计算结果显示 I/O 操作的总成本为 5000 万次，这反映了这种连接方法的潜在高成本。
The cost metric for this type of join is determined by the number of page reads from disk. The left input, or the outer relation, is traversed exactly once, which equates to the total number of its pages. As for the right input, or the inner relation, it is traversed once for each tuple of the outer relation. Mathematically, this is expressed as the number of tuples in the outer relation multiplied by the number of pages in the inner relation. Plugging in the numbers from our example, with 1000 pages in the outer relation and 500 pages in the inner, and 100 tuples in the outer relation, results in a computation that indicates a total cost of 50 million I/O operations, reflecting the potentially high cost of this join method.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225314265.png" alt="image-20240404225314265" style="zoom:50%;" /> 

**1️⃣**For each page of R

1. get each page of S
2. write out matching pairs of tuples <r, s>, where r is in R-page and S is in S-page

**2️⃣**Pseudo code:

![image-20240404225505833](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225505833.png) 

```pseudocode
foreach page bRin R do
	foreach page bS in S do
		foreach tuple r in bR do
			foreach tuple s in bS do
				if ri== sj then add <r, s> to result
```

```sql
Cost (PNJL) = NPages(Outer) + NPages(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (PNLJ)= 1000+1000*500= 501000 (I/O)
```
```
为了优化连接操作，我们可以采用面向页面的方法，而不是逐个元组迭代的方法。这种方法利用了这样一个事实，即当一条记录被检索到内存中时，包含该记录的整个页面都会被访问。因此，"面向页面的嵌套循环连接 "利用了这一点，将左侧输入取回的页面中的所有记录与右侧输入相应页面中的所有记录进行检查。这种逐页比较的过程一直持续到检查完左侧输入的所有页面为止。
In an effort to optimize the join operation, we can adopt a page-oriented approach rather than tuple-by-tuple iteration. This method leverages the fact that when a single record is retrieved into memory, the entire page containing that record is accessed. Therefore, the Page-Oriented Nested Loops Join capitalizes on this by checking all records within the fetched page of the left input against all records in the corresponding page of the right input. This page-by-page comparison proceeds until all pages from the left input have been examined.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225314265.png" alt="image-20240404225314265" style="zoom:50%;" /> 

**1️⃣**For each page of R

1. get each page of S
2. write out matching pairs of tuples <r, s>, where r is in R-page and S is in S-page

**2️⃣**Pseudo code:

![image-20240404225505833](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225505833.png) 

```pseudocode
foreach page bRin R do
	foreach page bS in S do
		foreach tuple r in bR do
			foreach tuple s in bS do
				if ri== sj then add <r, s> to result
```

```sql
Cost (PNJL) = NPages(Outer) + NPages(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (PNLJ)= 1000+1000*500= 501000 (I/O)
```
```
下面的伪代码演示了这一过程：对于左输入 R 中的每一页，右输入 S 中的每一页都会被带入内存。在这些页面中，我们会遍历每个元组，将 R 中的元组与 S 中的元组进行比较。
The pseudocode demonstrates this process: for each page in the left input R, every page in the right input S is brought into memory. Within these pages, we then iterate through each tuple, comparing tuples from R to those in S. When a match is found, the tuple pair is added to the result set.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225314265.png" alt="image-20240404225314265" style="zoom:50%;" /> 

**1️⃣**For each page of R

1. get each page of S
2. write out matching pairs of tuples <r, s>, where r is in R-page and S is in S-page

**2️⃣**Pseudo code:

![image-20240404225505833](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404225505833.png) 

```pseudocode
foreach page bRin R do
	foreach page bS in S do
		foreach tuple r in bR do
			foreach tuple s in bS do
				if ri== sj then add <r, s> to result
```

```sql
Cost (PNJL) = NPages(Outer) + NPages(Outer) * NPages(Inner)
```

**3️⃣**Our example:

```sql
Cost (PNLJ)= 1000+1000*500= 501000 (I/O)
```
```
在评估这种方法的成本时，以页面为导向的嵌套循环连接显然有很大的改进。左侧输入 R 仍会被遍历一次，相当于其页面数。但是，右侧输入 S 的遍历次数与左侧输入的页数相同，而不是其图元数。在我们的示例中，R 中有 1000 个页面，S 中有 500 个页面，因此 I/O 操作的成本为 501,000 次，比简单嵌套循环连接的成本大幅降低。这种效率的提高得益于在进入下一个页面之前，对所访问页面中的所有记录进行了策略性检查，从而最大限度地减少了所需的页面访问总数。
When evaluating the cost of this approach, it becomes clear that the Page-Oriented Nested Loops Join offers a significant improvement. The left input,  R , is still traversed once, equating to the number of its pages. However, the right input,  S , is traversed as many times as there are pages in the left input, rather than its tuples. Applying this to our example, with 1000 pages in  R  and 500 in  S , results in a cost of 501,000 I/O operations, a drastic reduction from the cost of a simple nested loops join. This efficiency gain is due to the strategic check of all records within an accessed page before proceeding to the next, hence minimizing the total number of page accesses required.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230005157.png" alt="image-20240404230005157" style="zoom:33%;" /> 

```
让我们总结一下到目前为止的讨论。想象一下，我们有两个输入：一个在这里，用一种颜色标记；另一个在这里，用另一种颜色标记。我们的内存比两个输入都要小，所有的处理都在内存中进行。为了执行连接，我们从左侧输入端和右侧输入端各引入一页，确保有一页留给结果。在这个空间中，我们检查两个输入的所有记录，并将结果存储在结果页中。这描述了面向页面的嵌套循环方法，该方法每次处理来自两个输入的一页数据。
Let's summarize our discussion so far. Imagine we have two inputs: one here, marked in one color, and another here, marked in a different color. Our memory, which is smaller than both inputs, is where all processing occurs. To perform a join, we bring in one page from the left input and one page from the right input, ensuring we have a page reserved for results. In this space, we examine all records from both inputs, storing the outcomes in the results page. This describes the page-oriented nested loops approach, which processes data one page at a time from both inputs.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230124884.png" alt=" " style="zoom:33%;" /> 

```
在现实中，我们通常拥有更多的可用内存，而不是仅够存储左侧输入和右侧输入的单个页面。我们要讨论的下一种连接方式就利用了这一事实，利用整个可用内存空间来提高效率。我们仍将为结果和右输入各保留一页，但将对方法进行优化，以充分利用额外的内存空间。
In reality, we typically have more memory available than just enough to store a single page from the left input and one from the right. The next type of join we will discuss takes advantage of this fact, utilizing the entire available memory space for improved efficiency. We will still reserve one page for the result and one page for the right input, but the approach will be optimized to make full use of the additional memory space.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230424907.png" alt="image-20240404230424907" style="zoom: 33%;" /> 

```
对于左侧输入，我们会在空间允许的情况下在内存中分配尽可能多的页面。想象一下，我们已经存储了前四页，为了让场景更有趣，我们假设还有更多可用页面。将这四页加载到内存后，我们将逐页处理左侧输入。对于放入内存的每一页，我们都要将其与已存储在内存中的所有页面进行比较。完成这些比较后，我们将继续插入下一页，并再次与内存中的所有页面进行比较，然后再继续处理。
For the left input, we allocate as many pages in memory as space permits. Imagine we've stored the first four pages, and to make the scenario more intriguing, let's say there are more pages available. Once these four pages are loaded into memory, we process the left input page by page. For each page placed in memory, we compare it against all pages already stored there. After completing these comparisons, we move on to insert the next page, again comparing it against all pages in memory before proceeding further.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230744120.png" alt="image-20240404230744120" style="zoom: 33%;" /> 

```
在完成对整个右侧输入的一次传递后，我们可以丢弃内存中的现有页面。这些页面可以返回到它们的源头，这样我们就可以获取下一个大的页面块，并将它们存储在内存中进行处理。这个过程包括根据左侧输入依次检查每个新页面，从第一页开始，然后再检查后续页面。通过这种方法，我们可以大大减少对右侧输入所需的检查次数。在我的例子中，我们只需要对右侧输入进行两次检查，因为我们可以一次性在内存中存储一大块内容，从而更高效地完成检查。
After completing a single pass over the entire right input, we can dis🚗d the existing pages from memory. These can be returned to their source, allowing us to fetch the next large block of pages and store them in memory for processing. This process involves sequentially checking each new page against the left input, starting from the first page and moving on to subsequent pages. By doing this, we significantly reduce the number of passes required over the right input. In my example, we only need two passes over the right input because we can store a large chunk of it in memory at once, thus completing the pass more efficiently.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230824781.png" alt="image-20240404230824781" style="zoom:33%;" /> 

```
在内存中存储下一个大块后，考虑到我们总共有 8 个页面，我们就结束了处理过程。这样，我们只需对正确的输入进行两次传递就完成了处理。这种效率是我们下一个连接方法的基石，它利用这种方法优化了数据处理。
After storing the next large chunk in memory and considering that we have a total of eight pages, we conclude the process. Thus, we complete our processing with just two passes over the right input. This efficiency is the cornerstone of our next join method, which leverages this approach to optimize data processing.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230919992.png" alt="image-20240404230919992" style="zoom:50%;" /> 

**1️⃣**Page-oriented NL doesn'texploit extra memory buffers

**2️⃣**Alternative approach:

- Use one page as an input buffer for scanning the inner S, one page as the output buffer, and use all remaining pages to hold ‘block’of outer R

**3️⃣**For each matching tuple r in R-block, s in S-page, add <r, s> to result. Then read next R-block, scan S, etc

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231011499.png" alt="image-20240404231011499" style="zoom:67%;" /> 

```
我们探索的下一个技术是块嵌套循环连接，旨在最大限度地利用可用内存。这种方法是尽可能多地存储左侧输入的页面，同时为右侧输入的一个页面和输出的另一个页面预留内存。假设我们的数据库关系是存储在磁盘上的 R 和 S，可用内存为 B 页，那么我们为输入 R 分配 B 减 2 页。这种循环确保了内存的有效利用，总有一个插槽专门用于累积连接结果。如果输出结果超出了分配的页面，则会无缝写入磁盘，从而确保持续处理而不会出现内存溢出。
The next technique we explore is the Block Nested Loops Join, designed to maximize the use of available memory. This method involves storing as many pages from the left input as possible while reserving memory for one page from the right input and another for the output. Assuming our database relations are R and S stored on disk, and the available memory is B pages, we allocate B minus two pages for the input R. We process the input S one page at a time—loading a page, using it for joins, then removing it to make room for the next. This cycle ensures efficient use of memory, with one slot always dedicated to accumulating the join results. Should the output exceed the allocated page, it is seamlessly written to disk, ensuring continuous processing without memory overflow.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231122285.png" alt="image-20240404231122285" style="zoom: 50%;" /> 

```sql
Cost (BNJL) = NPages(Outer) + NBlocks(Outer) * NPages(Inner)
```

![image-20240404231157593](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231157593.png) 

**1️⃣**NBlocks(Outer) = $[\cfrac{NPages(Outer)}{B-2}]$

**2️⃣**Our example:

Let’s say we have 102 pages of space in memory, and consider Reserves (R) as the outer and Sailors (S) as the inner table.

```sql
NBlocks(R) = 1000/(102-2) = 10
Cost(BNLJ) = 1000+ 10* 500= 6000 I/O
```

```
块嵌套循环连接的成本公式考虑了左输入的每个块都需要遍历整个右输入的必要性。因此，计算公式为左输入的页数加上左输入的块数与右输入的页数的乘积。例如，如果我们有两个区块用于遍历关系 S，那么我们计算的区块数就是左侧关系的总页数除以 B 再减去 2。这一推导考虑了为右侧输入和输出各保留一个页面的情况，解释了如何通过 B 减 2 来有效地确定区块数。
The cost formula for the Block Nested Loops Join accounts for the necessity to traverse the entire right input for each block of the left input. Thus, the formula is the number of pages in the left input plus the product of the number of blocks in the left input and the number of pages in the right input. For example, if we have two blocks for traversing relation S, we calculate the number of blocks as the total number of pages in the left relation divided by B minus two. This deduction accounts for the pages reserved for one input from the right and one for the output, explaining the division by B minus two to determine the block count efficiently.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404230919992.png" alt="image-20240404230919992" style="zoom:50%;" /> 

**1️⃣**Page-oriented NL doesn'texploit extra memory buffers

**2️⃣**Alternative approach:

- Use one page as an input buffer for scanning the inner S, one page as the output buffer, and use all remaining pages to hold ‘block’of outer R

**3️⃣**For each matching tuple r in R-block, s in S-page, add <r, s> to result. Then read next R-block, scan S, etc

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231011499.png" alt="image-20240404231011499" style="zoom:67%;" />  

```
如果我们考虑到关系 R 由 10 页组成，而我们的内存容量在考虑了右输入和右输出各一页（B 减 2）后，允许一次存储 5 页关系 R，那么我们最终会有两个区块（10 除以 5）。这就导致我们基本上需要对右侧输入进行两次传递，对应于关系 R 的两个处理块。
If we consider a scenario where relation R consists of 10 pages and our memory capacity, after accounting for one page each for the right input and output (B minus 2), allows for storing 5 pages of relation R at a time, we end up with two blocks (10 divided by 5). This results in essentially requiring two passes over the right input, corresponding to the two blocks of relation R being processed.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231122285.png" alt="image-20240404231122285" style="zoom: 50%;" /> 

```sql
Cost (BNJL) = NPages(Outer) + NBlocks(Outer) * NPages(Inner)
```

![image-20240404231157593](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231157593.png) 

**1️⃣**NBlocks(Outer) = $[\cfrac{NPages(Outer)}{B-2}]$

**2️⃣**Our example:

Let’s say we have 102 pages of space in memory, and consider Reserves (R) as the outer and Sailors (S) as the inner table.

```sql
NBlocks(R) = 1000/(102-2) = 10
Cost(BNLJ) = 1000+ 10* 500= 6000 I/O
```

```
在这个示例中，我们将在有 102 页可用内存的情况下检查 "块嵌套循环连接"。我们将其应用到之前涉及 "储备"（R）和 "水手"（S）关系的案例研究中。为了启动计算，我们首先确定关系 R 所需的块数，计算公式为总页数（1000）除以可用内存页数减 2，得出 10 个块。
In this example, we examine the Block Nested Loops Join given a scenario where we have 102 pages of memory available, a number selected for simplicity. We apply this to our previous case study involving the 'Reserves' (R) and 'Sailors' (S) relations. To initiate the computation, we first determine the number of blocks required for relation R, which can be calculated as the total number of pages (1000) divided by the number of available memory pages minus two, resulting in 10 blocks.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231122285.png" alt="image-20240404231122285" style="zoom: 50%;" /> 

```sql
Cost (BNJL) = NPages(Outer) + NBlocks(Outer) * NPages(Inner)
```

![image-20240404231157593](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231157593.png) 

**1️⃣**NBlocks(Outer) = $[\cfrac{NPages(Outer)}{B-2}]$

**2️⃣**Our example:

Let’s say we have 102 pages of space in memory, and consider Reserves (R) as the outer and Sailors (S) as the inner table.

```sql
NBlocks(R) = 1000/(102-2) = 10
Cost(BNLJ) = 1000+ 10* 500= 6000 I/O
```

```
在确定关系 R 有 10 个数据块后，我们对每个数据块与整个关系 S 进行一次迭代。这一操作会产生一个连接成本公式：左侧输入的大小（1000 页）加上区块数（10）和右侧输入的大小（500 页）的乘积。因此，I/O 操作总成本为 6000 次页面访问。这一数字大大低于我们最初示例中令人望而却步的 5000 万次 I/O 操作的成本。这种优化有效地证明了通过减少对数据的访问次数可以获得显著的性能提升--这是数据库操作中至关重要的优化，可以将查询响应时间从数天缩短到数秒，而这正是数据库性能调整的最终目标。
Upon establishing that there are 10 blocks for relation R, we proceed by iterating over each block against the entire relation S once. This operation yields a formula for the join cost: the size of the left input (1000 pages) added to the product of the number of blocks (10) and the size of the right input (500 pages). The total I/O operation cost is, therefore, 6,000 page accesses. This figure is drastically lower than the prohibitive cost of 50 million I/O operations from our original example. Such optimization effectively demonstrates the significant performance gains that can be achieved by reducing the number of passes over the data—a crucial optimization in database operations that can turn query response times from days into seconds, which is the ultimate goal in database performance tuning.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231631994.png" alt="image-20240404231631994" style="zoom:50%;" /> 

```
在探讨了对两个输入关系进行循环的概念后，我们现在来看看前面简要提到的第二种方法：对两个输入关系中的记录进行排序。这种策略使我们只需遍历两个关系一次，通过按预定顺序排列记录来简化流程，从而提高比较和匹配的效率。
Having explored the concept of looping through both inputs, we now turn to a second approach briefly mentioned earlier: sorting records in both inputs. This strategy enables us to traverse both relations just once, streamlining the process by aligning the records in a predetermined order to facilitate more efficient comparison and matching.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231751282.png" alt="image-20240404231751282" style="zoom:50%;" /> 

**1️⃣****Sort** R and S on the join column, then scan them to do a **merge**(on join column), and output result tuples

![image-20240404231818521](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231818521.png) 

**2️⃣**Sorted R is scanned once;

**3️⃣**Each S group of the same key values is scanned once per matching R tuple (typically means Sorted S is scanned once too).

**4️⃣**Useful when:

1. one or both inputs are already sorted on join attribute(s)
2. output is required to be sorted on join attributes(s)

```
排序-合并连接算法的运行原理简单而有效。首先，它根据连接键对 R 和 S 这两个输入进行排序。然后，它同时遍历两个已排序的输入，寻找匹配的键。找到匹配后，相应的图元就会被连接起来。这个过程会依次进行，直到两个输入中的所有元素都完全比较过一次为止。这种方法的高效之处在于，一旦两个输入都排序完毕，就可以对其进行单程操作，无需嵌套循环，因此是一种非常精简的方法。
The sort-merge join algorithm operates on a simple yet effective principle. Firstly, it sorts both inputs, R and S, based on the join key. Then, it concurrently traverses both sorted inputs, looking for matching keys. When a match is found, the corresponding tuples are joined. This process continues sequentially until all elements of both inputs have been compared exactly once. The efficiency of this method lies in its single-pass operation over both inputs once they are sorted—there's no need for nested looping, making it a very streamlined approach.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231751282.png" alt="image-20240404231751282" style="zoom:50%;" /> 

**1️⃣****Sort** R and S on the join column, then scan them to do a **merge**(on join column), and output result tuples

![image-20240404231818521](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404231818521.png) 

**2️⃣**Sorted R is scanned once;

**3️⃣**Each S group of the same key values is scanned once per matching R tuple (typically means Sorted S is scanned once too).

**4️⃣**Useful when:

1. one or both inputs are already sorted on join attribute(s)
2. output is required to be sorted on join attributes(s)

```
不过，初始排序步骤可能会耗费大量资源。我们将详细探讨相关的成本，但需要注意的是，现实世界中的数据库系统只有在特定条件下才会选择排序合并连接。例如，当其中一个输入已经排序，或者连接列上有索引时，这种方法就会受到青睐。特别是 B+ 树索引，可以方便地按排序顺序读取数据。此外，如果先前操作的输出已排序，排序-合并连接也是一种可行的选择。最终，数据库会对这些因素进行评估，以决定排序合并连接是否是最佳策略。
However, the initial sorting step can be resource-intensive. The associated costs will be explored in detail, but it's important to note that real-world database systems might only opt for a sort-merge join under certain conditions. For example, this approach is favored when one of the inputs is already sorted, or there's an index on the join column. A B+ tree index, specifically, can facilitate reading data in a sorted sequence. Additionally, if the output from a prior operation is sorted, a sort-merge join becomes a viable option. Ultimately, databases will evaluate these factors to decide if a sort-merge join is the optimal strategy.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404234728062.png" alt="image-20240404234728062" style="zoom:50%;" /> 

```sql
Cost (SMJ) = Sort(Outer) + Sort(Inner) + NPages(Outer) + NPages(Inner)
-- Sort inputs Merge inputs
Sort(R) = External Sort Cost = 2*NumPasses*NPages(R)
```

- Our example: Let’s say that both Reserves and Sailors can be sorted in 2 passes, then:

```sql
Cost(SMJ) = Sort R + Sort S + NPages(R) + NPages(S)
			 = 2*2*NPages(R)+ 2*2*NPages(S)
			 + NPages(R) + NPages(S)
			 = 5*1000 + 5* 500 = 7500 I/O
```

```
执行排序合并连接的全部成本取决于外部输入和内部输入的排序总和，以及处理排序后的匹配输入所需的页面读取次数。如前所述，这种排序费用与外部排序费用相同。数据库一直使用外部排序算法进行排序。计算公式概括地说，就是通过次数与关系页数乘积的两倍。2 的系数考虑了排序过程中的读写操作。
The full cost of executing a sort-merge join is determined by the sum of sorting both the outer and inner inputs and then the number of page reads required to process the sorted inputs for matches. This sorting expense is identical to the external sort cost, as previously discussed. Databases consistently utilize the external sort algorithm for sorting purposes. The formula for calculating this, to recap, is twice the product of the number of passes and the number of pages of the relation. The factor of two accounts for both reading and writing operations during the sorting process.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404234728062.png" alt="image-20240404234728062" style="zoom:50%;" /> 

```sql
Cost (SMJ) = Sort(Outer) + Sort(Inner) + NPages(Outer) + NPages(Inner)
-- Sort inputs Merge inputs
Sort(R) = External Sort Cost = 2*NumPasses*NPages(R)
```

- Our example: Let’s say that both Reserves and Sailors can be sorted in 2 passes, then:

```sql
Cost(SMJ) = Sort R + Sort S + NPages(R) + NPages(S)
			 = 2*2*NPages(R)+ 2*2*NPages(S)
			 + NPages(R) + NPages(S)
			 = 5*1000 + 5* 500 = 7500 I/O
```

```
让我们重温一下最初涉及 "预备役 "和 "水手 "数据集的例子。假设这两个数据集只需通过两次就能高效排序，这就简化了我们的计算。需要注意的是，排序次数的确定是基于更复杂的数学计算，但一旦确定，这些数字就可以直接插入公式中。每个数据集的排序成本计算为通过次数的 2 倍乘以相应数据集的页数。排序完成后，我们会将两个数据集的页数相加，以计算合并阶段和生成结果集的费用。
Let's revisit our initial example involving the 'Reserves' and 'Sailors' datasets. Suppose both datasets can be efficiently sorted in just two passes, which simplifies our computation. It's important to recognize that determining the number of passes is based on more complex mathematics, but once established, these figures are plugged directly into the formula. The sorting cost for each dataset is calculated as two times the number of passes times the number of pages in the respective dataset. After sorting, we add the number of pages from both datasets to account for the merge phase and the production of the result set.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404234728062.png" alt="image-20240404234728062" style="zoom:50%;" /> 

```sql
Cost (SMJ) = Sort(Outer) + Sort(Inner) + NPages(Outer) + NPages(Inner)
-- Sort inputs Merge inputs
Sort(R) = External Sort Cost = 2*NumPasses*NPages(R)
```

- Our example: Let’s say that both Reserves and Sailors can be sorted in 2 passes, then:

```sql
Cost(SMJ) = Sort R + Sort S + NPages(R) + NPages(S)
			 = 2*2*NPages(R)+ 2*2*NPages(S)
			 + NPages(R) + NPages(S)
			 = 5*1000 + 5* 500 = 7500 I/O
```

```
这样累计计算的结果是，输入共经过五次，从而得出本例的输入/输出成本为 7,500 美元。需要特别提醒的是，根据我们的例子得出的 5 倍乘数不应被视为标准系数。实际的计算公式因所需通过次数而异，根据相关数据集，可能是一次、三次、四次或任何其他次数。因此，了解基本公式及其应用而不是记住任何具体的数字示例至关重要。
This cumulative calculation results in a total of five passes over the inputs, leading to the specific example's I/O cost of 7,500. As a critical reminder, the multiplier of five, derived from our example, should not be taken as a standard coefficient. The actual formula varies depending on the number of passes required, which could be one, three, four, or any other number based on the datasets in question. Therefore, it's essential to understand the underlying formula and its application rather than memorizing any specific numerical examples.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235151177.png" alt="image-20240404235151177" style="zoom:50%;" /> 

```
最后，我们探讨了散列连接的概念，它将数据从分类转为散列。通过对两个输入应用相同的哈希函数，我们将数据归类到相应的桶中。这确保了如果存在匹配，则会在同一数据桶中找到，从而使我们能够绕过大量不必要的页面检查。这种方法只关注潜在的匹配，简化了连接过程，从而大大提高了效率。
Finally, we explore the concept of Hash Joins, which pivot from sorting to hashing data. By applying the same hash function to both inputs, we categorize data into corresponding buckets. This ensures that if a match exists, it will be found within the same bucket, allowing us to bypass a significant number of unnecessary page checks. This method significantly enhances efficiency by focusing only on potential matches, streamlining the join process.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235318787.png" alt="image-20240404235318787" style="zoom:50%;" /> 

**1️⃣**Partition both relations using hash function h: R tuples in partition l will onlymatch S tuples in partition I

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235347620.png" alt="image-20240404235347620" style="zoom:50%;" /> 

```
哈希连接技术首先应用哈希函数（记为 h）将输入关系 R 和 S 分割成磁盘上的相应部分。在此过程中，给定分区 I 中关系 R 的每个元组将完全匹配关系 S 的相应分区 I 中的元组。这一步骤确保只需要比较相关分区，从而大大减少了潜在匹配的搜索空间。
The hash-join technique begins by applying a hash function, denoted as h, to partition both input relations R and S into corresponding segments on the disk. In this process, each tuple from relation R within a given partition I will exclusively match tuples in the corresponding partition I of relation S. This step ensures that only relevant partitions need to be compared, significantly reducing the search space for potential matches.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235318787.png" alt="image-20240404235318787" style="zoom:50%;" /> 

**2️⃣**Read in a partition of R, hash it using h2 (<> h!). Scan matching partition of S, probe hash table for matches

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235406468.png" alt="image-20240404235406468" style="zoom:50%;" /> 

```
分区后，我们每次只关注关系 R 中的一个分区。我们应用第二个哈希函数 h2（可能与 h 相同，也可能不同）来组织分区内的数据。同时，我们扫描关系 S 中的匹配分区。在扫描过程中，我们使用为 R 分区创建的哈希表来探测和识别 S 中的任何匹配图元。以这种方式迭代每个分区对--将 R 中的第一个分区与 S 中的第一个分区匹配，然后移动到第二个分区，以此类推，我们就有效地执行了连接操作。
After partitioning, we focus on one partition at a time from relation R. We apply a second hash function, h2, which may be the same as or different from h, to organize the data within the partition. Simultaneously, we scan the matching partition from relation S. As we proceed, we use the hash table created for the partition of R to probe and identify any matching tuples in S. This probing is performed in the main memory, using designated input and output buffers for the operation. By iterating through each partition pair in this manner—matching the first partition from R with the first from S, then moving to the second, and so on—we effectively perform the join operation.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235826054.png" alt="image-20240404235826054" style="zoom:50%;" /> 

**1️⃣**In partitioning phase, we read+writeboth relations

**2️⃣**In matching phase, we read both relations

```sql
Cost (HJ) = 2 * NPages(Outer) + 2* NPages(Inner) -- Create partitions
				+ NPages(Outer) + NPages(Inner)      -- Match partitions
```

- Our example:

```sql
Cost(HJ) = 2*NPages(R) + 2*NPages(S) + NPages(R) + NPages(S)
			= 3 * 1000+ 3* 500= 4500 I/Os
```

```
在哈希连接过程的初始阶段，即分区阶段，两个关系都从磁盘读取，并在经过哈希函数处理后以分区的形式写回。这种读写操作会产生成本，计算公式为外部关系和内部关系的页数的两倍--这代表了创建分区所涉及的输入和输出操作。
In the initial phase of the hash-join process, the partitioning phase, both relations are read from disk and written back as partitions after being processed by the hash function. This read-write operation incurs a cost, which is calculated as two times the number of pages for both the outer and inner relations—this represents the input and output actions involved in creating the partitions.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235826054.png" alt="image-20240404235826054" style="zoom:50%;" /> 

**1️⃣**In partitioning phase, we read+writeboth relations

**2️⃣**In matching phase, we read both relations

```sql
Cost (HJ) = 2 * NPages(Outer) + 2* NPages(Inner) -- Create partitions
				+ NPages(Outer) + NPages(Inner)      -- Match partitions
```

- Our example:

```sql
Cost(HJ) = 2*NPages(R) + 2*NPages(S) + NPages(R) + NPages(S)
			= 3 * 1000+ 3* 500= 4500 I/Os
```

```
随后的匹配阶段需要读取相应关系的分区，以识别和匹配元组。这一阶段的相关成本是对每个分区的一次读取，即外部关系和内部关系的页数之和。总的来说，这一阶段的成本是外部关系的页数加上内部关系的页数。综合这些成本，我们可以得出结论，哈希连接需要对两个输入进行三次遍历，这个公式非常简单，可以在实际使用中记住。
The subsequent matching phase entails reading the partitions of the corresponding relations to identify and match tuples. The cost associated with this phase is a single read pass over each partition, which is the sum of the number of pages of the outer and inner relations. In totality, this phase’s cost is the number of pages of the outer relation plus the number of pages of the inner relation. Combining these costs, we conclude that the hash-join necessitates three passes over both inputs, a formula that is simple enough to be memorized for practical use.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240404235826054.png" alt="image-20240404235826054" style="zoom:50%;" /> 

**1️⃣**In partitioning phase, we read+writeboth relations

**2️⃣**In matching phase, we read both relations

```sql
Cost (HJ) = 2 * NPages(Outer) + 2* NPages(Inner) -- Create partitions
				+ NPages(Outer) + NPages(Inner)      -- Match partitions
```

- Our example:

```sql
Cost(HJ) = 2*NPages(R) + 2*NPages(S) + NPages(R) + NPages(S)
			= 3 * 1000+ 3* 500= 4500 I/Os
```

```
在我们的示例中--"储备 "关系的大小为 1000 页，"水手 "关系的大小为 500 页--哈希连接操作的总成本为 4,500 次 I/O 操作。值得注意的是，这个数字大大低于简单嵌套循环连接所产生的成本。虽然这个公式是一个有用的工具，但关键是要避免死记硬背，将其作为普遍最便宜的选择。散列连接的效率会因多种因素而变化，例如所需的传递次数和可用内存量。因此，在特定情况下，必须逐一评估各种替代方案，以确定最具成本效益的连接方法。
In the context of our illustrative example—with the 'Reserves' relation sized at 1000 pages and the 'Sailors' relation at 500 pages—the total cost for the hash-join operation is 4,500 I/O operations. Notably, this figure is significantly lower than the cost incurred by a simple nested loop join. While this formula is a useful tool, it's crucial to avoid rigidly memorizing it as the universally cheapest option. The hash-join's efficiency can vary based on several factors, such as the number of passes required and the amount of available memory. Consequently, it’s imperative to assess the alternatives case-by-case to determine the most cost-effective join method for a given situation.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405000940204.png" alt="image-20240405000940204" style="zoom:50%;" /> 

https://www.youtube.com/watch?v=o1dMJ6-CKzU

From 0:58

```
因此，如果你仍然对这些算法感到困惑，我建议你观看这个简短的视频，它基本上描述了这三种关节的算法是如何工作的。
So if you're still confused by any of these algorithms, I do encourage you to watch this short video that essentially depicts how these algorithms work uh for all three types of joints. 
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
![image-20240405001338970](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405001338970.png) 

```
让我们简要讨论一下数据库引擎。从本质上讲，我们有一个存储在磁盘上的大型数据库，所有数据都存放在那里。在内存中运行的数据库管理软件包括一个称为数据缓冲区的部分。这是一个内存缓冲区，数据从磁盘暂时引入到这里进行处理。在这里进行计算和数据处理，然后将结果返回给用户。虽然我们现在不会深入研究代理或日志缓冲区，但了解基本流程很重要：当用户从表中请求数据时，系统会从磁盘读取数据，在内存中进行处理，然后显示给用户。如果是数据更新，则首先在内存中进行更改。关于事务的概念以及数据状态如何在提交后永久保存在数据库中，我们将在后面进行探讨。本概述仅供参考。
Let's briefly discuss the database engine. Essentially, we have a large database stored on disk, where all data resides. The database management software, which operates in memory, includes a segment known as the data buffer. This is the memory buffer where data is temporarily brought from disk for processing. Here, calculations and data manipulations occur before the results are presented back to the user. While we won't delve into agents or the log buffer now, it's important to understand the basic flow: when a user requests data from a table, the system reads the data from disk, processes it in memory, and then displays it to the user. In the case of data updates, changes are first made in memory. The concepts of transactions and how data states are permanently stored in the database upon committing will be explored later. This overview serves merely for illustration purposes.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
![image-20240405001433135](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405001433135.png) 

```
要了解连接的操作方法，让我们从嵌套循环连接开始。在这种方法中，我们有两个输入：左边的输入用蓝色表示，右边的输入用绿色表示。这个过程包括从左输入端读取一条记录（为简化起见，通常是一页），然后从右输入端扫描所有记录以查找匹配。为便于说明，我们考虑的是单条记录而不是页面，以便清楚地表达概念。因此，我们从左边输入第一条记录（标为 "1"），然后将其与右边输入的所有记录进行比较，搜索匹配的记录。我们依次将第二条记录 "2"，然后是 "3"，以此类推，直到我们将左侧输入的每条记录都与右侧输入的所有记录进行比对。当我们检查到左边最后一条记录 "6 "时，我们的过程就结束了。这种高层次的描述有助于说明嵌套循环连接的基本工作原理。
To understand how joins operate, let's start with the Nested Loops Join. In this approach, we have two inputs: the left input is depicted in blue, and the right input in green. The process involves reading one record (for simplification, though typically it's one page) from the left input and then scanning all records from the right input to find matches. For illustrative purposes, we're considering individual records instead of pages to clearly convey the concept. So, we take the first record, labeled '1', from the left and compare it against all records on the right, searching for matches. We continue this process sequentially with the second record, '2', then '3', and so on, until we have checked each record from the left input against the entire right input. Once we reach the last record on the left, marked '6', our process concludes. This high-level description helps illustrate the fundamental workings of a Nested Loops Join.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
![image-20240405001525520](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405001525520.png) 

```
索引嵌套循环操作是我们尚未深入研究的主题，也不会在本主题中涉及，它围绕着利用索引（数据在其中排序）来高效导航到指示实际数据所在位置的指针。这种方法通过利用索引的有序结构来简化数据库中的搜索过程，但它不在我们当前的研究范围内，因此在现阶段无需担心对它的理解。
The Index Nested Loops operation, a topic we haven't delved into and won't be covering in this subject, revolves around utilizing an index—where data is sorted—to efficiently navigate to the pointers that indicate where the actual data resides. This method streamlines the search process within databases by leveraging the organized structure of indexes, but it's not within our current scope of study, so there's no need for concern about understanding it at this stage.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
![image-20240405001600314](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405001600314.png) 

```
排序-合并连接首先要对两个输入进行排序，确保它们完全有序。排序完成后，我们同时遍历两个输入。例如，在比较一个输入项中的 "1 "和另一个输入项中的 "1 "时，一旦遇到 "2"，我们就知道 "1 "的比较已经完成，然后继续比较 "2"。这个过程会高效地继续下去；当到达 "3 "时，我们无需再检查 "2"。这个方法展示了如何只需对两个输入进行一次传递就足以完成操作，突出了排序-合并连接在简化比较过程中的效率。
The Sort-Merge join begins by sorting both inputs, ensuring they are fully organized. Once sorted, we traverse both inputs simultaneously. For example, when comparing '1' from one input with '1' from the other, and as soon as we encounter '2', we know that the comparison for '1' is complete and move on to '2'. This process continues efficiently; upon reaching '3', we proceed without needing further checks for '2'. This method demonstrates how a single pass over both inputs suffices to complete the operation, highlighting the efficiency of the Sort-Merge join in streamlining the comparison process.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
![image-20240405001638302](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405001638302.png) 

```
散列连接最初是使用相同的散列公式将两个输入数据散列到数据桶中，这是确保相应数据进入相同数据桶的关键步骤。散列后，该过程包括检查这些相应数据桶中的匹配项。例如，当我们散列值 "2 "并将其指向第一个数据桶时，我们只需检查该特定数据桶中的所有值。检查完成后，我们就可以确定在其他任何数据桶中都找不到与 "2 "匹配的值。这种方法凸显了散列连接的效率，它根据应用的散列公式将数据分离成易于管理的部分，从而简化了匹配搜索。
The Hash Join involves initially hashing both inputs into buckets using the same hash formula, a critical step to ensure corresponding data lands in the same buckets. After hashing, the process involves checking for matches within these corresponding buckets. For instance, when we hash the value '2' and it directs us to the first bucket, we only need to examine all values within this specific bucket. Once this check is complete, we can be certain that no matches for '2' will be found in any other bucket. This method highlights the efficiency of the Hash Join, which simplifies the search for matches by segregating data into manageable segments based on the hash formula applied.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405002001457.png" alt="image-20240405002001457" style="zoom:50%;" /> 

```
好的 所以，最后一点要注意的是，当涉及到 "连接 "时，其实就是 "关节连接条件"。
OK. So uh the last note when it comes to join is really uh on joint joint conditions. 
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405002246792.png" alt="image-20240405002246792" style="zoom:50%;" /> 

**1️⃣**Equalities over several attributes (e.g., R.sid=S.sidAND R.rname=S.sname):

- For Sort-Merge and Hash Join, sort/partition on combination of the two join columns

**2️⃣**Inequality conditions (e.g., R.rname< S.sname):

1. Hash Join, Sort Merge Join not applicable
2. Block NL quite likely to be the best join method here

```
在本讲座对等连接的探索中，我们学习了如何执行以属性间相等为条件的连接，例如 R.sid = S.sid AND R.rname = S.rname。对于这些情况，我们有几种方法可供选择：嵌套循环、排序合并或散列连接，每种方法都有自己独特的基于共同属性组合行的方法。对于排序合并和散列连接，其实现依赖于根据两个连接列的组合对表进行排序或分区，而这两个连接列是完全匹配的。
In our exploration of equi-joins during this lecture, we've learned how to execute joins where the condition is based on equality between attributes, such as R.sid = S.sid AND R.rname = S.rname. For these scenarios, we have several options at our disposal: nested loops, sort-merge, or hash joins, each with its unique approach to combining rows based on a common attribute. In the case of sort-merge and hash joins, the implementation relies on sorting or partitioning the tables on the combination of the two join columns, which are matched exactly.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405002246792.png" alt="image-20240405002246792" style="zoom:50%;" /> 

**1️⃣**Equalities over several attributes (e.g., R.sid=S.sidAND R.rname=S.sname):

- For Sort-Merge and Hash Join, sort/partition on combination of the two join columns

**2️⃣**Inequality conditions (e.g., R.rname< S.sname):

1. Hash Join, Sort Merge Join not applicable
2. Block NL quite likely to be the best join method here

```
但是，当遇到涉及不等式的其他类型连接条件（如 R.rname < S.sname）时，上述连接方法就不适用了。在这种情况下，数据库通常会恢复使用嵌套循环连接，而块嵌套循环 (BNL) 连接可能是最具成本效益的解决方案。之所以选择这种方法，是因为散列连接本身不适合非相等条件--它们无法处理范围比较。同样，排序合并连接也不支持不等式条件，尽管潜在的期望与此相反。虽然在排序合并框架内设计一种允许不等式的算法是可以想象的，但标准数据库系统并没有采用这种方法。因此，在连接条件不是基于相等条件的相对罕见的情况下，数据库默认采用阻塞嵌套循环连接。
However, when faced with other types of join conditions that involve inequalities, such as R.rname < S.sname, the aforementioned join methods are not applicable. Databases typically revert to using nested loop joins for these cases, with block nested loops (BNL) joins being a likely candidate for the most cost-effective solution. The reason for this preference is that hash joins are inherently unsuitable for non-equality conditions—they cannot process range comparisons. Similarly, despite potential expectations to the contrary, sort-merge joins do not support inequality conditions either. While it is conceivable to design an algorithm that allows for inequalities within a sort-merge framework, standard database systems have not adopted such an approach. Therefore, in the relatively uncommon scenarios where the join condition is not based on equality, databases default to block nested loops joins.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405002510259.png" alt="image-20240405002510259" style="zoom:50%;" /> 

**1️⃣**A virtue of relational DBMSs:

1. Queries are composed of a few basic operators
2. Implementation of operators can be 🚗efully tuned
3. Important to do this

**2️⃣**Many alternative implementations for each operator

- No universally superior technique for most operators

**3️⃣**Must consider alternatives for each operation in a query and choose best one based on system statistics…

- Part of the broader task of optimizing a query composed of several operations

```
总之，本讲座深入探讨了数据库系统中各种操作的实现，将关系代数的理论概念转化为实际应用，如选择、投影和连接。我们探讨了实现这些操作的不同方法，以及如何估算它们在数据库系统中的成本。这就介绍了优化器的作用，也就是我们将在下一课讨论的幕后 "大脑"。优化器会评估执行查询的所有可能替代方案，并选择最具成本效益的方法，以确保快速响应时间。这一错综复杂的过程强调了优化器在数据库管理中的关键作用，其目的是提高效率和性能。
In summary, this lecture has delved into the implementation of various operations within a database system, translating the theoretical concepts of relational algebra into practical applications such as selection, projection, and joins. We've explored different methods of implementing these operations and how to estimate their costs within a database system. This introduces the role of the optimizer, the "brain" behind the scenes, which we will discuss in our next session. The optimizer evaluates all possible alternatives to execute a query and chooses the most cost-effective approach, ensuring fast response times. This intricate process underscores the optimizer's crucial role in database management, aiming to enhance efficiency and performance.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240405002727089.png" alt="image-20240405002727089" style="zoom:50%;" /> 

**1️⃣**Understand alternatives for join operator implementations

- Be able to calculate the cost of alternatives

**2️⃣**Important for Assignment 3 as well

```
那么，什么是必不可少的呢？你需要掌握如何应用这些公式，你将有一份公式小抄供你使用。我会为你们提供所有的公式，但关键是你们要了解如何有效地利用每一个公式。这将是你的作业地形，你将在这里扮演优化师或数据库管理员的角色。感谢您的关注，我期待着下一堂课的到来。
So, what is essential? You'll need to grasp how to apply these formulas, and you'll have a formula cheat sheet at your disposal. I'll provide you with all the formulas, but it's crucial for you to understand how to utilize each one effectively. This will be your assignment terrain, where you'll step into the role of the optimizer or the database administrator. Thank you for your attention, and I look forward to our next session.
```

