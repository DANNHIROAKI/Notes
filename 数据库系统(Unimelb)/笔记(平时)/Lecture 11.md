# Lecture11: QueryProcessing I

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240423171101265.png" alt="image-20240423171101265" style="zoom: 45%;" />  

# 1. Query Processing

> ## 1.1. Overview
>
> > **1️⃣**为何需要查询优化：Some database operations are EXPENSIVE
> >
> > **2️⃣**如何查询优化
> >
> > 1. 各种关系代数运算符的复杂实现技术
> > 2. 选取最优的查询计划
>
> ## 1.2. 查询处理工作流
>
> > ```sql
> > Select * From Blah B Where B.blah = "foo"
> > ```
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240423182552042.png" alt="image-20240423182552042" style="zoom:40%;" />  
> >
> > **1️⃣**查询解析器(Paeser): 检查语法，这一步还可能重写语句，以保证等价情况下更高效查询
> >
> > **2️⃣**查询优化器：数据库的大脑
> >
> > 1. 检查执行查询的所有可行计划
> > 2. 基于关系代数原理估计成本→选取最优计划
> >
> > **3️⃣**查询计划评估器(aka执行器)：
> >
> > 1. 接收选出的最优查询计划
> > 2. 执行查询计划，以**最有效的方式**执行每个步骤和关系运算符
>
> ## 1.3. 关系操作
>
> > 如何高效地实现三种基本数据库操作
> >
> > | Operation  | Symbol |              Description               |
> > | :--------: | :----: | :------------------------------------: |
> > | Selection  |   σ    | Selects a subset of rows from relation |
> > | Projection |   π    | Deletes unwanted columns from relation |
> > |    Join    |   ⋈    |   Allows us to combine two relations   |

# 2. Selection

> ## 2.0. 所用示例：水手
>
> > ```c#
> > Sailors (sid: integer, sname: string, rating: integer, age: real)
> > Reserves (sid: integer, bid: integer, day: dates, rname: string)
> > ```
> >
> > **1️⃣**Sailors Dataset
> >
> > |    Description    |   Value   | 符号                                 |
> > | :---------------: | :-------: | :----------------------------------- |
> > | Each tuple length | 50 bytes  | #                                    |
> > |  Tuples per page  | 80 tuples | $\text{Ps = NTuplesPerPage(S) = 80}$ |
> > |    Total pages    | 500 pages | $\text{N = NPages(S) = 500}$         |
> > |   Total Tuples    |   40000   | $\text{NTuples(S) = 500*80 = 40000}$ |
> >
> > **2️⃣**Reserves Dataset
> >
> > |    Description    |   Value    | 符号                                  |
> > | :---------------: | :--------: | :------------------------------------ |
> > | Each tuple length |  40 bytes  | #                                     |
> > |  Tuples per page  | 100 tuples | $\text{Ps = NTuplesPerPage(S) = 100}$ |
> > |    Total pages    | 1000 pages | $\text{N = NPages(S) = 1000}$         |
> > |   Total Tuples    |   100000   | $\text{NTuples(S) = 500*80 = 100000}$ |
>
> ## 2.1. Simple Selection
>
> > **1️⃣**在SQL语句中：
> >
> > 1. 用`SELECT`指定表中的列
> > 2. 用`WHERE`根据特定条件过滤结果
> >
> > ```sql
> > SELECT * FROM Reserves R WHERE R.BID > 20;
> > ```
> >
> > **2️⃣**影响`SELECT`语句执行速度的要素
> >
> > 1. 可用的索引/访问路径
> > 2. 预期大小，aka查询将返回的元组数量。由此在执行前会估计返回元组量
> >
> > **3️⃣**单个表上选择操作估计大小 = 整个关系大小*还原因子
> > $$
> > \text{size of relation * Π(reductionfactors)}
> > $$
> > 
> >
> > 1. 

# 3. Projection

