

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152442013.png" alt="image-20240421152442013" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152502013.png" alt="image-20240421152502013" style="zoom:50%;" /> 

```
我们考试的重点是实际应用，而不是死记硬背定义。我们不只是要求你回忆每条 SQL 语句的作用，或定义 GROUP BY、HAVING 或嵌套等术语。相反，我们注重的是您应用这些概念为各种实际场景编写 SQL 查询的能力。这对你未来的职业生涯和职场生活都是至关重要的技能。感谢您的关注，我期待着下一堂课的到来。
The focus of our examinations is on practical application rather than rote definitions. We're not just asking you to recall what each SQL statement does or to define terms like GROUP BY, HAVING, or nesting. Instead, we're concentrating on your ability to apply these concepts to write SQL queries for various real-world scenarios. This is a crucial skill for your future 🚗eer and life in the workforce. Thank you for your attention, and I look forward to our next session.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152442013.png" alt="image-20240421152442013" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152601994.png" alt="image-20240421152601994" style="zoom:50%;" /> 

```
我们已经学会了如何将这些转化为模型，特别是转化为数据库方案的模型。
And we have learned how to transform those into models in particular into model that was translated then into a database scheme. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152442013.png" alt="image-20240421152442013" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152643512.png" alt="image-20240421152643512" style="zoom:50%;" /> 

```
之后，我们了解了如何操作刚刚创建的数据。因此，我们已经深入学习了 SQL 
After that, we have looked at how we can manipulate with the data we have just created. So we have covered SQL in depth 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152725196.png" alt="image-20240421152725196" style="zoom: 50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152745411.png" alt="image-20240421152745411" style="zoom:50%;" /> 

```
欢迎回到数据库系统第十讲。今天，我们将深入探讨存储和索引，并研究这两者在整个课题框架中的关系。最初，我们用通俗易懂的语言分析了高级用例，现在我们开始第三个主要部分--剖析执行查询时数据库系统的内部运作。这一部分可能更具挑战性，尤其是对于那些没有计算机科学背景的人来说，因为其中涉及的一些算法可能对某些人来说并不熟悉。
Welcome back to the tenth lecture of Database Systems. Today, we’re delving into storage and indexing, and we’ll examine how this fits within the whole framework of the subject. Initially, we analyzed high-level use cases in plain language, and now we’re embarking on the third major segment—dissecting the internal workings of a database system when we execute our queries. This section may be more challenging, particularly for those without a computer science background, given it covers some algorithms that may be unfamiliar to some of you.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152725196.png" alt="image-20240421152725196" style="zoom: 50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152745411.png" alt="image-20240421152745411" style="zoom:50%;" /> 

```
如果你觉得这个广泛的主题令人生畏，请不要担心--无论你是设计、医学或其他领域的学士，你都会通过它，就像每个人一样。本课程提供了成功所需的一切。了解数据库的内部结构对于复杂的数据库使用至关重要；它不仅关乎开箱即用的功能。数据库经常需要调整，而扎实掌握内部运作的基础知识将大有裨益。数据库技术在不断发展，许多工作机会都集中在数据库的构建、优化和调整上--这些领域都是回报丰厚的领域。这些知识不仅仅是学术性的，更重要的是让你掌握工作所需的技能，而我的目标就是让你为这些高需求的职位做好准备。
Don't worry if you're finding this broad subject matter daunting—whether you're a Bachelor of Design, Medicine, or any other field, you’ll get through it, just like everyone does. Everything you need to succeed is provided in this course. Understanding the internals of databases is crucial for sophisticated database use; it's not just about out-of-the-box functionality. Databases often require tuning, and a solid grasp of the basics of internal workings is immensely beneficial. Database technology is constantly evolving, with many job opportunities focused on building, optimizing, and tuning databases—areas that are highly rewarding. This knowledge is not just academic; it’s about equipping you with the skills needed in the workforce, and I aim to prepare you for these high-demand roles.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152947963.png" alt="image-20240421152947963" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153002482.png" alt="image-20240421153002482" style="zoom:50%;" /> 

This is one of several possible architectures; each system has its own slight variations.

```
当我们执行查询时，我们将进入数据库系统内部发生的错综复杂的细节--这是本课程具有挑战性但又必不可少的部分，尤其是对于那些非计算机科学背景的人来说。对于某些人来说，算法可能是全新的，重要的是不要不知所措。本课程旨在让所有学生，无论其主要学习领域如何，都能对这些概念有必要的了解。这些知识对于任何希望成为数据库技术高级用户的人来说都至关重要，因为数据库需要微调才能达到最佳性能。了解数据库内部运作的基础知识非常有帮助，这门课程可以让您为在科技行业中担任数据库性能和优化方面的高薪职位做好准备。
We're moving into the intricate details of what happens inside a database system when we execute queries—a challenging but essential part of this course, especially for those not from a computer science background. With algorithms that might be new to some, it's important not to be overwhelmed. The course is designed to equip all students, irrespective of their primary field of study, with the necessary understanding of these concepts. This knowledge is crucial for anyone aiming to become a sophisticated user of database technologies, as databases require fine-tuning to perform optimally. Understanding the basics of a database's internal workings is immensely helpful, and this subject prepares you for the highly sought-after, well-compensated roles in database performance and optimization that are abundant in the tech industry.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421152947963.png" alt="image-20240421152947963" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153002482.png" alt="image-20240421153002482" style="zoom:50%;" /> 

This is one of several possible architectures; each system has its own slight variations.

```
这部分课程深入探讨数据库的查询处理模块、并发控制和存储模块--决定数据库如何存储、访问和管理数据的关键组件。我们首先关注数据的存储方式，为后面讨论事务和数据管理打下基础。请记住，了解这些内部工作原理并不仅仅是学术性的，而是为你进入现实世界做好准备，因为在现实世界中，数据库技能是非常宝贵的，而且往往是必需的。
This part of the course dives into the database's query processing module, concurrency control, and storage module—key components that determine how a database stores, accesses, and manages data. We start by focusing on how data is stored, giving a foundation for later discussions on transactions and data management. Remember, understanding these inner workings is not just academic; it's about preparing you for the real world where database skills are valuable and often required.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153735692.png" alt="image-20240421153735692" style="zoom:50%;" /> 

- File organization (Heap \& sorted files)
- Index files \& indexes
- Index classification

```
今天我们将重点讨论文件组织，特别是堆文件和排序文件，并介绍索引，它是数据库操作的基础。索引至关重要，因为它在很大程度上决定了查询的执行速度。我们还将深入研究各种索引分类。本讲座的材料取材于 Ramakrishnan 和 Gehrke 的著作《数据库系统》的第 8 章。现在，让我们开始深入学习这些概念。
In our coverage today, we’re focusing on file organization, specifically heap and sorted files, and discussing indices, which are fundamental to the operation of databases. Indices are crucial because they largely determine how quickly our queries are executed. We'll also delve into various index classifications. This lecture's material is based on Chapter 8 from Ramakrishnan & Gehrke’s "Database Systems" book. Now, let's get started and dive deep into these concepts.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153819371.png" alt="image-20240421153819371" style="zoom:50%;" /> 

**1️⃣****FILE**: A collection of **pages**, each containing a collection of **records.**

```
在数据库领域，数据存储在称为文件的结构中。在概念上，这些文件与我们熟悉的 Excel 电子表格或 PowerPoint 演示文稿等文档类似，都是容器。不过，数据库中的文件与众不同，它们由固定大小的页面组成，每个页面都包含一组记录。这些记录是由逗号划分的数据行，每行包含多列。记录密密麻麻地排列在一页中，直到页满为止，然后再开始新的一页。
In the realm of databases, data is stored in structures known as files. Conceptually similar to familiar documents, such as Excel spreadsheets or PowerPoint presentations, these files serve as containers. However, the files in databases are distinctive in that they are composed of fixed-size pages, each harboring a collection of records. These records are rows of data delineated by commas, each containing multiple columns. The records are densely packed into a page until it is full, at which point a new page is started.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421153819371.png" alt="image-20240421153819371" style="zoom:50%;" /> 

**1️⃣****FILE**: A collection of **pages**, each containing a collection of **records.**

**2️⃣**DBMS must support:

1. insert/delete/modify record
2. read a particular record (specified using record id)
3. scan all records (possibly with some conditions on the records to be retrieved)

```
数据库必须高效地执行各种操作，包括插入、删除和修改记录。数据库还必须根据用户要求查找和检索特定记录。为了实现这一目标，数据库使用了一种称为 "记录 ID "的系统，这是一种唯一的标识符，可精确定位记录在文件中的物理位置。虽然这里没有显示，但可以设想每条记录都有一个独特的 ID，通常是页码和该页内位置偏移的组合。这种机制可确保数据库能精确地导航和管理记录，这也是我们将进一步详细探讨的一项重要功能。
Databases must efficiently perform a variety of operations, including inserting, deleting, and modifying records. They must also locate and retrieve specific records as requested by users. To achieve this, databases use a system known as 'record ID,' a unique identifier pinpointing the physical location of a record within the file. Although not shown here, envision each record possessing a distinct ID, typically a combination of the page number and a positional offset within that page. This mechanism ensures that the database can precisely navigate to and manage records, an essential feature we'll explore in further detail.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154040761.png" alt="image-20240421154040761" style="zoom:50%;" /> 

Many alternatives exist, each good for some situations, and not so good in others:

**1️⃣**Heap files: no particular order among records

