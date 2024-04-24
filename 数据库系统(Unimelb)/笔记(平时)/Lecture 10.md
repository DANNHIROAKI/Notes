# Lecture 10: Storage and Indexing

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153002482.png" alt="image-20240421153002482" style="zoom: 60%;" /> 

# 1. File Organization (Heap & Sorted files)

> ## 1.1. DBMS中文件的概念
>
> > **1️⃣**$\text{File}\xrightarrow{\text{A collection of}}
> > \begin{cases}\text{Page 1}\\
> > \text{Page 2} \xrightarrow{\text{Containing}}\begin{cases}
> > \text{Record 1}\\
> > \text{Record 2}\\
> > ....
> > \\\text{Record M}\end{cases}\\.....\\\\\text{Page N}
> > \end{cases}$​
> >
> > 1. 页内：大小是固定的(fixed-size)，每个页都有一组逗号间隔属性值的记录，记录一行行排满页
> >
> > 2. 页间：通过类似于链表(Link List)的结构链接
> >
> > **2️⃣**示例：每页四个记录，每个记录两个属性
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154422821.png" alt="image-20240421154422821" style="zoom:50%;" /> 
> >
> > **3️⃣**DBMS支持的文件操作
> >
> > 1. insert/delete/modify record
> > 2. 检索/读取特定记录 → using record id 
> >    - 记录与id对应，定位文件的物理位置
> >    - id通常是页码+页内偏移的组合
> > 3. scan all records (possibly with some conditions on the records to be retrieved)
>
> ## 1.2. 文件组织
>
> > |        文件类型         |         描述         |              适用情况              |
> > | :---------------------: | :------------------: | :--------------------------------: |
> > |   堆文件 (Heap files)   |      记录间无序      |      扫描文件 / 检索所有记录       |
> > | 有序文件 (Sorted files) |  页面及其内记录有序  |     按某种顺序检索(一系列)记录     |
> > |  索引文件 (Index File)  | 某种顺序下的最快检索 | 需要快速按照特定顺序检索记录的情况 |
> >
> > ### 1.2.1. Heap Files / Unsorted Files
> >
> > > **1️⃣**特点：简单的文件结构，记录无序(in no particular order)
> > >
> > > **2️⃣**堆文件操作
> > >
> > > |   操作   |                       描述                        | 特点 |
> > > | :------: | :-----------------------------------------------: | :--: |
> > > | 添加记录 | 找到下一个空槽(empty slot)，空槽可以在本页/下一页 | 快速 |
> > > |   查找   |              查找每一页的每一个记录               |  慢  |
> >
> > ### 1.2.2. Sorted Files
> >
> > > **1️⃣**特点：页面和记录都按明确顺序排列，例如下面的按照年龄从低到高排列
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154608240.png" alt="image-20240421154608240" style="zoom:50%;" /> 
> > >
> > > **2️⃣**排序文件操作：
> > >
> > > |   操作   |               描述               | 特点 |
> > > | :------: | :------------------------------: | :--: |
> > > | 添加记录 | 按正确顺序插入，后续记录依次后移 |  慢  |
> > > |   查找   |     二分搜索(binary search)      | 快速 |
> >
> > ### 1.2.3. DBMS权衡插入/查找
> >
> > > **1️⃣**Model cost of all operations
> > >
> > > 1. 准则：从磁盘到内存传输数据所需的**页访问次数**，即**IO次数**
> > >
> > > 2. 示例：100个记录的表 / 每页10个记录 → 需要10次IO才能取出所有记录
> > >
> > > **2️⃣**搜索成本示例：在位置页数B(姑且让B=4)的文件中，寻找年龄20-30的记录
> > >
> > > 1. Heap File: 必须检查每个记录→取出每一页→B次IO即成本=B
> > >
> > >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421234857176.png" alt="image-20240421234857176" style="zoom: 40%;" /> 
> > >
> > > 2. Sort File: 从中间截断然后寻找右侧→成本是$\log_2{B}$
> > >
> > >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421235236728.png" alt="image-20240421235236728" style="zoom:40%;" /> 

# 2. Index files \& indexes

> ## 2.1. Overview
>
> > **1️⃣**排序文件太贵了，索引文件反倒是一个好的替代
> >
> > **2️⃣**索引：建立在文件之上(top of data pages)的辅助结构(auxiliary structure)
> >
> > **3️⃣**检索字段(search key fields)：构建索引的特定字段或属性的子集，例如在年龄属性上建立索引快速找到20岁的人
> >
> > **4️⃣**索引组成：数据条目(Entries), 数据记录(Record), 查找条件
>
> ## 2.2. 索引示例：B+ 树结构
>
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240422210931943.png" alt="image-20240422210931943" style="zoom:33%;" /> 
> >
> > ### 2.2.1. 结构
> >
> > > **1️⃣**最底层：是数据文件，original pages with data records stored within
> > >
> > > **2️⃣**索引文件建立在数据文件之上
> > >
> > > **3️⃣**数据条目(Entries): 位于索引的底部
> >
> > ### 2.2.2. B+树结构：n叉树
> >
> > > **1️⃣**顶端：是根节点(aka目录)，是检索的开端。例如小于2时就会往最左边检索........
> > >
> > > **2️⃣**结点：
> > >
> > > 1. 内部：值从左到右增大
> > > 2. 左子树：值始终小于节点值
> > > 3. 右子树：值大于或等于节点值