- Suitable when typical access is a file scan retrieving all records

**2️⃣**Sorted Files: pages and records within pages are ordered by some condition

- Best for retrieval (of a range of records) in some order

```
在文件组织领域，各种替代方案在特定情况下表现出色，而在其他情况下则不尽如人意。堆文件组织方式的特点是记录之间没有任何特定顺序，因此当典型操作是全文件扫描以检索所有记录时，堆文件组织方式特别有效。另一方面，排序文件会根据特定标准在页面内保持内部顺序。在搜索特定范围的记录时，这种排序方式能大大提高数据检索的速度。
In the realm of file organization, various alternatives are designed to excel in specific scenarios while being less optimal in others. The heap file organization is characterized by a lack of any specific order among records, making it particularly efficient when the typical operation is a full file scan to retrieve all records. On the other hand, sorted files maintain an internal order within pages according to a specific criterion. This ordering significantly enhances the speed of data retrieval when searching for a particular range of records.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154040761.png" alt="image-20240421154040761" style="zoom:50%;" /> 

Many alternatives exist, each good for some situations, and not so good in others:

**1️⃣**Heap files: no particular order among records

- Suitable when typical access is a file scan retrieving all records

**2️⃣**Sorted Files: pages and records within pages are ordered by some condition

- Best for retrieval (of a range of records) in some order

**3️⃣**Index File Organizations:

1. Special data structure that has the fastest retrieval in some order
2. Will cover shortly..

```
索引文件组织引入了一种辅助数据结构，通过保持一定的顺序来显著提高检索速度，我们将在后面详细讨论。每种结构都有其独特的优势：堆文件在插入操作和全记录访问方面速度明显较快，而排序文件在查询给定条件下的特定值方面表现出色。不过，由于必须保持顺序，在排序文件中修改或插入数据可能是一个复杂的过程，我们将对此进行更深入的探讨。
Index file organization introduces an auxiliary data structure that significantly boosts retrieval speed by maintaining some order, which we will discuss in detail later. Each structure offers distinct advantages: heap files are notably swift for insert operations and full-record access, while sorted files excel in querying for specific values under a given condition. However, modifying or inserting data in sorted files can be a complex process due to the necessity of maintaining order, which we will examine more closely.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154320772.png" alt="image-20240421154320772" style="zoom:50%;" /> 

**1️⃣**Simplest file structure, contains records in no particular order

**2️⃣**As file grows and shrinks, disk pages are allocated and de-allocated

- Fastest for inserts compared to other alternatives

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154422821.png" alt="image-20240421154422821" style="zoom:50%;" /> 

```
堆文件是一种用于存储数据的初级结构，它所包含的记录没有任何固有顺序。举例说明，想象一个由四页组成的文件，每页保存四条记录，每条记录包含两个属性，如年龄和工资。这些页面看起来似乎是孤立的，但实际上它们是以一种不易察觉的方式连接在一起的；它们形成了一个序列，允许从一个页面移动到另一个页面。这类似于计算机科学中的链表--一种同时引用下一个和上一个元素的结构，可以在页面之间进行导航。对于不太熟悉链表概念的人来说，最重要的一点是要了解有一种机制可以轻松地从一个页面跳转到下一个页面。
The heap file is a rudimentary structure for storing data, without any inherent order to the records it contains. To illustrate, imagine a file made up of four pages, where each page holds four records, and each record comprises two attributes, such as age and salary. These pages may seem isolated, but they are actually connected in a way that isn't immediately visible; they form a sequence allowing movement from one page to another. This is similar to a linked list in computer science—a structure with references to both the next and previous elements—enabling navigation between pages. For those less familiar with the concept of linked lists, the critical aspect to understand is that there is a mechanism in place to jump from one page to the subsequent one with ease.
```
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154320772.png" alt="image-20240421154320772" style="zoom:50%;" /> 

**1️⃣**Simplest file structure, contains records in no particular order

**2️⃣**As file grows and shrinks, disk pages are allocated and de-allocated

- Fastest for inserts compared to other alternatives

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154422821.png" alt="image-20240421154422821" style="zoom:50%;" /> 

```
现在，在堆文件中添加新数据的过程非常简单，只需找到下一个空槽即可。这个空槽可能就在现有页面中，或者，如果当前页面已满，我们就跳转到下一个可用页面。这就是它的复杂程度，或者说没有复杂程度。然而，要搜索一个特定的数据点，比如找出所有 12 岁有工资的人的记录，则是一个挑战。在没有任何顺序的情况下，我们必须检查文件中的每一页，找出符合我们标准的每一个实例。这样，堆文件就显示出一种明显的权衡：它们允许快速插入数据，却使搜索过程更加费力和耗时。
Now, when it comes to adding new data to a heap file, the process is as simple as finding the next open slot. This empty slot could be within an existing page, or, if the current page is filled to capacity, we proceed to the next available page. That's the extent of its complexity—or lack thereof. However, searching for a specific data point, such as identifying all records of 12-year-olds with a salary, presents a challenge. Without any sort of order, we must examine each page in the file to locate every instance that meets our criteria. In this way, heap files display a clear trade-off: they permit quick data insertion but make the search process more laborious and time-consuming.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154520138.png" alt="image-20240421154520138" style="zoom:50%;" /> 

**1️⃣**Similar structure like heap files (pages and records), but pages and records are **ordered**

**2️⃣**Fast for range queries, but hard for maintenance (each insert potentially reshuffles records)

**3️⃣****Example**: A sorted file ordered by age

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154608240.png" alt="image-20240421154608240" style="zoom:50%;" /> 

```
排序文件与堆文件在结构上有相似之处，页面和记录都是按照清晰明确的顺序排列的。以按年龄排序的文件为例，如屏幕底部所示。在这种排列中，年龄依次递增-11、12、13，以此类推-确保数据有条不紊地排序。这种排序大大提高了搜索效率，因为记录的位置是可预测的。
Sorted files share a structural similarity with heap files, with pages and records arranged in a clear, defined order. Take, for example, a sorted file organized by age, as depicted at the bottom of the screen. In this arrangement, ages ascend sequentially—11, 12, 13, and so forth—ensuring the data is methodically ordered. This ordering significantly enhances search efficiency since records are predictably placed.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154520138.png" alt="image-20240421154520138" style="zoom:50%;" /> 

**1️⃣**Similar structure like heap files (pages and records), but pages and records are **ordered**

**2️⃣**Fast for range queries, but hard for maintenance (each insert potentially reshuffles records)

**3️⃣****Example**: A sorted file ordered by age

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154608240.png" alt="image-20240421154608240" style="zoom:50%;" /> 

```
然而，排序文件的有序性也给数据维护带来了挑战。例如，要插入一条 15 岁的记录，就需要按照正确的顺序排列，这样就会把后面的记录往下拉。当一页的空间用完时，必须将记录转移到后续页，这就需要额外的空间，并可能导致记录转移的连锁效应。与处理堆文件相比，这个过程显然更耗费人力，因此速度也更慢。然而，这种组织方式的好处是与高效搜索算法（如二进制搜索）兼容，允许数据库快速定位所需记录。这种对立体现了数据库管理的核心权衡--插入的简便性与检索的速度。
However, the ordered nature of sorted files presents challenges during data maintenance. To insert a record for a 15-year-old, for instance, requires placement in the correct sequence, pushing subsequent records down. When space on a page runs out, records must be shifted to subsequent pages, necessitating additional space and potentially causing a cascading effect of record shifting. This process is evidently more labor-intensive compared to handling heap files and, as a result, is slower. Yet, the benefit of such organization is its compatibility with efficient searching algorithms like binary search, allowing databases to quickly locate desired records. This dichotomy exemplifies a core trade-off in database management—ease of insertion versus speed of retrieval.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154754467.png" alt="image-20240421154754467" style="zoom:50%;" /> 

- Data is typically stored in pages on Hard Disks (HDD).
- To be able to process and analyze it - data needs to be brought to Memory (RAM).

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154815423.png" alt="image-20240421154815423" style="zoom: 67%;" /> 

```
排序文件与堆文件的框架相似，都是由包含记录的页面组成。但关键区别在于，在排序文件中，这些页面和记录是按照特定顺序排列的。这方面的一个例子是按年龄属性排序的文件，如幻灯片底部所示。这种有序结构有利于搜索，因为它可以使用高效的搜索算法，如二进制搜索，这种算法充分利用了数据的排序性质。例如，如果我们要查找年龄为 14 岁的记录，我们就会知道小于 14 岁的记录在左边，大于 14 岁的记录在右边，从而优化了搜索过程。
Sorted files share a similar framework with heap files, in that they are composed of pages containing records. The critical distinction, however, is that within sorted files, these pages and records are arranged in a specific order. An exemplar of this is a file sorted by the attribute of age, as depicted at the bottom of the slide. This ordered structure is beneficial for searches since it enables the use of efficient searching algorithms like binary search, which capitalizes on the sorted nature of the data. For example, if we seek a record with an age of 14, we know anything less will be to the left, and anything greater will be to the right, thus optimizing the search process.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154754467.png" alt="image-20240421154754467" style="zoom:50%;" /> 

- Data is typically stored in pages on Hard Disks (HDD).
- To be able to process and analyze it - data needs to be brought to Memory (RAM).

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154815423.png" alt="image-20240421154815423" style="zoom: 67%;" /> 

```
然而，排序文件给维护工作带来了挑战，尤其是在插入记录时。要插入一条 15 岁个人的记录，我们必须将其放在正确的页面上，并相应地移动后续记录，包括在空间有限的情况下将一些记录移动到新的页面。这可能涉及大量的工作，使插入速度明显慢于堆文件，从而在高效数据检索和维护文件顺序的复杂性之间做出权衡。
However, sorted files pose challenges for maintenance, particularly when inserting records. To insert a record of an individual aged 15, we must place it on the correct page and shift subsequent records accordingly, including potentially moving some to a new page if space constraints are met. This can involve a substantial amount of work, making insertions significantly slower compared to heap files, thereby presenting a trade-off between efficient data retrieval and the complexity of maintaining the file's order.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154955705.png" alt="image-20240421154955705" style="zoom:50%;" /> 

**1️⃣**DBMS model the cost of all operations

**2️⃣**The cost is typically expressed in the number of page accesses (or disk I/O operations - to bring data from disk to memory)

- 1 page access (on disk) $==1 \mathrm{l} / \mathrm{O}$ (used interchangeably)

```
从磁盘读取数据到内存会产生一定的代价，数据库用 I/O 操作来衡量这种代价。I/O 操作表示将数据从磁盘传输到内存的过程，这种传输以页为单位进行。因此，每次从磁盘获取数据都相当于一次 I/O 操作。因此，当数据库模拟操作成本时，会考虑到这个指标，将“I/O”和“页访问”作为同义词使用。这意味着从磁盘访问的每个页都相当于一次 I/O 操作，这是数据库系统领域用来评估数据检索和存储过程效率的标准衡量标准。
Accessing data stored on a disk to bring it into memory incurs a cost, which databases quantify in terms of I/O operations. An I/O operation represents the process of transferring data from the disk to memory, and this transfer occurs in units of pages. Consequently, each time data is fetched from the disk, it equates to a single I/O operation. Thus, when databases model the cost of operations, they do so with this metric in mind, using the terms "I/O" and "page access" synonymously. This means that each page access from the disk is considered equivalent to one I/O operation, a standard measure used within the realm of database systems to evaluate the efficiency of data retrieval and storage processes.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421154955705.png" alt="image-20240421154955705" style="zoom:50%;" /> 

**1️⃣**DBMS model the cost of all operations

**2️⃣**The cost is typically expressed in the number of page accesses (or disk I/O operations - to bring data from disk to memory)

- 1 page access (on disk) $==1 \mathrm{l} / \mathrm{O}$ (used interchangeably)

****3️⃣**Example**: If we have a table of 100 records, and each page can store 10 records, what would be the cost of accessing the entire file

```
为了确定最具成本效益的数据存储方式，数据库管理系统 (DBMS) 会模拟所有操作的成本，成本通常用从磁盘到内存传输数据所需的页访问次数（即 I/O 操作次数）来衡量。一次从磁盘的页访问相当于一次 I/O 操作，并且该指标在成本建模中可以互换使用。例如，对于一个包含 100 条记录的表，其中每个页可以容纳 10 条记录，我们可以通过考虑这些操作的基本成本含义来直观地理解访问整个文件所需的成本。
To ascertain the most cost-effective data storage option, a DBMS will model the cost of all operations, where cost is typically gauged by the number of page accesses—or I/O operations—required to transfer data from disk to memory. A single page access from the disk equates to one I/O operation, and this metric is employed interchangeably in cost modeling. For example, with a table comprising 100 records, where each page can accommodate 10 records, the cost of accessing the entire file can be intuitively understood by considering the basic cost implications of these operations.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155409793.png" alt="image-20240421155409793" style="zoom:50%;" /> 

**1️⃣**DBMS model the cost of all operations

**2️⃣**The cost is typically expressed in the number of page accesses (or disk I/O operations - to bring data from disk to memory)

- 1 page access (on disk) $==1 \mathrm{l} / \mathrm{O}$ (used interchangeably)

**3️⃣**Example: If we have a table of 100 records, and each page can store 10 records, what would be the cost of accessing the entire file

**3️⃣**==Answer: For 100 records we have 10 pages in total (100/10), thus the cost to access the entire file is $10 \mathrm{I} / \mathrm{O}$ (or 10 pages)==

```
回到我们的成本模型，访问一个包含 100 条记录的表（每个页面容纳 10 条记录）的操作将需要 10 次 I/O 操作，相当于访问 10 个页面。这很简单，我们只需将总记录数除以每页的记录数即可。结果是 10 页，很容易理解。随着我们对这个主题的深入学习，我们将研究更复杂的例子，但是理解这个基本概念至关重要。它是以后掌握更复杂概念的基础直觉。
Returning to our cost model, the operation to access a table of 100 records—where each page holds 10 records—will require 10 I/O operations, equivalent to accessing 10 pages. This is straightforward: we simply divide the total number of records by the number of records per page. The outcome, 10 pages, is quite intuitive. As we progress through the subject, we'll delve into more intricate examples, but this basic understanding is vital. It's an essential intuition that will greatly aid in grasping more complex concepts later on.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155840161.png" alt="image-20240421155840161" style="zoom:50%;" /> 

**1️⃣**Example: Find all records with ages between 20 and 30 , for the file that has $B$ pages. Consider both alternative: having an unsorted and sorted file. What would be the cheapest cost?

**2️⃣**$20<$ age $<30$, num pages =$B$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155947657.png" alt="image-20240421155947657" style="zoom:50%;" /> 

```
让我们扮演数据库的角色，并通过一个简单示例来理解搜索成本。在这个例子中，我们将要在一个包含未知页数（记为 B）的文件中查找年龄在 20 到 30 岁之间的记录。我们需要评估使用未排序文件或已排序文件的成本效益。在这个例子中，我用 B 等于 4 页来演示了这种情况，并比较了未排序的堆文件和已排序的文件。当我们查看堆文件时，我们立即会看到记录的排列方式没有任何特定的顺序。
Let's step into the role of a database and tackle a simple example where we are searching for records with ages between 20 and 30 in a file consisting of an unknown number of pages, denoted as 'B'. We need to evaluate the cost-effectiveness of using either an unsorted or a sorted file. In this instance, I've demonstrated the situation with 'B' equating to four pages, comparing an unsorted heap file and a sorted file. When we look at the heap file, we immediately see that there's no specific order to how the records are arranged.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155840161.png" alt="image-20240421155840161" style="zoom:50%;" /> 

**1️⃣**Example: Find all records with ages between 20 and 30 , for the file that has $B$ pages. Consider both alternative: having an unsorted and sorted file. What would be the cheapest cost?

**2️⃣**$20<$ age $<30$, num pages =$B$

**3️⃣**Heap file(no order)=B;

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155947657.png" alt="image-20240421155947657" style="zoom:50%;" /> 

```
在一个包含 "B "页数的文件中查找所有年龄在 20 岁至 30 岁之间的记录时，我们必须同时考虑无排序（堆）和排序的文件结构。对于没有任何特定排序的堆文件，成本必然是 "B "页，因为必须检查每条记录，以确定其值是否在所需的年龄范围内。这种方法要求我们依次查看每条记录，因此必须扫描所有页面，而且不知道何时才能完成。
When determining the most cost-effective way to find all records with ages between 20 and 30 within a file that contains 'B' number of pages, we have to consider both unsorted (heap) and sorted file structures. For a heap file, which lacks any specific ordering, the cost will invariably be 'B' pages because every record must be checked to ascertain whether its value falls within the desired age range. This method requires us to review each record in turn, making it necessary to scan all pages without knowing when we might be done.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155840161.png" alt="image-20240421155840161" style="zoom:50%;" /> 

**1️⃣**Example: Find all records with ages between 20 and 30 , for the file that has $B$ pages. Consider both alternative: having an unsorted and sorted file. What would be the cheapest cost?

**2️⃣**$20<$ age $<30$, num pages =$B$

**3️⃣**Heap file(no order)=B;

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155947657.png" alt="image-20240421155947657" style="zoom:50%;" /> 

```
相比之下，对于排序文件，我们可以利用有序结构来执行二进制搜索。这种高效的搜索算法会将搜索空间分成两半，检查中点以确定哪边包含所需的年龄范围，然后在这两半空间内继续搜索。这一过程大大减少了我们需要访问的页面数量，在查找特定年龄范围时，页面数量可能远远少于 "B"。
In contrast, with a sorted file, we can leverage the ordered structure to perform a binary search. This efficient searching algorithm splits the search space in half, examines the midpoint to determine which side of the split contains the desired age range, and then continues the search within that half. This process dramatically reduces the number of pages we need to access, which can potentially be far less than 'B' when looking for a specific age range.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155840161.png" alt="image-20240421155840161" style="zoom:50%;" /> 

**1️⃣**Example: Find all records with ages between 20 and 30 , for the file that has $B$ pages. Consider both alternative: having an unsorted and sorted file. What would be the cheapest cost?

**2️⃣**$20<$ age $<30$, num pages =$B$

**3️⃣**Heap file(no order)=B;

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421160337090.png" alt="image-20240421160337090" style="zoom:50%;" />  

```
我把四页纸分成两页进行分析，重点是以 20 作为阈值。鉴于我搜索的是大于 20 的值，我相信它们会在右侧找到。因此，没有必要考虑左侧，因为肯定不会有我要找的值。按照这种类似于二进制搜索算法的方法，我将有条不紊地缩小搜索范围，直到穷尽我感兴趣的范围。
I've divided my four pages into two for analysis, focusing on the value of 20 as the threshold. Given that I am searching for values greater than 20, I am confident that they will be found on the right-hand side. Therefore, there's no need to consider the left-hand side since it's certain that the values I seek won't be there. Following this approach, akin to a binary search algorithm, I'll proceed by methodically narrowing down the search area until I exhaust the range I'm interested in.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421155840161.png" alt="image-20240421155840161" style="zoom:50%;" /> 