# 3. Index classification

> ## 3.1. 聚类与非聚类(Clustered vs. unclustered)
>
> > **1️⃣**聚类索引定义：数据记录(data records)的顺序 = 索引数据项(index data entries)的顺序
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162644005.png" alt="image-20240421162644005" style="zoom:50%;" /> 
> >
> > **2️⃣**Zooming into Clustered Index
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162822085.png" alt="image-20240421162822085" style="zoom:50%;" /> 
> >
> > 1. 结构：从根节点开始搜索→到达底部的叶节点→叶节点包括指针/Record ID(指向数据存放位置)
> > 2. 优化了搜索过程，但是插入数据时是灾难性的
> >
> > **3️⃣**Zooming into Unclustered Index
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162937573.png" alt="image-20240421162937573" style="zoom:50%;" /> 
> >
> > 1. 结构：data is stored in no particular order
> > 2. 搜索很慢，但是插入很快
> >
> > **4️⃣**Clustering Properties
> >
> > 1. 单个表上只能有一个聚类索引，因为物理顺序要与索引对齐就只有一种方式
> > 2. 聚类后的数据，检索(Retrieving)成本大大降低
> > 3. 聚类索引的维护成本极高，尤其是对于需要频繁插入Record的时候
> >
> > **5️⃣**索引开销
> >
> > 1. Clustered: cost $\approx$ pages in data file with matching records包含匹配的记录有多少页数
> >
> > 2. Unclustered: cost $\approx $ of matching index data entries (data records)匹配的索引数据条目
> >
> > 比如我要找300个20-30岁的Record，这300个record within 4 Pages
> >
> > - Clustered: Cost ≈ 4
> > - Uncluttered: Cost ≈ 300
>
> ## 3.2. Key Related Index
>
> > **1️⃣**Primary Vs. Secondary (主次索引)
> >
> > 1. 主索引：建立在primary key上的，索引永远没有重复项
> > 2. 次索引：可能存在重复
> >
> > **2️⃣**Single Key vs. Composite (单键/复合键索引)
> >
> > 1. composite index: Index built over **composite search key**
> >
> > 2. 示例：年龄+薪水(先排年龄再排薪水) & 薪水+年龄→复合键索引
> >
> >    Efficient to answer: age $=12$ and sal $=10$ & age $=12$ and sal $>15$
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164052741.png" alt="image-20240421164052741" style="zoom:60%;" />  
>
> ## 3.3. Hash-Based Index: 擅长精确搜索
>
> > **1️⃣**哈希：$\text{Key(Record)}\xrightarrow{\text{Hash函数}}\text{Hash值}\xrightarrow{\text{一个Hash值对应一个桶(Buckedt)}}放入对应桶(内\text{Record}无序)$
> >
> > | Record | Key  | Hash |  Bucket   |
> > | :----: | :--: | :--: | :-------: |
> > | Alice  |  A   |  1   | 1对应的桶 |
> > |  Bob   |  B   |  2   | 2对应的桶 |
> > | David  |  D   |  4   | 4对应的桶 |
> >
> > **2️⃣**示例：Data Entries 在索引中不是排序的，而是按照Hash函数进行分组
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" /> 
> >
> > 1. 记录除以4→余数=0/1/2/3时放入桶1/2/3/4
> > 2. 一旦在索引上找到符合要求条目→按照子帧访问数据文件
> >
> > **3️⃣**基于Hash的索引工作原理
> >
> > 1. 搜索2007特定工资
> > 2. 计算2007的Hash值→为3→指向四号桶
> > 3. 然后就只要在桶内(而非全局)搜索就行了，因为2007有的话必定只能在该捅内
> >
> > **4️⃣**缺陷：检索一系列值时Hash的效率降低，比如检索1000-2000范围显然排序更有效
>
> ## 3.4. Tree-Based Index
>
> > **1️⃣**Overview for B+ Tree
> >
> > 1. Nodes contain pointers to lower levels, Left = Lower / Right = Higher
> > 2. Leave contain data entries sorted by search key values.
> >
> > **2️⃣**示例：搜索年龄超过39岁的个体
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421165933628.png" alt="image-20240421165933628" style="zoom:50%;" /> 
> >
> > 1. 39位于12-78之间$\xrightarrow{下一层}$39位于19-56之间$\xrightarrow{下一层}$​39位于33-44之间
> > 2. 从33开始，逐渐进展到40

