**1️⃣**Example: Find all records with ages between 20 and 30 , for the file that has $B$ pages. Consider both alternative: having an unsorted and sorted file. What would be the cheapest cost?

**2️⃣**$20<$ age $<30$, num pages =$B$

**3️⃣**Heap file(no order)=B;

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421160555379.png" alt="image-20240421160555379" style="zoom:50%;" /> 

**4️⃣**Sorted file(exploit order)=log2B

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421160659424.png" alt="image-20240421160659424" style="zoom: 50%;" /> 

```
在评估搜索操作的成本时，使用排序文件效率要高得多，因为成本由 B 的以 2 为底的对数决定。原因很简单：搜索过程中的每次除以 2 在对数上都对应于单个步骤。因此，B 的以 2 为底的对数本质上将比 B 本身更小。因此，为了搜索效率，我更倾向于使用排序文件而不是堆文件，因为排序文件在搜索复杂度上提供了对数优势，从而可以更快地检索数据。
In assessing the cost of search operations, using a sorted file is significantly more efficient, given that the cost is determined by log base 2 of B. The reasoning is straightforward: each division by two in the search process corresponds to a single step in logarithmic terms. Consequently, log base 2 of B will inherently be a smaller number than B itself. Therefore, for the purpose of search efficiency, I would favor a sorted file over a heap file, since the former provides a logarithmic advantage in search complexity, leading to faster data retrieval.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324225411474.png" alt="image-20240324225411474" style="zoom:50%;" /> 

```
Now, let's talk about indices. OK? And they are very important. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421160847030.png" alt="image-20240421160847030" style="zoom:50%;" /> 

**1️⃣**Sometimes, we want to retrieve records by specifying the values in one or more fields, e.g.,

1. Find all students in the "CIS" department
2. Find all students with a gpa $>3$

**2️⃣**An index is a data structure built on top of data pages used for efficient search. The index is built over specific fields called **search key fields**. 

E.g. we can build an index on GPA, or department name.

1. The index speeds up selections on the search key fields
2. Any subset of the fields of a relation can be the search key for an index on the relation
3. Note: Search key is not the same as key (e.g., doesn't have to be unique)

```
已排序的文件确实是数据库搜索的基石。尽管它们很有用，但维护排序文件却代价不菲。这就是为什么数据库经常采用替代方法，例如索引的原因。索引是建立在文件之上的辅助结构，可以加快数据检索过程。这些结构不仅仅是任何字段; 它们专门建立在选定的字段或称为搜索键字段的键属性之上。
Sorted files are indeed the cornerstone of database searches. Despite their usefulness, maintaining sorted files can be quite costly. That's why databases often employ an alternative method, such as indexes. Indexes are auxiliary structures that are layered on top of files to expedite data retrieval processes. These structures are not just any fields; they are specifically built over chosen fields or key attributes known as search key fields.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421160847030.png" alt="image-20240421160847030" style="zoom:50%;" /> 

**1️⃣**Sometimes, we want to retrieve records by specifying the values in one or more fields, e.g.,

1. Find all students in the "CIS" department
2. Find all students with a gpa $>3$

**2️⃣**An index is a data structure built on top of data pages used for efficient search. The index is built over specific fields called **search key fields**. 

E.g. we can build an index on GPA, or department name.

1. The index speeds up selections on the search key fields
2. Any subset of the fields of a relation can be the search key for an index on the relation
3. Note: Search key is not the same as key (e.g., doesn't have to be unique)

```
让我们澄清一下搜索键字段的含义 - 它们不同于“键”，尽管命名相似，但不要混淆它们。索引建立在任何属性子集上，这些子集就是我们所说的搜索键字段。考虑我们需要找到来自“CIS”系的所有学生或所有 GPA 高于 3 的学生的情况。在这种情况下，索引非常宝贵，可以大大促进和加速搜索过程。现在让我们来看看这些索引是如何构建的以及它们如何在实践中发挥作用。
Let's clarify what we mean by search key fields—they're distinct from 'keys' and should not be confused with them despite the similar nomenclature. An index is constructed over any subset of attributes, and these subsets are what we refer to as the search key fields. Consider the scenarios where we need to find all students from the "CIS" department or all students with a GPA above 3. In these instances, indexes are invaluable, greatly facilitating and accelerating the search process. Let's now examine how these indexes are constructed and how they function in practice.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
这个例子说明了一种基本的索引类型，称为 B+ 树索引，我们稍后将详细讨论它。它的结构围绕搜索键，在本例中是 GPA。这表示索引建立在 GPA 列之上。现在，让我们来看一下您在框中看到的描述。索引本质上由一个数据条目数组组成，旨在简化检索符合特定搜索标准的数据记录。让我们解开并澄清这个概念。
This example illustrates a basic type of index known as the B+ tree index, which we will delve into shortly. It's structured around the search key, in this case, the GPA. That signifies that the index is constructed over the column of GPAs. Now, let's turn to the description you see outlined in the box. An index essentially consists of an array of data entries, designed to streamline the retrieval of data records corresponding to a specific search criterion. Let's unpack and clarify this concept.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
底部的蓝色部分是我们的数据文件，其中包含存储有数据记录的原始页面。数据记录保存在数据页上。建立在数据文件之上的索引文件，在此例中，包含数据条目 - 底部称为数据条目的那些数字以及索引中叶数据条目位于索引底部的对应值。叶值包括搜索键，例如，数据条目 1.2 表示 GPA 值，并指示包含此 GPA 的实际记录的确切位置。
At the bottom of this display is our data file, marked in blue. This file holds the original pages with data records stored within. Data records are kept on data pages. Built atop the data file is the index file, which in this context, comprises data entries—those figures at the bottom termed data entries and the corresponding values in the index where the leaf data entries are positioned at the index's base. The leaf values include the search key, for instance, the data entry of 1.2 represents a GPA value and indicates the precise location of the actual record that holds this GPA.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
这里展示的图表就是一个 B+ 树结构的例子。可以把它想象成一个二叉树，但要强大得多——这就是为什么我把它比喻成“打了激素的二叉树”。这种增强来自于它具有多分支的特性，不像简单的二叉树每个节点都只分两个分支，左边的值永远小于节点的值，右边的值大于或等于节点的值。
The diagram presented here exemplifies a B+ tree structure. Think of it as a binary tree, only much more robust—that's why I liken it to a binary tree on steroids. The enhancement comes from its multiple branching conditions, unlike the simple binary tree where each node branches into two, and the left value is always less than the node's value, with the right value being greater or equal.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
B+ 树的顶端是根节点，也称为目录，为了清楚起见，我会交替使用这两个术语。我们从根节点开始搜索，在那里评估搜索键的值。例如，任何小于 2 的值都向左分支，而介于 2 和 2.5 之间以及 2.5 和 3 之间的值则遵循这些特定的指针，引导我们找到对应的数据条目。
At the pinnacle of the B+ tree is the root, also referred to as the directory, and I'll use these terms interchangeably for clarity. We initiate our search from the root, where we evaluate the search key values. For example, anything less than the value of two branches off to the left, while values ranging between two and 2.5, and 2.5 and three follow these specific pointers, guiding us to the corresponding data entries.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
该插图描绘了一个 B+ 树索引。可以把它想象成一个具有多分支（不仅仅是两个分支）的增强型二叉树。左边的值始终小于中间值，右边的值大于或等于中间值。搜索从顶部开始，我们称之为根节点或目录。对于小于 2 的 GPA，请遵循左侧指针。介于 2 和 2.5 之间的 GPA 可以通过遵循下一个指针找到，等等。
This illustration depicts a B+ tree index. Think of it as a binary tree enhanced with multiple branching—not just two. The values on the left are always less than the middle value, and the right values are greater than or equal. Starting at the top, which we call the root or directory, the search begins. For a GPA less than two, follow the left-hand pointer. GPAs between two and 2.5 are found by following the next pointer, and so on.
```
$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161545368.png" alt="image-20240421161545368" style="zoom:50%;" /> 

An index contains a collection of **data entries**, and supports effi retrieval of **data records** matching a given **search condition**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421161700519.png" alt="image-20240421161700519" style="zoom:50%;" /> 

```
当搜索介于 2 和 2.4 之间的 GPA 时，我们从顶部开始导航，识别到介于 2.2 和 2.4 之间的 GPA 位于此处，然后沿着该指针找到第一个符合我们条件的 2.2 的数据条目。我们将继续这个过程，沿着指向数据记录的指针，直到达到我们范围的上限。一旦遇到超出我们范围的值（例如 2.7），我们就知道要停止，因为所有后续数据都将超出范围。这种效率对于包含大量记录的数据库来说至关重要，因为它可以显着提高查询性能。请记住，B+ 树用于根据 GPA 排序的结构允许进行这种有效的基于范围的搜索和检索。
When searching for a GPA between two and 2.4, we navigate from the top, recognizing that GPAs between 2.2 and 2.4 are here, following this pointer to the first data entry at 2.2, which meets our criteria. We continue this process, following pointers to the data records, until reaching the upper limit of our range. Upon hitting a value like 2.7, which exceeds our range, we know to stop, as all subsequent data will be out of scope. This efficiency is vital in databases with vast numbers of records, as it significantly enhances query performance. Remember, the B+ tree's structure for sorting on the GPA allows this effective range-based search and retrieval.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324231708904.png" alt="image-20240324231708904" style="zoom:50%;" /> 

```
Now, let's see uh what types of fitnesses we can have. 
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162346324.png" alt="image-20240421162346324" style="zoom:50%;" /> 

Classification based on various factors:

- Clustered vs. Unclustered
- Primary vs. Secondary
- Single Key vs. Composite
- Indexing technique:
  - Tree-based, hash-based, other

```
索引可以根据几个关键因素进行分类：聚集或非聚集索引、主键或辅助索引、使用单个键或复合键，以及索引技术 - 基于树或基于哈希。这种分类有助于区分数据库系统中索引的类型和功能。我们将单独探索这些分类。这很重要，因为这些术语在互联网、教科书和专业讨论中普遍存在。一些参考文献可能只提到主键或辅助索引，而另一些参考文献可能会将索引区分为聚集索引或非聚集索引。理解这些术语可以确保您自信地驾驭主题并充分理解数据库中索引分类的讨论。
Indexes can be classified based on several key factors: whether they are clustered or unclustered, primary or secondary, use a single key or a composite key, and the indexing technique—tree-based or hash-based. This classification helps to distinguish the type and functionality of an index in a database system. We’ll explore these classifications individually. This is important because these terms are prevalent across the internet, in textbooks, and in professional discussions. Some references may simply mention primary or secondary indexes, others might distinguish indexes as clustered or unclustered. Understanding these terms ensures you can confidently navigate the subject matter and fully grasp the discussions around index classification in databases.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162455006.png" alt="image-20240421162455006" style="zoom: 50%;" /> 

**Clustered vs. unclustered**: If order of **data records** is the same as the order of **index data entries**, then the index is called clustered index. Otherwise is unclustered.

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162644005.png" alt="image-20240421162644005" style="zoom:50%;" /> 

```
集中式与否取决于数据记录的顺序是否与索引数据项的顺序相对应。如果堆的数据记录与叶子页中索引项的顺序一致，则该索引为聚集索引。否则，它是非聚集的。插图左侧显示了一个聚集的 B+ 树索引，其中页面内的数据项顺序反映了堆中数据记录的顺序。右侧为非聚集 B+ 树索引示例，虽然数据项是排序的，但关联的数据记录并遵循特定顺序。这是聚集索引和非聚集索引的主要区别。三角形图是 B+ 树的抽象表示，常用于教科书中，表示从根节点到包含数据项的叶节点的导航过程。该图表简化了 B+ 树的结构：左侧表示聚集排列，右侧表示非聚集排列。
Clustering is determined by whether the order of data records corresponds with the order of index data entries. If a heap’s data records match the sequence of index entries in the leaf pages, that’s a clustered index. Otherwise, it's unclustered. The left side of the illustration shows a clustered B+ tree index where the order of data entries within a page reflects the sequential order of data records in the heap. When it comes to an unclustered B+ tree index, depicted on the right, while the data entries are sorted, the associated data records do not follow a particular order. This is the primary distinction between clustered and unclustered. The triangular diagrams are abstract representations of B+ trees, commonly used in textbooks to symbolize the navigation through the tree, from the root down to the leaf nodes where the data entries reside. The diagram simplifies the B+ tree's structure: the left signifies a clustered arrangement, and the right, unclustered.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162803204.png" alt="image-20240421162803204" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162822085.png" alt="image-20240421162822085" style="zoom:50%;" /> 

```
让我们仔细观察数据库文件顶部的 B+ 树索引是如何构建数据的。搜索始于根节点（也称为目录），然后到达内部节点，最终到达底部的叶节点。正如我之前提到的，这些叶节点包含数据项，它们通过指针或记录 ID 与数据相关联，这些指针或记录 ID 精确地指向所需数据的存放位置。如图所示，这个顺序经过精心排序，从而优化了搜索过程。然而，这种结构在数据更新过程中会带来复杂性，需要对相应的索引进行更改，而这又是一项繁重的操作。
Zooming into the details of how data is structured, we observe the B+ tree index built atop our heap file. It starts with the root, or directory, where every search begins. This leads to internal nodes and, ultimately, to the leaf nodes at the bottom. These leaf nodes contain data entries linked with pointers or record IDs, which I mentioned at the beginning of the lecture, pinpointing the exact location of the desired data. As shown, the sequence is meticulously ordered, optimizing the search process. However, this structure introduces complexities during data updates, necessitating alterations in the corresponding index, which is an extensive operation.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162930816.png" alt="image-20240421162930816" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162937573.png" alt="image-20240421162937573" style="zoom:50%;" />  

```
现在让我们来看看这里描绘的非聚集索引，索引本身的结构仍然和我们之前讨论的一致：它有根节点、内部节点和叶节点。然而，区别之处在于我们从叶节点跟踪数据项到堆文件时的表现。在这里，数据没有特定的存储顺序，这使得我们无法预先确定找到所需记录的路径。这一点在搜索一系列值时尤其重要，因为在搜索效率方面，一种类型的索引可能比另一种类型具有明显的优势，这一点将在我们更深入地研究该主题时变得更加明显。
Turning our attention to the unclustered index depicted here, the structure of the index itself remains consistent with what we’ve discussed: there's a root, internal nodes, and leaf nodes. However, the distinguishing feature arises when we trace the data entries from the leaf nodes to the heap file. Here, the data is stored in no particular order, leaving us without a predetermined path to locate the desired record. This aspect becomes particularly critical when searching for a range of values, as one type of index may offer a significant advantage over the other in terms of search efficiency, which will become apparent as we delve deeper into the topic.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162951520.png" alt="image-20240421162951520" style="zoom:50%;" /> 

**1️⃣**A data file can have a clustered index on at most one search key combination (i.e. we cannot have multiple clustered indexes over a single table)

**2️⃣**Cost of retrieving data records through index varies greatly based on whether index is clustered (cheaper for clustered)

**3️⃣**Clustered indexes are more expensive to maintain (require file reorganization), but are really efficient for range search

```
在聚集方面，存在一些固有的特性。一个数据文件只能对单个搜索键组合拥有一个聚集索引，这意味着单个表上不可能存在多个聚集索引。这种限制是由于聚集索引的性质所致，它将行的物理顺序与索引对齐，因此只允许一种这样的排序顺序。
In terms of clustering, certain properties are inherent. A data file may only have a clustered index on a single search key combination, meaning multiple clustered indexes on a single table are not possible. This constraint is due to the nature of clustered indexes, which align the physical order of rows with the index, thus only one such ordering is feasible.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162803204.png" alt="image-20240421162803204" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162822085.png" alt="image-20240421162822085" style="zoom:50%;" /> 

```
如果我们重新审视这个概念，就会明显发现为什么不能在同一张表上拥有两个聚集索引。聚集索引的本质在于数据输入的顺序，它维护着已排序页面的线性序列。引入第二个聚集索引将需要一个完全不同的顺序，这对于一组页面来说是不可行的。例如，想象一下，如果一个页面同时要按年龄和薪水排序；它根本无法同时容纳两种不同的顺序。因此，双重聚集索引是不可能的。
If we revisit the concept, it becomes apparent why we cannot have two clustered indexes on the same table. The essence of a clustered index lies in the order of data entry, maintaining a linear sequence of sorted pages. Introducing a second clustered index would necessitate an entirely different order, which is infeasible for a single set of pages. Imagine, for instance, if a page were to be simultaneously sorted by age and salary; it simply cannot accommodate two distinct orders simultaneously. Hence, the impossibility of dual clustered indexes.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162951520.png" alt="image-20240421162951520" style="zoom:50%;" /> 

**1️⃣**A data file can have a clustered index on at most one search key combination (i.e. we cannot have multiple clustered indexes over a single table)

**2️⃣**Cost of retrieving data records through index varies greatly based on whether index is clustered (cheaper for clustered)

**3️⃣**Clustered indexes are more expensive to maintain (require file reorganization), but are really efficient for range search

```
在讨论检索数据记录的成本时，重要的是要注意这些成本可能差别很大。目前，只需提及在搜索特定值时，聚类索引通常比非聚类索引更具成本效益。不过，它们的维护成本可能相当高。我们将在后面的主题中深入探讨这些特性，进一步阐明它们的特点。
When discussing the cost of retrieving data records, it's important to note that these costs can vary greatly. For the time being, it suffices to mention that clustered indexes are generally more cost-effective than unlu when searching for specific values. However, they can be quite expensive to maintain. We will delve deeper into these properties later in the subject, shedding further light on their characteristics.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162803204.png" alt="image-20240421162803204" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162822085.png" alt="image-20240421162822085" style="zoom:50%;" /> 

```
考虑到非聚集索引，如前所述，插入数据的成本相当高。这是因为我们必须在页面中移动数据，并可能需要调整索引，从而导致大量的开销。尽管如此，通过这种索引进行搜索是非常高效的。举例来说，假设我们的年龄值范围在20到30岁之间，这个条件筛选出了300名符合条件的学生。假设我们每页存储100名学生，由于数据是完全排序的，一旦索引定位到满足条件的第一个记录，我们只需要访问三页即可检索所有300名学生。与其他场景相比，这种有针对性的方法显著减少了访问的页面数量。
Considering an unclustered index, as previously mentioned, inserting data incurs significant cost. This is because we must shift data within the pages and potentially adjust the index as well, resulting in substantial overhead. Nonetheless, searching through this index is highly efficient. To illustrate, imagine we have a range of values for age between 20 and 30, a condition that yields 300 qualifying students. Assuming we store 100 students per page, due to the fully sorted nature of the data, once the index locates the first record satisfying the condition, we need only access three pages to retrieve all 300 students. This targeted approach significantly reduces the number of pages accessed compared to other scenarios.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162803204.png" alt="image-20240421162803204" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162822085.png" alt="image-20240421162822085" style="zoom:50%;" /> 

```
然而，值得注意的是，虽然非聚集索引在搜索速度方面表现出色，但其维护成本可能非常高，尤其是对于频繁的数据插入操作。重新排列数据页面和索引本身的需要增加了显著的复杂性，并可能影响整体性能。因此，在评估非聚集索引的适用性时，必须权衡搜索效率和维护开销之间的平衡。
However, it's worth noting that while unclustered indexes excel in search speed, their maintenance cost can be prohibitive, especially for frequent data insertions. The need to rearrange both data pages and the index itself adds significant complexity and can impact overall performance. Thus, when evaluating the suitability of an unclustered index, it's crucial to consider the balance between search efficiency and maintenance overhead.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162930816.png" alt="image-20240421162930816" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421162937573.png" alt="image-20240421162937573" style="zoom:50%;" />  

```
现在想象一下，我们正在使用完全相同的条件搜索相同的 300 名学生。 和聚集索引一样，我们再次在根节点找到相关数据，并且由于数据是排序的，这带来了优势。 但是，对于非聚集索引，对于每个符合条件的学生，我们都必须跳转才能找到他们特定的数据特征。 由于没有保证的顺序，我们可能需要跳转并访问这 300 名学生的 300 个不同的页面。 将其与聚集索引场景中访问的三个页面进行比较，很明显哪种方法更快。 这才是成本差异真正所在的地方。
Imagine now that we are searching for the same 300 students, under the exact same conditions. Once again, we locate the relevant data in the root, and just like in the case of the clustered index, the data is sorted, which is advantageous. However, in the case of an unclustered index, for each qualifying student, we must jump to find their specific data features. Since there is no guaranteed order, we could potentially need to jump and access 300 distinct pages for these 300 students. Compare this to the three pages accessed in the clustered index scenario, and it becomes clear which approach is faster. This is where the cost difference truly lies.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163403492.png" alt="image-20240421163403492" style="zoom:50%;" /> 

- (Approximated) cost of retrieving records found in range scan:

**1️⃣**Clustered: cost $\approx \#$ pages in data file with matching records

**2️⃣**Unclustered: cost $\approx \#$ of matching index data entries (data records)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163451903.png" alt="image-20240421163451903" style="zoom:50%;" /> 

```
我刚才引导你们的逻辑将在评估备选方案时得到应用。对于聚集索引，成本是通过数据文件中包含匹配记录的页数来估算的。稍后我们将深入研究实际的公式。目前，我们正在建立一种直觉，这将有助于我们在以后的成本决策中，特别是在非聚集索引方面。对于非聚集索引，成本是通过匹配的索引数据条目或数据记录的数量来估计的。这是因为在第一个场景中，300名学生被放置在三页内，一旦访问这些页面，我们就完成了。一切都是排序的，所以没有必要进一步或更深入地检查值。使用树找到范围后，我们可以简单地访问所需的数据。然而，在非聚集索引的情况下，即使数据条目是排序的，每个条目也可能产生IO操作。在我们的示例中，这相当于300页。
The logic I have guided you through will be applied when costing the alternatives. In the case of a clustered index, the cost is approximated by the number of pages in the data file containing matching records. We will delve into the actual formulas later. Currently, we are building an intuition that will aid in costing decisions, especially when it comes to unclustered indexes. For unclustered indexes, the cost is estimated by the number of matching index data entries or data records. This is because, in the first scenario, where 300 students are placed within three pages, once these pages are accessed, we are done. Everything is sorted, so there's no need to go further or deeper to examine values. Using the tree to find the range, we can simply access the required data. However, in the case of unclustered indexes, even though the data entries are sorted, each entry potentially incurs an IO operation. In our example, that amounts to 300 pages.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163403492.png" alt="image-20240421163403492" style="zoom:50%;" /> 

- (Approximated) cost of retrieving records found in range scan:

**1️⃣**Clustered: cost $\approx \#$ pages in data file with matching records

**2️⃣**Unclustered: cost $\approx \#$ of matching index data entries (data records)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163451903.png" alt="image-20240421163451903" style="zoom:50%;" /> 

```
此外，这种区别突出了聚集索引的效率。一旦定位到相关页面，所有必要的数据都可以以最小的努力访问。相反，对于非聚集索引，每个数据条目可能需要单独的IO操作，从而增加成本。在评估不同索引策略的性能和成本效益时，这种理解至关重要。
Continuing on, this distinction highlights the efficiency of clustered indexes. With a clustered index, once the relevant pages are located, all the necessary data is accessible with minimal effort. Conversely, with an unclustered index, each data entry may require a separate IO operation, leading to increased costs. This understanding is crucial when evaluating the performance and cost-effectiveness of different indexing strategies.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163623963.png" alt="image-20240421163623963" style="zoom:50%;" /> 

- Primary index includes the table's primary key
- Secondary is any other index
- Properties:
- Primary index never contains duplicates
- Secondary index may contain duplicates

```
在讨论主索引和次索引分类时，需要注意的是，主索引是指建立在主键上的索引，而次索引则涵盖其他类型的索引。这个术语虽然常用，但往往会让学生感到困惑。然而，区别其实很简单：由于主索引是建立在主键上的，因此它永远不会包含重复项，因为主键本身就不允许重复。另一方面，次索引可能包含重复项。这种结构上的差异可能会对数据库性能和成本产生影响，具体取决于特定的用例。
When discussing primary and secondary index classification, it's important to note that a primary index refers to an index created over a primary key, while a secondary index encompasses any other type of index. This terminology is commonly used and can often lead to confusion among students. However, the distinction is quite simple: a primary index, being built on the primary key, will never contain duplicates as primary keys themselves do not allow duplicates. On the other hand, a secondary index may contain duplicates. This difference in structure can have implications for database performance and costing, depending on the specific use case.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163811064.png" alt="image-20240421163811064" style="zoom:50%;" /> 

**1️⃣**An index can be built over a combination of search keys

**2️⃣**Data entries in index sorted by search keys

**3️⃣**- Examples:
1. Index on <age, sal>
2. Index on <sal, age>
3. Efficient to answer:
    age $=12$ and sal $=10$
    age $=12$ and sal $>15$​

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163906832.png" alt="image-20240421163906832" style="zoom:50%;" /> 

```
索引也可以建立在复合搜索键上，这被称为复合索引，以区别于仅覆盖单个列的简单索引。那么，复合索引是什么样的呢？这里，我展示了两个索引示例：一个建立在年龄和薪水的组合上，另一个建立在薪水和年龄的组合上。你可以看到，我们有一组搜索键的组合。这就是复合索引。在屏幕的右侧，我展示了一个数据页面的示例。为了简化，假设我们只有一个包含四条按姓名排序的记录的数据页。这仅仅是为了说明目的。
An index can also be built over a composite search key, which is then called a composite index, as opposed to a simple index that covers a single column. So, what does a composite index look like? Here, I'm showing two examples of indexes: one built on the combination of age and salary, and the other on the combination of salary and age. As you can see, we have a combination of search keys. That's a composite index. On the right-hand side of the screen, I'm showing an example of just one data page. For simplicity, imagine we have only one data page with four records sorted by name. This is just for illustrative purposes.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163811064.png" alt="image-20240421163811064" style="zoom:50%;" /> 

**1️⃣**An index can be built over a combination of search keys

**2️⃣**Data entries in index sorted by search keys

**3️⃣**- Examples:

1. Index on <age, sal>
2. Index on <sal, age>
3. Efficient to answer:
   age $=12$ and sal $=10$
   age $=12$ and sal $>15$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164052741.png" alt="image-20240421164052741" style="zoom:50%;" /> 

```
我正在构建两个索引。一个主要按年龄排序，薪水作为次要标准；另一个则主要按薪水排序，然后按年龄排序。这些索引以垂直方式展示，旋转了90度，这是教科书中常见的表示方法。这不应该引起混淆；它只是一种不同的可视化方式。从根节点开始，我们遍历树结构，直到遇到完全排序的数据条目。然后，我们利用指针从一个数据条目导航到另一个，定位所需的信息。想象一下，这些索引是以二叉B+树的形式实现的。
I am constructing two indices. One sorts the values primarily based on age, with salary as the secondary criterion, while the other sorts primarily by salary and then by age. These indices are represented vertically, rotated by 90 degrees, a common presentation found in textbooks. This shouldn't cause confusion; it's simply a different visualization. Starting from the root, we traverse the tree structure until we encounter fully sorted data entries. We then utilize pointers to navigate from one data entry to another, locating the desired information. Imagine that these indices are implemented as binary B+ trees.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163811064.png" alt="image-20240421163811064" style="zoom:50%;" /> 

**1️⃣**An index can be built over a combination of search keys

**2️⃣**Data entries in index sorted by search keys

**3️⃣**- Examples:

1. Index on <age, sal>
2. Index on <sal, age>
3. Efficient to answer:
   age $=12$ and sal $=10$
   age $=12$ and sal $>15$
4. Efficient to answer:
   age $=12$ and sal $=10$
   age $=12$ and sal $>15$



<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164052741.png" alt="image-20240421164052741" style="zoom:50%;" /> 

```
“按年龄和薪水排序”意味着什么呢？它表示数据首先按年龄排序，然后在每个年龄组内再按薪水排序。例如，考虑到年龄为12岁的情况，数据将首先按年龄排序，然后在这个年龄组内，薪水将被排序，比如说10在20之前。对于另一个索引来说，主要排序是按薪水进行的。请注意，如果存在重复项（在本场景中不存在），它们也将按年龄排序。这两种类型的索引便于高效搜索，例如查找所有12岁的人或具有特定薪水或年龄和薪水组合标准的人。搜索过程涉及跟随树结构定位所需的值，如12，然后利用指针检索相关数据。一旦找到所需的年龄，我们就可以停止搜索，因为数据已完全按年龄排序，此点之后的任何内容都将大于指定的年龄。
What does "sorted on age and salary" imply? It means the data is primarily sorted by age, and within each age group, it is further sorted by salary. For instance, considering an age of 12, the data would first be sorted by age, and then within that age group, salaries would be sorted, say 10 comes before 20. In the case of the other index, the primary sorting is done by salary. Note that if duplicates existed, which they don't in this scenario, they would be sorted by age as well. These two types of indices facilitate efficient searches, such as finding all 12-year-olds or those with a specific salary or a combination of age and salary criteria. The search process involves following the tree structure to locate the desired value, such as 12, and then utilizing pointers to retrieve the associated data. Once the desired age is found, we can stop searching because the data is fully sorted by age, and anything beyond that point would be greater than the specified age.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421163811064.png" alt="image-20240421163811064" style="zoom:50%;" /> 

**1️⃣**An index can be built over a combination of search keys

**2️⃣**Data entries in index sorted by search keys

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164221294.png" alt="image-20240421164221294" style="zoom:50%;" /> 

```
当我们仅搜索高于特定阈值（如70）的薪水时，无法高效地使用第一个索引来加速分析。尽管技术上可以使用这个索引，但它不会提高搜索速度。背后的原因是数据主要按年龄排序，没有直接显示薪水的顺序。因此，我们将被迫检查每个条目并遵循相关的指针，这实质上相当于访问全部数据。因此，出于我们的目的，使用此特定索引没有任何优势。相反，对于薪水超过70的情况，第二个索引（主要按薪水排序，其次按年龄排序）将是有益的，因为它允许进行更直接和高效的搜索。
We cannot efficiently use the first index to accelerate analysis when searching solely for salaries above a certain threshold, such as 70. While technically possible to employ this index, it wouldn't enhance the search speed. The reason behind this is that the data is primarily sorted by age, providing no direct indication of the salary order. Therefore, we would be compelled to examine each entry and follow the associated pointers, essentially amounting to accessing the entirety of the data. Hence, there's no advantage in using this particular index for our purpose. Conversely, for salaries exceeding 70, the second index—sorted primarily by salary and secondly by age—would be beneficial, as it allows for a more direct and efficient search.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164304681.png" alt="image-20240421164304681" style="zoom:50%;" /> 

**1️⃣**Hash-based index:

1. Represents index as a collection of buckets. Hash function maps the search key to the corresponding bucket.
   - $\mathbf{h}(r$.search_key $)=$ bucket in which record $r$ belongs

2. Good for equality selections

**2️⃣**Example: Hash-based index on (sal)

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" />

```
最后的分类依赖于用于创建索引的数据结构。在这里，我展示了一个基于哈希的索引的例子。索引可以是树状的，也可以是哈希的；后者基于哈希表。对于不熟悉的人来说，哈希表本质上是一系列桶，其中哈希函数将搜索键值映射到它们各自的桶中。这个哈希函数可以是任何接受一个输入值并产生另一个值的函数。在数据库中，模运算是一种常见且典型的哈希函数。
The final classification relies on the data structure used to create an index. Here, I present an example of a hash-based index. Indices can be tree-based or hash-based; the latter is founded on hash tables. For those unfamiliar, hash tables are essentially collections of buckets where a hash function maps search key values to their respective buckets. This hash function can be any function that takes an input value and produces another. In databases, the modulo operation is a common and typical hash function.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164304681.png" alt="image-20240421164304681" style="zoom:50%;" /> 

**1️⃣**Hash-based index:

1. Represents index as a collection of buckets. Hash function maps the search key to the corresponding bucket.
   - $\mathbf{h}(r$.search_key $)=$ bucket in which record $r$ belongs

2. Good for equality selections

**2️⃣**Example: Hash-based index on (sal)

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" />



```
让我们深入了解哈希的工作原理。想象一下，我们有很多桶，每个桶内都有一组记录。这些记录没有特定的顺序；它们只是因为哈希函数为它们输出了相同的值而被分组在一起。在屏幕的底部，我展示了一个哈希函数：模四。这意味着我们除以四并存储余数。因此，当除以四时余数为零的值会被放在零号桶中。在二进制中，这可能看起来像3000或5004。所有除以四后余数为零的值都会放在这里。想象一下，如果我有四个桶，在最后一个桶中，余数是三。
Let's delve into how hashing works. Imagine we have numerous buckets, and within each bucket, there's a group of records. These records have no particular order; they're simply grouped because the hash function outputted the same value for them. At the bottom of the screen, I'm showing a hash function: modulo four. This means we divide by four and store the remainder. So, bucket zero holds values with a remainder of zero when divided by four. In binary, this might look like 3000 or 5004. Everything that, when divided by four, leaves a remainder of zero ends up here. Imagine if I had four buckets and in the last one, the remainder was three.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164304681.png" alt="image-20240421164304681" style="zoom:50%;" /> 

**1️⃣**Hash-based index:

1. Represents index as a collection of buckets. Hash function maps the search key to the corresponding bucket.
   - $\mathbf{h}(r$.search_key $)=$ bucket in which record $r$ belongs

2. Good for equality selections

**2️⃣**Example: Hash-based index on (sal)

 <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" />


```
在这里，我举例说明了一个基于哈希的索引，它是建立在一个数据文件上的。在左侧，我们有三个页面，每个页面有三条记录，显示姓氏、年龄和薪水。为了简洁起见，我只展示了两个桶，但你可以推断出其他两个桶中的内容。一号桶包含像6000、5004和5004这样的值。这是我的索引文件。搜索过程类似：一旦我在索引中找到符合条件的数据条目，我就会跟随指针。这无论我们使用的是树状索引还是哈希索引都适用。关键的区别在于，我的数据在索引文件中并不是完全排序的；相反，它是根据哈希函数进行分组的。
Here, I'm illustrating an example of a hash-based index built over a data file. On the left, we have three pages, each with three records showing surname, age, and salary. For brevity, I'm only displaying two buckets, but you can extrapolate what's in the other two. Bucket one contains values like 6000, 5004, and 5004. This is my index file. The search process is similar: once I find the qualifying data entry in the index, I follow the pointer. This applies regardless of whether we have a tree-based or hash-based index. The key difference is that my data isn't fully sorted in the index file; instead, it's grouped based on the hash function.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164304681.png" alt="image-20240421164304681" style="zoom:50%;" /> 

**1️⃣**Hash-based index:

1. Represents index as a collection of buckets. Hash function maps the search key to the corresponding bucket.
   - $\mathbf{h}(r$.search_key $)=$ bucket in which record $r$ belongs

2. Good for equality selections

**2️⃣**Example: Hash-based index on (sal)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" /> 

``` 
Find Sal = 2007
2007 mod 4 = 3 go to Buck.4
```

```
哈希索引在精确数据检索方面非常有效。让我来解释一下基于哈希的索引搜索是如何工作的。例如，当搜索2007年的特定工资时，初始步骤包括应用哈希函数。在这种情况下，我使用模数为四的哈希函数，它产生的余数为三，指向第四号桶。我进入该桶并开始搜索。我扫描其中的值，检查是否有与2007年对应的项。在遇到第二个条目“1, 2007”时，我跟随相关的指针并检索相关数据，即6003。一旦我穷尽了该桶内的所有可能性，我就可以确信没有其他桶包含2007年的工资数据。即使存在许多其他桶，也许多达一千个，这种确定性也是成立的。哈希过程确保所有与2007年工资相关的值都仅位于此桶内，从而显著提高了搜索效率。
Hash indices are highly effective for precise data retrieval. Let me explain how a hash-based index search works. When searching for a specific salary from 2007, for instance, the initial step involves applying a hash function. In this case, I use a modulus of four, which yields a remainder of three, indicating bucket number four. I proceed to that bucket and commence my search. I scan the values within, checking if any correspond to 2007. Upon encountering the second entry, “1, 2007,” I follow the associated pointer and retrieve the relevant data, which is 6003. Once I’ve exhausted the possibilities within this bucket, I can confidently conclude that no other bucket contains the salary data for 2007. This certainty holds true even if there are numerous other buckets, perhaps as many as a thousand. The hashing process ensures that all values related to the salary for 2007 are located exclusively within this bucket, significantly enhancing search efficiency.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164304681.png" alt="image-20240421164304681" style="zoom:50%;" /> 

**1️⃣**Hash-based index:

1. Represents index as a collection of buckets. Hash function maps the search key to the corresponding bucket.
   - $\mathbf{h}(r$.search_key $)=$ bucket in which record $r$ belongs

2. Good for equality selections

**2️⃣**Example: Hash-based index on (sal)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421164445500.png" alt="image-20240421164445500" style="zoom: 80%;" /> 

``` 
Find Sal = 2007
2007 mod 4 = 3 go to Buck.4
```

```
然而，当搜索一系列值时，哈希索引的效用会降低。考虑一个搜索条件涉及1,000,000到2,000,000之间工资的场景。在这种情况下，哈希索引就不那么有用了，因为数据是按组分类的，而不是排序的。因此，像3000这样的值可能会出现在第一桶中，而3001可能会出现在第二桶中，依此类推。这种模式在所有桶中都是一致的，这意味着为了找到所需的工资范围，我可能需要访问每个桶。虽然从技术上讲是可行的，但与简单访问完全排序的数据文件相比，这种方法的效率要低得多。因此，虽然哈希索引在精确数据检索方面表现出色，但它们可能不是基于范围的搜索的最佳选择。
However, the utility of hash indices diminishes when searching for a range of values. Consider a scenario where the search criteria involve salaries between 1,000,000 and 2,000,000. In this instance, the hash index is less helpful because the data is grouped rather than sorted. Consequently, values such as 3000 may be found in bucket one, while 3001 could be in bucket two, and so on. This pattern continues across all buckets, meaning that to locate the desired salary range, I would potentially need to access every bucket. Although technically feasible, this approach is far less efficient compared to simply accessing a fully sorted data file. Therefore, while hash indices excel at precise data retrieval, they may not be the optimal choice for range-based searches.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421165634992.png" alt="image-20240421165634992" style="zoom:50%;" /> 

**1️⃣**Tree-based index:

- Underlying data structure is a binary $\left(\mathrm{B}^{+}\right)$tree. Nodes contain pointers to lower levels (search left for lower, right for higher).
  Leaves contain data entries sorted by search key values.
- Good for range selections
- So far we have shown those

**2️⃣**Example: Tree-based index on (age)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421165728855.png" alt="image-20240421165728855" style="zoom: 33%;" /> 

``` 
现在，让我们深入探讨一下基于树的索引的工作机制。本质上，到目前为止，我所展示的索引是以二进制B+树数据结构为基础的。在这种设置中，树中的节点包含指向较低级别的指针。搜索时，人们会向左寻找较小的值，向右寻找较大或相等的值。这种结构对于范围选择或等值条件查询特别有利。为了进一步说明这一点，我在屏幕底部加入了一段书中的摘录。我的目的是揭开这个概念的神秘面纱，并向你们展示不同的人可能会如何表示一棵树。你可以看到，我们有一个根，它在不同的表示中是一致的。我们还有指针。在这个例子中，我们有一个值为12的节点，指针指向1、12、78、19和56。这看起来可能有点不寻常，但你可以在书中找到这个确切的例子。
Now, let's delve into the mechanics of a tree-based index. Essentially, what I've been presenting thus far is an index whose foundation lies in the binary B+ tree data structure. In this setup, the nodes within the tree house pointers that lead to lower levels. When searching, one would go left for smaller values and right for larger or equal values. This structure is particularly advantageous for range selection or equal condition queries. To illustrate this further, I've included an excerpt from a book at the bottom of the screen. My aim is to demystify this concept and show you how different individuals might represent a tree. As you can see, we have a root, which is consistent across representations. We also have pointers. In this example, we have a value of 12, and the pointers lead to 1, 12, 78, 19, and 56. It may seem slightly unusual, but you can find this exact example in the book.
```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421165634992.png" alt="image-20240421165634992" style="zoom:50%;" /> 

**1️⃣**Tree-based index:

- Underlying data structure is a binary $\left(\mathrm{B}^{+}\right)$tree. Nodes contain pointers to lower levels (search left for lower, right for higher).
  Leaves contain data entries sorted by search key values.
- Good for range selections
- So far we have shown those

**2️⃣**Example: Tree-based index on (age) ==Find age>39==

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421165933628.png" alt="image-20240421165933628" style="zoom:50%;" /> 

```
让我们设想一个场景，我们正在搜索所有年龄超过39岁的个体。请记住，我们的数据文件包含诸如姓氏、年龄和薪水等信息，它是一个独立的实体。在这里，我们以三个数据页面和相应的索引结构为例进行说明。我们的搜索从索引的根开始。我们确定39位于12到78的范围内，这引导我们进入下一层级。随后，我们确定39位于19和56之间，进一步引导我们深入树状结构。最后，我们将搜索范围缩小到33到44之间。此时，我们开始检查数据，从33开始，逐渐进展到40。正是在这个关键时刻，我们迅速发现了感兴趣的相关数据。
Let's envision a scenario where we are searching for all individuals whose age exceeds 39. Recall that our data file, containing information such as surname, age, and salary, exists as a distinct entity. Here, we illustrate with an example of three data pages and a corresponding index structure. Our search commences at the root of the index. We ascertain that 39 falls within the range of 12 to 78, leading us to the next level. Subsequently, we determine that 39 lies between 19 and 56, guiding us further down the tree. Finally, we narrow our search to the range of 33 to 44. At this point, we commence our data inspection, commencing with 33 and progressing to 40. It is at this juncture that we swiftly discover the pertinent data of interest.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421170013313.png" alt="image-20240421170013313" style="zoom: 50%;" />  

**1️⃣**Many alternative file organizations exist, each appropriate in some situation

**2️⃣**If selection queries are frequent, sorting the file or building an index is important

**3️⃣**Index is an additional data structure (i.e. file) introduced to quickly find entries with given key values

- Hash-based indexes only good for equality search
- Sorted files and tree-based indexes best for range search; also good for equality search
- Files rarely kept sorted in practice (because of the cost of maintaining them); $\mathrm{B}+$ tree index is better

```
在本次讲座中，我们探讨了多种适用于特定情境的1025组织方式。尽管我们将来会深入探讨这些主题，但我当前的目标是帮助你们直观地理解它们的高级操作。这个基础将为你们未来的学习做好准备。如果你们在区分哈希索引和树基索引等方面遇到困难，我强烈建议你们参考我提供的示例。这些图示允许你们与不同的B树和哈希基索引进行交互，揭示它们底层的数据结构。尽管它们很复杂，但要记住，我们的重点是分析这些替代方案的成本效益。即使有些概念仍然难以理解，但掌握成本是如何确定的至关重要。
In this lecture, we have explored a variety of manual 1025 organizations, each suitable for specific situations. While we will delve deeper into these topics in the future, my current objective was to cultivate an intuitive understanding of their high-level operations. This foundation will prepare you for what lies ahead. If you encounter difficulties distinguishing between, say, hash indexes and tree-based indexes, I urge you to refer to the examples I've provided. These illustrations allow you to interact with different B-tree and hash-based indexes, revealing their underlying data structures. Despite their complexity, remember that our focus is on analyzing the cost-effectiveness of these alternatives. Even if some concepts remain elusive, grasping how costs are determined is crucial.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240421170013313.png" alt="image-20240421170013313" style="zoom: 50%;" />  

**1️⃣**Many alternative file organizations exist, each appropriate in some situation

**2️⃣**If selection queries are frequent, sorting the file or building an index is important

**3️⃣**Index is an additional data structure (i.e. file) introduced to quickly find entries with given key values

- Hash-based indexes only good for equality search
- Sorted files and tree-based indexes best for range search; also good for equality search
- Files rarely kept sorted in practice (because of the cost of maintaining them); $\mathrm{B}+$ tree index is better

```
必须牢记的是，哈希基索引擅长精确搜索，但由于数据未排序，因此不适用于范围查询。相反，已排序的文件或基于树的结构既便于范围搜索，也适用于精确查找。值得注意的是，在实践中，由于相关成本较高，数据文件很少保持完全排序。在真实世界的数据库中，通常使用非聚集索引来加速搜索。非聚集索引的优点在于无需更新底层数据。相反，新条目可以放置在下一个可用位置，这提供了便利和效率。
It's essential to remember that hash-based indices excel at precise searches but are unsuitable for range queries as the data lacks sorting. Conversely, sorted files or tree-based structures facilitate range searches while also accommodating precise lookups. It's worth noting that in practice, data files are rarely kept fully sorted due to the associated costs. In real-world databases, unclustered indexes are commonly employed to expedite searches. The advantage of unclustered indexes is that they obviate the need to update the underlying data. Instead, new entries can be placed in the next available location, offering convenience and efficiency.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324233238220.png" alt="image-20240324233238220" style="zoom:50%;" /> 

```
本次讲座的真正重点在于理解这些不同的替代方案。随着学习的深入，我们将练习如何运用这些索引，并判断在特定情况下哪种索引最为适用。这种全面的方法将引导我们在接下来的课程中逐步深入探讨这一主题。感谢您的聆听，期待下次与您再会。
What is truly essential from this lecture is understanding these various alternatives. As we progress, we will practice applying these indexes and determine which index is best suited for specific cases. This comprehensive approach will guide us as we gradually delve deeper into the topic over the upcoming lectures. Thank you for your attention, and I look forward to seeing you next time.
```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$
