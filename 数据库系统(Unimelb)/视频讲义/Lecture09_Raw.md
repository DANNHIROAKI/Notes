![image-20240324030501179](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324030501179.png)Hello, everyone, and welcome back to our ninth lecture on database systems. Today we are rounding off our discussion on SQL concepts, addressing a few significant yet pending statements. But let's not forget, SQL proficiency comes with practice—this cannot be emphasized enough. You will find a plethora of exercises in the four or five workshops ahead, and I highly recommend that you tackle these challenges independently. While I have provided a foundational understanding of SQL today and highlighted the approach to tackling SQL problems, the next step is up to you. Practice is essential, as it is with any programming language. Our lab materials are tailored with exercises for all levels, from beginners to experts, ensuring there’s something for each one of you. I strongly encourage you to work through these exercises on your own. And should you find yourself puzzled or uncertain, do not hesitate to reach out for guidance.

大家好，欢迎回到我们关于数据库系统的第九讲。今天，我们将结束对SQL概念的讨论，涉及一些重要但尚未讨论的声明。但请记住，掌握SQL的关键在于实践——这一点绝不能过分强调。在未来的四到五个研讨会中，你会发现大量的练习题，我强烈推荐你独立面对这些挑战。虽然我今天为你们提供了SQL的基础理解，并强调了解决SQL问题的方法，但下一步取决于你们。像任何编程语言一样，练习是必不可少的。我们的实验材料根据不同水平定制了练习，从初学者到专家，确保每个人都有适合自己的内容。我强烈鼓励你独立完成这些练习。如果你感到困惑或不确定，请不要犹豫，寻求指导。









![image-20240324030614557](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324030614557.png)

Today, we will be expanding our SQL knowledge further by exploring additional Data Manipulation Language (DML) statements that are yet to be discussed. We'll dive into subqueries, a powerful tool often used in database operations, and delve into the intricacies of INSERT, UPDATE, and DELETE commands. We'll also take a look at a few Data Definition Language (DDL) commands, and I'll clarify the use of Data Control Language (DCL) commands. Following that, our focus will shift to the problem-solving aspect of SQL. I'll share with you a strategic approach, a systematic method if you will, which will aid you in tackling any SQL task with confidence.

今天，我们将通过探索尚未讨论的数据操纵语言（DML）陈述来进一步扩展我们的SQL知识。我们将深入了解子查询，这是数据库操作中经常使用的一个强大工具，并将深入探讨INSERT、UPDATE和DELETE命令的复杂性。我们还将看一看几个数据定义语言（DDL）命令，并且我将澄清数据控制语言（DCL）命令的使用。在此之后，我们的重点将转移到SQL的问题解决方面。我将与您分享一个战略方法，一个系统的方法，它将帮助您有信心地应对任何SQL任务。. 







![image-20240324030701374](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324030701374.png)

In our journey through SQL, it's crucial to bear in mind a few key points, frequently highlighted by common questions from students. First, SQL keywords are case-insensitive, meaning that it doesn't matter whether you write them in uppercase or lowercase—though I recommend capitalizing them for clarity. When it comes to table names, however, they are sensitive to the operating system you are working with. This sensitivity means that, unlike keywords, the case you use for table names does matter—especially if you're working on Mac or Linux systems, where table names are case sensitive. For Windows, the case is often insensitive, but this is not a rule set in stone. It's best to verify this through direct experimentation with your own database setups.

我们在学习SQL的过程中，必须记住一些关键点，这些点常常是学生们常问的问题。首先，SQL关键字对大小写不敏感，这意味着你可以用大写或小写来写它们——虽然我建议用大写来提高清晰度。然而，当涉及到表名时，它们对你工作的操作系统是敏感的。这种敏感性意味着，与关键字不同，你用于表名的大小写确实很重要——特别是如果你在Mac或Linux系统上工作，那里的表名是区分大小写的。对于Windows，大小写通常是不敏感的，但这不是一条铁律。通过直接用你自己的数据库设置来验证这一点是最好的。

Secondly, attribute names are not case sensitive, providing more flexibility in how you write them in your queries. Lastly, there is a common misconception that SQL is solely for data retrieval. While that is its primary use, SQL is capable of much more. It allows for arithmetic operations, for example—you can perform simple calculations within your queries. An example I'm sharing here is a straightforward arithmetic expression in a `SELECT` statement, which SQL will execute and return as a result in a single row and column. This capability demonstrates SQL's versatility beyond data retrieval, extending to various operations you can perform on the data you manage.

其次，属性名不区分大小写，这为你在查询中如何编写它们提供了更多的灵活性。最后，有一个常见的误解认为SQL仅用于数据检索。虽然这是它的主要用途，但SQL能做的还有很多。例如，它允许进行算术操作——你可以在查询中进行简单的计算。这里我分享的一个例子是一个简单的算术表达式在`SELECT`语句中，SQL将执行它并返回一个单行和列的结果。这种能力展示了SQL超越数据检索的多功能性，扩展到你可以对你管理的数据执行的各种操作。







![image-20240324031018629](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031018629.png)

In the realm of SQL, understanding operators is foundational for building accurate queries. We have already discussed the basic comparison operators like equality, greater than, less than, and their respective variations that include equality. It's also important to know that the syntax for expressing inequality can vary; it might be '<>' or '!=', depending on the DBMS you're working with. It's imperative to test these out on your system to see which one is appropriate for your specific database software.

在SQL领域内，理解操作符是构建准确查询的基础。我们已经讨论了基本的比较操作符，如等于、大于、小于及其包含等于的变体。还需要知道，表达不等式的语法可能会有所不同；可能是'<>'或'!='，这取决于您使用的数据库管理系统（DBMS）。必须在您的系统上进行测试，以确定哪一个适用于您特定的数据库软件。

In addition to comparison operators, we've introduced logical operators such as AND and OR. A crucial operator I haven't yet mentioned is NOT, which is used for negation. These operators allow us to construct complex expressions, which can be further clarified by the use of parentheses to denote precedence. Despite the inherent priority of these operators—NOT being the highest, followed by AND, then OR—I recommend using parentheses to explicitly define the evaluation order. This approach aligns with relational algebra principles and ensures that SQL processes the predicates as intended. For instance, in the example 'SELECT * FROM Furniture WHERE ((Type="Chair" AND Colour = "Black") OR (Type = "Lamp" AND Colour = "Black"))', the use of parentheses directs SQL to evaluate the conjunctions first ('Type="Chair" AND Colour = "Black"') and then the disjunction ('OR'), thereby facilitating a search for either a black chair or a black lamp.

除了比较操作符，我们还介绍了逻辑操作符，如AND和OR。我还没有提到的一个关键操作符是NOT，用于否定。这些操作符允许我们构建复杂的表达式，使用括号可以明确优先级，从而进一步明确表达。尽管这些操作符有内在的优先级——NOT的优先级最高，其次是AND，然后是OR——我建议使用括号来明确定义评估顺序。这种方法与关系代数原理一致，并确保SQL按预期处理谓词。例如，在例子'SELECT * FROM Furniture WHERE ((Type="Chair" AND Colour = "Black") OR (Type = "Lamp" AND Colour = "Black"))'中，使用括号指示SQL首先评估合取('Type="Chair" AND Colour = "Black"')，然后是析取('OR')，从而方便地搜索黑色的椅子或黑色的灯。













![image-20240324031112820](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031112820.png)

When working with set operations in SQL, particularly within the context of relational algebra, it's crucial to utilize parentheses to ensure clarity in your queries. We've covered the concepts of UNION and SET DIFFERENCE in INTERSECTION, but it's worth noting that in MySQL—the database system we’re using for this course—only UNION and UNION ALL are supported. UNION ALL specifically combines all records from both queries, including duplicates. So, if there are identical records in both sets, UNION ALL will include each copy in the final result set. While the necessity for retaining duplicates with UNION ALL might be rare and its practical use cases less common, it remains a viable option within MySQL's capabilities, and understanding its function is part of grasping the full scope of SQL's set operations.

在SQL中使用集合操作时，尤其是在关系代数的背景下，使用括号来确保查询的清晰度是至关重要的。我们已经讨论了UNION和SET DIFFERENCE与INTERSECTION的概念，但值得注意的是，在我们这门课程中使用的MySQL数据库系统中，只支持UNION和UNION ALL。具体来说，UNION ALL将合并来自两个查询的所有记录，包括重复的记录。因此，如果两个集合中有完全相同的记录，UNION ALL会在最终的结果集中包括每一个副本。虽然使用UNION ALL保留重复项的必要性可能很少见，其实际用例也不常见，但它仍然是MySQL能力范围内的一个可行选项，理解其功能是掌握SQL集合操作全部范围的一部分。











![image-20240324031211856](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031211856.png)

In this UNION example, we're working with two distinct yet structurally similar SQL statements. The example illustrates that contrary to the belief held by some students, a union in SQL is not limited to just two tables; it can be applied to any two result sets, provided they are union compatible. This means the involved sets must have the same number of columns with corresponding data types. Here, both expressions are structured to return 'Name' and 'EmployeeType', where the first query fetches hourly employees and the second one salaried employees. Despite the queries joining different tables, the output schema remains consistent, allowing for a valid union operation. This results in a composite output, conceptually a table, where hourly and salaried employees are combined. It's essential to remember that this 'table' is a temporary construct, used solely for displaying the query results, not a persistent structure stored in the database.

在这个UNION的例子中，我们使用的是两个结构上相似但内容不同的SQL语句。这个例子说明，与一些学生的想法相反，SQL中的union不仅限于两个表；它可以应用于任何两个结果集，前提是它们是union兼容的。这意味着涉及的集合必须有相同数量的列和相应的数据类型。在这里，两个表达式都设计为返回'Name'和'EmployeeType'，其中第一个查询获取按小时计酬的员工，第二个查询获取固定薪资的员工。尽管这些查询连接的是不同的表，输出架构保持一致，允许进行有效的union操作。这导致了一个综合输出，从概念上讲是一个表，其中小时工和固定工资员工被组合在一起。重要的是要记住，这个'表'是一个临时结构，仅用于显示查询结果，而不是数据库中存储的持久结构。











![image-20240324031309041](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031309041.png)

Query nesting in SQL is a technique you'll likely find yourself using quite often. It's particularly popular among students, who may use nested queries extensively, even when other options are available. A nested query is essentially a SELECT statement placed within a larger query to generate a result set that the outer query can utilize. These inner queries are executed first, and their results are then employed by the outer query. We'll look at specific examples of how this works, but the principle to remember is that in relational algebra, any SELECT query yields a result in the form of a table. This is what makes nesting possible: the output of the inner query is a table that seamlessly integrates into the surrounding SQL statement, allowing for a high degree of complexity and precision in your database operations.

在SQL中，查询嵌套是一个你可能会经常使用的技术。这在学生中特别受欢迎，即使有其他选项可用，他们也可能广泛使用嵌套查询。嵌套查询本质上是一个放置在更大查询内部的SELECT语句，用来生成外部查询可以使用的结果集。这些内部查询首先被执行，然后它们的结果被外部查询使用。我们将查看这是如何工作的具体例子，但要记住的原则是，在关系代数中，任何SELECT查询都会产生一个表形式的结果。这就是嵌套成为可能的原因：内部查询的输出是一个表，它无缝地整合到周围的SQL语句中，允许在数据库操作中实现高度的复杂性和精确性。











![image-20240324031414181](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031414181.png)

The most common operators used in SQL subqueries are IN, NOT IN, ANY, ALL, and EXISTS. These operators are pivotal for set operations within queries, as tables can be considered sets in relational algebra. IN and NOT IN determine whether a value exists or does not exist in a list returned by a subquery. ANY and ALL check conditions against each value returned: ANY requires at least one value to meet the condition, while ALL requires all values to do so. EXISTS is true if the subquery returns one or more records. Remember, when we say an operator returns true, it means that the condition meets the criteria to include a record in the result set, not that 'true' is displayed as output. It's a signal to include or dis🚗d a record from the final query results. I recommend exploring tutorials, particularly on subqueries and set operations, to strengthen your understanding of these concepts.

在SQL子查询中使用的最常见的操作符是IN、NOT IN、ANY、ALL和EXISTS。这些操作符对于查询中的集合操作至关重要，因为在关系代数中，表可以被视为集合。IN和NOT IN确定一个值是否存在或不存在于子查询返回的列表中。ANY和ALL对返回的每个值检查条件：ANY要求至少有一个值满足条件，而ALL要求所有值都满足条件。EXISTS在子查询返回一个或多个记录时为真。记住，当我们说操作符返回真时，意味着条件满足了包含记录在结果集中的标准，并不是说'真'作为输出显示。这是一个从最终查询结果中包含或丢弃记录的信号。我推荐探索教程，特别是关于子查询和集合操作的部分，以加强你对这些概念的理解。













![image-20240324031512209](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031512209.png)

Let’s examine an example to illustrate the concepts from this lecture, utilizing the auction bids physical model. This model maps out the interactions between buyers and sellers in an auction setting, detailing the artifacts being sold, the offers made by buyers, and whether those offers are accepted. Each offer includes the proposed amount and an acceptance status, allowing us to track the progress of each bid.

这里我们来看一个例子，以阐明本讲中的概念，使用的是拍卖出价的物理模型。这个模型描述了拍卖场景中买家和卖家之间的互动，详细记录了所售艺术品、买家的出价以及这些出价是否被接受。每个出价都包括提议的金额和接受状态，使我们能够追踪每个出价的进展情况。

Now, a thought-provoking question for you: How was this model developed? It's a physical model that emerged from the transformation of our conceptual and logical models. Can you deduce the original model from which it evolved? Initially, there was an inherently many-to-many relationship between artifacts, buyers, and sellers, characterized by attributes like date, amount, and acceptance. In essence, a buyer could purchase multiple artifacts from various sellers, and this intricate relationship, once resolved, resulted in this associative entity in our physical model.

现在，给你一个发人深省的问题：这个模型是如何开发出来的？它是从我们的概念模型和逻辑模型转化而来的物理模型。你能推断出它演变而来的原始模型吗？起初，艺术品、买家和卖家之间本质上存在着多对多的关系，其特征属性包括日期、金额和接受情况。本质上，一个买家可以从不同的卖家那里购买多个艺术品，这一复杂的关系一旦解决，就会在我们的物理模型中形成这个关联实体。











![image-20240324031603845](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031603845.png)

In this dataset, we observe a snapshot of the auction market, featuring three unique artifacts, each associated with individual buyers and sellers. This provides a diverse glimpse into the dynamics of the auction, underscored by the detailed information about various offers made.

在这组数据中，我们观察到拍卖市场的一个快照，展示了三件独特的艺术品，每件都与不同的买家和卖家相关。这为我们提供了对拍卖动态的多样化窥探，各种出价的详细信息更是突显了这一点。







![image-20240324031652283](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324031652283.png)

Let's delve into a practical example using a subquery with the IN statement. The objective here is to retrieve the BuyerID, Name, and Phone number for all bidders on artifact number one. For this, an IN statement is utilized within the SELECT query. This approach efficiently filters and lists the desired information by matching the specified criteria in the subquery.

我们来看一个实际的例子，这个例子使用了带有IN语句的子查询。这里的目标是检索出对第一件艺术品出价的所有竞买者的BuyerID、Name和Phone number。为此，我们在SELECT查询中使用了IN语句。这种方法通过匹配子查询中指定的条件，有效地过滤并列出了所需的信息。











![image-20240324032659387](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032659387.png)

To understand how to retrieve information about bidders for a specific artifact using a subquery, we start by identifying where the required data resides. In this case, the buyer information, including BuyerID, Name, and Phone, is in the 'Buyer' table, while the 'Offer' table holds the bids for artifacts. Here, we’re interested in the bidders for artifact number one. The subquery is crafted to first extract the BuyerIDs from the 'Offer' table where the ArtifactID is one. This inner query returns BuyerIDs one and two, representing the bidders of interest. Subsequently, these IDs are utilized by the outer query to select the corresponding records from the 'Buyer' table. The resulting output displays the details of these buyers.

为了理解如何使用子查询检索特定艺术品的竞标者信息，我们首先需要确定所需数据的存放位置。在这个例子中，包括BuyerID、Name和Phone在内的买家信息存储在'Buyer'表中，而'Offer'表则包含了艺术品的出价信息。这里，我们感兴趣的是艺术品编号1的竞标者。子查询的目的是首先从'Offer'表中提取ArtifactID为1的BuyerIDs。这个内部查询返回了BuyerID 1和2，代表我们感兴趣的竞标者。随后，这些ID被外部查询用来从'Buyer'表中选择相应的记录。结果输出显示了这些买家的详细信息。







![image-20240324032022547](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032022547.png)

Let’s explore another example of using subqueries, this time incorporating the NOT IN clause, which often perplexes students due to its negation aspect. The task at hand is to identify artifacts that have not received any offers. To achieve this, we use the NOT IN clause. The approach is to find artifacts that do have offers and then apply negation to filter those out, effectively leaving us with the ones that have no offers.

让我们来探讨使用子查询的另一个例子，这次是结合了NOT IN语句，这个因为它的否定方面经常让学生感到困惑。我们现在的任务是识别那些没有收到任何出价的艺术品。为了做到这一点，我们使用了NOT IN语句。方法是找到那些确实有出价的艺术品，然后应用否定来过滤掉这些艺术品，有效地留下那些没有出价的艺术品。











![image-20240324032819229](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032819229.png)

In this example, we use a NOT IN subquery to identify artifacts that have not received any offers. The inner clause of our subquery first determines which artifacts have offers by searching for their IDs in the 'Offer' table. The IDs found are artifacts 1 and 2, which do have offers. Then, the outer query takes over, filtering out these IDs from the 'Artifact' table. It effectively excludes artifacts 1 and 2, leaving us with only artifact number 3, which is the pot, as the sole artifact without any offers.

在这个例子中，我们使用了NOT IN子查询来确定哪些艺术品没有收到任何出价。我们子查询的内层语句首先通过在'Offer'表中搜索它们的ID来确定哪些艺术品有出价。找到的ID是艺术品1和2，它们确实有出价。然后，外层查询接管，从'Artifact'表中过滤掉这些ID。它有效地排除了艺术品1和2，只留下了艺术品编号3，也就是罐子，作为唯一没有任何出价的艺术品。











![image-20240324032143168](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032143168.png)

In SQL, it's not mandatory to use the IN clause. There are various ways to retrieve data; aside from using IN, you can also employ EXISTS or JOIN operations. Taking the example we're discussing, the most straightforward method to list the BuyerID, Name, and Phone number for all bidders on artefact 1 would be to use a natural join between the Buyer and Offer tables, with the condition to look for ArtefactID 1. This method is often more efficient because joins are typically faster than nested queries. However, there are scenarios where you might need to resort to nesting due to the complexity or specific requirements of the task at hand.

在SQL中，使用IN子句并不是强制性的。有各种方法可以检索数据；除了使用IN，你还可以使用EXISTS或JOIN操作。以我们讨论的例子来看，列出所有对艺术品1出价的买家的BuyerID、姓名和电话号码最直接的方法是在Buyer和Offer表之间使用自然连接，条件是寻找ArtefactID 1。这种方法通常更高效，因为连接操作通常比嵌套查询要快。然而，有些情况下，你可能需要使用嵌套查询，因为手头的任务的复杂性或特定要求。











![image-20240324032233651](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032233651.png)

Let’s take a look at the `EXISTS` keyword in SQL. `EXISTS` is used to test for the existence of any record in a subquery. For example, to find the BuyerID, Name, and Phone number for all bidders on artifact 1, you might consider using a query with `EXISTS`. The SQL statement provided uses `EXISTS` to select all fields from the Buyer table where there's a corresponding BuyerID in the Offer table for ArtifactID 1. This ensures that only buyers who have made an offer on that particular artifact are selected. The execution of this statement will produce a list of buyers involved with artifact 1, showcasing how `EXISTS` efficiently filters records based on a condition defined in a subquery.

让我们来看看SQL中的`EXISTS`关键字。`EXISTS`用于测试子查询中是否存在任何记录。例如，要列出对文物1出价的所有买家的BuyerID、姓名和电话号码，您可能会考虑使用带有`EXISTS`的查询。所提供的SQL语句使用`EXISTS`从Buyer表中选择所有字段，条件是在Offer表中有一个对应ArtifactID 1的BuyerID。这确保只选择对该特定文物出过价的买家。这个语句的执行将产生一个与文物1有关的买家名单，展示了`EXISTS`如何有效地根据子查询中定义的条件过滤记录。











![image-20240324032953254](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324032953254.png)

Unlike `IN` or `NOT IN` clauses, where the subquery is fully executed before the outer query, the `EXISTS` operator works differently. In the case of `EXISTS`, there's an interplay between the outer and inner queries. For each record in the outer table (Buyers in this example), the inner query (checking Offers table) is executed to see if there are any matching records. If the inner query returns at least one record, the outer query keeps that row; otherwise, it dis🚗ds it and moves on to the next row in the outer table.

与 `IN` 或 `NOT IN` 子句（子查询在外部查询之前完全执行）不同，`EXISTS` 操作符的工作方式有所不同。 对于 `EXISTS`，外部查询和内部查询之间存在相互作用。 对于外部表中的每个记录（本例中的买家），将执行内部查询（检查报价表）以查看是否存在任何匹配的记录。 如果内部查询返回至少一条记录，则外部查询将保留该行； 否则，它会将其丢弃并继续处理外部表中的下一行。

In the example on the slide, we're looking for buyers who have placed bids on artifact number 1. We iterate through each buyer in the Buyer table. For each buyer, we check the Offer table to see if there's a record where the buyer ID matches and the artifact ID is 1.  Only Buyer 1 and Buyer 2 have matching records in the Offer table, so the outer query will return those two rows.

幻灯片中的示例中，我们正在寻找对工件编号 1 竞标的买家。 我们迭代买家表中的每个买家。 对于每个买家，我们都会检查报价表，看看是否存在买家 ID 匹配且工件 ID 为 1 的记录。 只有买家 1 和买家 2 在报价表中具有匹配的记录，因此外部查询将返回这两个行。













![image-20240324033016775](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324033016775.png)

The `EXISTS` statement can be confusing at first, but once you work through a few examples, you'll be able to use it confidently. The key to understanding `EXISTS` is to remember that it only checks for the existence of records, not their specific values.

`EXISTS` 语句乍看之下可能会让人困惑，但一旦你完成几个示例，你就能自信地使用它。 理解 `EXISTS` 的关键是记住它只检查记录是否存在，而不是它们的值。











![image-20240324033058118](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324033058118.png)

In the first section, we discuss the ALL operator. This operator is used to compare a value against a list of values, and it returns true if the value meets all the specified conditions. For example, if we want to find employees whose salaries are greater than a set of benchmark amounts (such as 200, 300, and 400), we use the ALL operator. The condition `sal > ALL (200, 300, 400)` means that the employee's salary must be greater than each of these values—effectively, it must be greater than 400 to satisfy all conditions. While this example simplifies the concept for illustrative purposes, in practice, conditions will often be more complex.

在第一部分，我们讨论了 ALL 操作符。此操作符用于将一个值与一系列值进行比较，如果该值满足所有指定条件，则返回 true。例如，如果我们想找到薪水高于一系列基准值（例如 200、300 和 400）的员工，则可以使用 ALL 操作符。条件 sal > ALL (200, 300, 400) 表示员工的薪水必须大于每个值 - 实际上，它必须大于 400 才能满足所有条件。虽然这个例子简化了概念以供说明，但实际上，条件通常会更复杂。

Moving on to the ANY operator, this allows us to check if at least one of multiple conditions is satisfied. In the context of our salary example, the ANY operator enables us to find employees whose salary is greater than any of the benchmark amounts of 200, 300, or 400. The clause `sal > ANY (200, 300, 400)` is satisfied if the salary is greater than at least one of the mentioned values. The logical equivalent is using the OR operator between each condition—`sal > 200 OR sal > 300 OR sal > 400`. Despite the straightforwardness of the example, the ANY operator can significantly streamline complex queries.

接下来是 ANY 操作符，它允许我们检查多个条件中至少满足一个。回到我们薪水示例的情景，ANY 操作符使我们能够找到薪水高于基准金额 (200、300 或 400) 中任何一个的员工。如果薪水大于所提到的值中至少一个，则满足条件 `sal > ANY (200, 300, 400)`。逻辑等价于在每个条件之间使用 OR 操作符 - `sal > 200 OR sal > 300 OR sal > 400`。尽管示例很简单，但 ANY 操作符可以显着简化复杂查询。

Lastly, the EXISTS operator is used to ascertain if an inner query returns at least one record. In our discussion, it's applied to identify employees who have at least one dependent. The EXISTS condition in a query like `WHERE EXISTS (SELECT * FROM dependents WHERE dependents.empid = employees.empid)` checks for any dependents linked to an employee. If there's at least one match, the result for the outer query will include that employee. This interplay between outer and inner queries is a powerful tool in SQL that can capture nuanced data relationships. Through practice, these operators become indispensable in crafting precise and efficient SQL queries.

最后，EXISTS 操作符用于确定内部查询是否返回至少一条记录。在我们的讨论中，它用于识别拥有至少一个抚养人的员工。查询中的 EXISTS 条件 `WHERE EXISTS (SELECT * FROM dependents WHERE dependents.empid = employees.empid)` 用于检查与员工关联的任何抚养人。如果至少有一个匹配项，则外部查询的结果将包含该员工。外部查询和内部查询之间的这种交互作用是 SQL 中一个强大的工具，可以捕获细致的数据关系。通过练习，这些操作符将成为编写精确高效的 SQL 查询不可或缺的一部分。

These set operations are foundational in SQL, offering immense flexibility to define almost any condition. Embracing them through rigorous practice will certainly sharpen your SQL skills, transforming complex data retrieval into a more intuitive and straightforward task.

这些集合操作是 SQL 的基础，提供了定义几乎任何条件的巨大灵活性。通过严格的实践来掌握它们肯定会提高您的 SQL 技能，使复杂的数据检索变得更加直观和简单。






![image-20240324033228351](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324033228351.png)

Let's delve into some additional aspects of inserting records, specifically the methods that we haven't explored yet. One efficient approach, showcased at the top of the slide, involves using the 'INSERT INTO SELECT FROM' statement. This method effectively duplicates data from an existing table to another. When the intention is to directly clone a table, this method is exceedingly swift because it operates on the entire table at once, as opposed to processing each record individually. This is a straight transfer from one table to another, ensuring speed and efficiency.

让我们深入研究插入记录的一些附加方面，特别是我们尚未探索的方法。幻灯片顶部展示了一种有效的方法，即使用“INSERT INTO SELECT FROM”语句。这种方法实际上可以将数据从一个现有表复制到另一个表。当想要直接克隆表时，这种方法非常快速，因为它一次处理整个表，而不是逐个处理每个记录。这是一种从一个表直接转移到另一个表的简单操作，确保了速度和效率。

When it comes to inserting multiple records, the approach is straightforward yet effective. By employing the 'INSERT INTO' statement, records can be inserted in a single command, with each record enclosed in parentheses and separated by commas. This structure is commonly produced by various data extraction tools from different applications, indicating it's a standard format for organizing data for insertion. It's important to remember that data can be inserted in two ways: by either specifying the attributes for each value, as seen in the bottom example on the slide, or by omitting the attribute names altogether, as in the first example. Both methods are valid and widely used in database management.

在插入多条记录时，方法既简单又有效。通过使用“INSERT INTO”语句，可以在单个命令中插入记录，每个记录都用括号括起来，并用逗号分隔。这种结构通常由来自不同应用程序的各种数据提取工具生成，表明它是用于组织插入数据的标准格式。需要注意的是，数据可以通过两种方式插入：一种是像幻灯片底部的例子一样，为每个值指定属性；另一种是完全省略属性名称，就像第一个例子一样。这两种方法都是有效的，并且在数据库管理中被广泛使用。







![image-20240324033331147](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324033331147.png)

```
The UPDATE statement in SQL is pivotal for modifying existing records within a table, and it's crucial to remember the significance of specifying conditions. Without a WHERE clause, an UPDATE will apply changes across the entire table, which might be intended in some scenarios. However, more often than not, the goal is to target a subset of records. For instance, to increase the hourly rate by 10%, one would use an UPDATE statement on the 'HourlyRate' column, setting it to its current value multiplied by 1.10. This precision ensures only the desired records are updated.
UPDATE 语句在 SQL 中是修改表内现有记录的关键部分，需要注意的是一定要指定条件。如果没有 WHERE 子句，UPDATE 会将更改应用于整个表，这在某些情况下可能是预期的。然而，更常见的情况是只想针对记录的子集进行更新。例如，要将时薪提高 10%，可以使用 UPDATE 语句对“HourlyRate”列进行操作，将其设置为当前值乘以 1.10。这种精确性确保只更新所需的记录。

In executing multiple UPDATE statements, the sequence in which they are run can dramatically affect the outcome. Take, for example, the task of adjusting salaries: we want to increase salaries above $100,000 by 10% and all others by 5%. To implement this, two separate UPDATE statements are needed. The first raises the annual salary by 5% for salaries under $100,000, while the second boosts salaries above that threshold by 10%.
执行多个 UPDATE 语句时，它们的运行顺序会显著影响结果。例如，调整薪酬的任务：我们想将 10 万美元以上的薪酬提高 10%，其他薪酬提高 5%。为了实现这一点，需要两个单独的 UPDATE 语句。第一个语句将低于 10 万美元的年薪提高 5%，第二个语句将高于该阈值的薪酬提高 10%。

The problem arises with the order of execution. If we first apply the 5% increase to salaries below $100,000, individuals on the cusp could inadvertently receive a second raise when their new salary exceeds $100,000 due to the first update. This means someone earning $98,000 would benefit from both increases, which likely isn't the intended effect. Such an oversight highlights why 🚗eful attention must be paid to the order in which updates are processed to avoid unexpected results.
问题出在执行顺序上。如果我们首先将 5% 的涨幅应用于低于 10 万美元的薪酬，那么由于第一次更新后薪酬超过了 10 万美元，处于这一临界点的员工可能会意外地再次获得加薪。这意味着一个原本赚 98,000 美元的人将从两次加薪中受益，但这可能不是预期的效果。这样的疏忽强调了必须仔细注意更新的处理顺序，以避免出现意外结果。





```

















![image-20240324042834099](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324042834099.png)

To address the earlier issue more effectively, SQL provides the CASE statement, which is analogous to the if-then-else logic found in many imperative programming languages. In SQL, the CASE statement allows for conditions to be specified directly within the update process. For instance, when adjusting salaries, the CASE statement can be used to increase the annual salary by 5% for amounts that are less than or equal to $100,000, and by 10% for those above this threshold. This is accomplished by stating the condition followed by the THEN keyword for the desired action, and an ELSE keyword for all other cases, with the entire condition wrapped up by the END keyword.

为了更有效地解决之前的问题，SQL 提供了 CASE 语句，它类似于许多命令式编程语言中使用的 if-then-else 逻辑。在 SQL 中，CASE 语句允许在更新过程中直接指定条件。例如，在调整薪酬时，可以使用 CASE 语句将低于或等于 100,000 美元的年薪增加 5%，将高于该阈值的薪酬增加 10%。实现方法是陈述条件，然后使用 THEN 关键字指定期望的操作， ELSE 关键字用于所有其他情况，整个条件最后用 END 关键字结尾。

This method isn't just limited to update scenarios; the versatility of the CASE statement extends across various SQL commands, including SELECT and INSERT statements. It's an elegant solution that simplifies complex conditional logic within queries. As you'll see in the lab materials, the CASE statement is often employed within SELECT statements to control the flow of data retrieval and manipulation based on specific criteria, enhancing the dynamism and power of SQL querying.

这并不仅仅局限于更新场景；CASE 语句的灵活性扩展到各种 SQL 命令，包括 SELECT 和 INSERT 语句。它是一种优雅的解决方案，可以简化查询中复杂的条件逻辑。正如您将在实验材料中看到的那样，CASE 语句经常用于 SELECT 语句中，根据特定条件控制数据检索和操作的流程，从而增强 SQL 查询的动态性和强大功能。









![image-20240324042949255](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324042949255.png)

In SQL, the REPLACE command is a unique operation that essentially combines the functions of INSERT and UPDATE. It operates just like INSERT, but with a twist: if a row already exists in the table with the same key as the one being inserted, the old row is overwritten with the new one. This command is particularly useful when you're uncertain if a record already exists, as it eliminates the need to separately check before deciding to insert a new record or update an existing one.

在 SQL 中，REPLACE 命令是一种独特的操作，它本质上结合了 INSERT 和 UPDATE 的功能。它就像 INSERT 一样工作，但有一点不同：如果表中已经存在一个与要插入的键相同的行，则旧行将被新行覆盖。此命令在您不确定记录是否已经存在时特别有用，因为它消除了在决定是插入新记录还是更新现有记录之前单独检查的需要。

The DELETE command, on the other hand, is often referred to as dangerous—and for good reason. It's capable of removing records from a table, and without the appropriate conditions, it can wipe clean the entire table's data. Therefore, it's paramount to handle this command with the utmost caution, ensuring you always use it in conjunction with specific conditions to target only the intended records, like deleting all entries where the name is 'Grace'.

另一方面，DELETE 命令通常被称为“危险的” - 这是有充分理由的。它可以从表中删除记录，如果没有适当的条件，它可以清除整个表的数据。因此，必须非常谨慎地处理此命令，确保始终将其与特定条件结合使用，以便仅针对目标记录，例如删除所有名称为“Grace”的条目。

Moreover, it's essential to be aware of the ramifications of DELETE operations within the context of foreign key constraints. For instance, a CASCADE delete will remove not only the targeted records but also any related records in different tables through foreign keys. Conversely, a RESTRICT delete will prevent the removal if any related records exist. These constraints are an integral part of maintaining referential integrity within the database and will be covered in your lab exercises, highlighting the importance of understanding the interrelated nature of your data before performing deletions.

此外，了解 DELETE 操作在外国键约束上下文中的影响也至关重要。例如，级联删除 (CASCADE delete) 不仅会删除目标记录，还会通过外键删除不同表中的任何相关记录。相反，限制删除 (RESTRICT delete) 会阻止删除任何相关记录存在的情况。这些约束是维持数据库内引用完整性不可或缺的一部分，并且将在您的实验练习中涵盖，强调在执行删除之前理解数据之间相互关联的重要性。









![image-20240324043711632](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324043711632.png)

Views in a database function like virtual tables; they don’t physically store data but present it as a result of a query. The primary purpose of views is to simplify user interaction with the database: they abstract the complexity of queries and restrict data visibility according to user permissions. Creating a view involves the 'CREATE VIEW' statement, where you define the view's name followed by a valid SQL query. This defines the view, which thereafter acts like a table, streamlining data access and enhancing security by controlling which parts of the data are exposed to different users. Views are stored in the database as metadata, meaning that while they don’t contain the data itself, they define how data from actual tables should be presented, and once established, they are used just like tables.

数据库中的视图就像虚拟表一样，它们并不物理存储数据，而是根据查询结果呈现数据。视图的主要目的在于简化用户与数据库的交互：它们可以隐藏复杂查询的细节，并根据用户权限限制数据可见性。创建视图需要使用“CREATE VIEW”语句，其中包含视图名称和一个有效的 SQL 查询。这会定义视图，使其以后可以像表格一样工作，既能简化数据访问，又能通过控制向不同用户展示哪些数据来增强安全性。视图作为元数据存储在数据库中，这意味着它们虽然不包含数据本身，但却定义了如何呈现来自实际表格中的数据。一旦创建完成，就可以像使用表格一样使用视图。











![image-20240324045328830](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324045328830.png)

In this example of creating a view, we are consolidating complex data from various types of employees into a single, accessible virtual table named 'EmpPay'. The view is constructed through a sophisticated SQL query that brings together different employee classifications—hourly workers by joining with the 'Hourly' table, salaried employees by joining with the 'Salaried' table, and consultants by joining with the 'Consultant' table. Each of these categories is unified using the UNION command, creating a composite view where the complexities of the underlying data structure are abstracted away. The end result, 'EmpPay', then serves as a simplified interface to access a diverse set of employee compensation data.

这个例子展示了如何创建一个视图，我们将来自不同类型员工的复杂数据整合到一个名为“EmpPay”的易于访问的虚拟表中。该视图通过复杂的 SQL 查询构建，该查询结合了不同的员工分类 - 通过与“Hourly”表连接时薪员工，通过与“Salaried”表连接的固定薪酬员工，以及通过与“Consultant”表连接的顾问。 这些类别中的每一个都使用 UNION 命令进行统一，从而创建一个复合视图，其中隐藏了底层数据结构的复杂性。最终结果“EmpPay”用作简化的接口，可用于访问各种员工薪酬数据。







![image-20240324045839714](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324045839714.png)

With a view like 'EmpPay' created, interacting with complex data sets becomes as straightforward as dealing with a single table. As illustrated, executing a command such as `SELECT * FROM EmpPay` or filtering specific types of employees with `SELECT * FROM EmpPay WHERE EmployeeType = 'H' OR EmployeeType = 'C'` simplifies the retrieval of information. This simplicity belies the complex SQL operations that are executed in the background; the user is spared from this complexity, accessing and manipulating data through an easily comprehensible interface.

创建了像“EmpPay”这样的视图后，与复杂数据集的交互变得和操作单个表格一样简单。正如示例所示，执行诸如 `SELECT * FROM EmpPay` 的命令或使用 `SELECT * FROM EmpPay WHERE EmployeeType = 'H' OR EmployeeType = 'C'` 筛选特定类型的员工会简化信息的检索。这种简单性隐藏了后台执行的复杂 SQL 操作； 用户不必理会这些复杂性，而是可以通过易于理解的界面访问和操作数据。

However, it's worth noting the educational standpoint on the use of views in learning environments. In assignments, particularly, the use of views is often discouraged. The rationale behind this is that students might over-rely on views, using them to overly simplify the construction of SQL queries. Instead of building a comprehensive understanding of SQL through writing complex queries directly, students might fall into the habit of creating multiple layered views, effectively breaking down SQL queries into too many incremental steps. This practice can detract from learning how to craft well-structured and efficient SQL queries, which is a crucial skill in database management.

但是，在学习环境中使用视图需要注意教学目的。特别是对于作业来说，通常不鼓励使用视图。其背后的原因是学生可能会过度依赖视图，使用它们来过度简化 SQL 查询的构建。学生可能会养成创建多层视图的习惯，将 SQL 查询分解成太多增量步骤，而不是通过直接编写复杂查询来建立对 SQL 的全面理解。这种做法可能会削弱学习如何构建结构良好且高效的 SQL 查询的能力，这对于数据库管理来说是一项重要技能。







![image-20240324050018303](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050018303.png)

In the realm of SQL, beyond the CREATE command which is used for creating tables, we have additional commands like ALTER and RENAME that allow us to modify existing database structures. The ALTER command is particularly versatile, enabling the addition or removal of columns within a table. For example, you can add a new attribute to a table with its specific data type using 'ALTER TABLE TableName ADD AttributeName AttributeType', or conversely, remove an attribute using 'ALTER TABLE TableName DROP AttributeName'. 

在 SQL 领域，除了用于创建表的 CREATE 命令之外，我们还有其他命令可以让修改现有的数据库结构，例如 ALTER 和 RENAME。ALTER 命令功能特别强大，可以添加或删除表中的列。例如，您可以使用 'ALTER TABLE 表名 ADD 属性名 属性类型' 向表中添加一个具有特定数据类型的新属性，反之，也可以使用 'ALTER TABLE 表名 DROP 属性名' 删除属性。

The RENAME command is straightforward; it's used to change the name of a table from an existing one to a new one, with the syntax 'RENAME TABLE CurrentTableName TO NewTableName'. These commands, while powerful, must be used with caution due to their potential to significantly alter database schemas.

RENAME 命令很简单; 它用于将表的名称从现有的名称更改为新的名称，语法为 'RENAME TABLE 当前表名 TO 新表名'。 这些命令虽然强大，但由于它们可能显着改变数据库架构，因此必须谨慎使用。







![image-20240324050233113](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050233113.png)

The TRUNCATE command in SQL is used to swiftly remove all records from a table, much like the DELETE statement, but it’s significantly faster. However, this speed comes with a major caveat: a TRUNCATE operation cannot be rolled back. Once you've truncated a table, the only way to recover the data is from a backup, assuming one exists. This underscores the importance of always having a recent backup, a practice that will be covered later in the course.

SQL 中的 TRUNCATE 命令用于快速删除表中的所有记录，类似于 DELETE 语句，但速度要快得多。但是，这种速度伴随着一个重大的警告：TRUNCATE 操作无法回滚。 一旦截断表格，唯一恢复数据的方法就是从备份中恢复（假设存在备份）。这强调了始终拥有最新备份的重要性，这将在本课程后面介绍。

The DROP command is equally potent and perilous. It completely removes a table from the database—both its data and its schema. Unlike TRUNCATE, DROP doesn't just delete records; it eliminates the entire table structure. There is no UNDO command for a DROP operation, so recovery is solely dependent on the existence of a backup. These commands, while useful, demand a high level of caution and an understanding of their irreversible nature.

DROP 命令同样强大且危险。 它会从数据库中完全删除表 - 包括其数据和架构。 与 TRUNCATE 不同，DROP 不仅会删除记录，还会删除整个表结构。 DROP 操作没有撤消命令，因此恢复完全依赖于备份的存在。 虽然这些命令很有用，但它们需要高度谨慎并理解其不可逆转的性质。









![image-20240324050404836](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050404836.png)

Within the realm of database management, the Data Control Language (DCL) encompasses commands that are essential for database administration, often referred to as Database Administrator Statements. Although not all of you might become database administrators, it’s useful to understand the capabilities of DCL. It includes commands such as CREATE USER and DROP USER to manage database access, as well as GRANT and REVOKE for assigning or removing user permissions. Setting passwords is another aspect of this language that you will get to practice, albeit on a basic level.

在数据库管理领域，数据控制语言 (DCL) 包含一些对数据库管理必不可少的命令，这些命令通常被称为数据库管理员语句。尽管并非所有用户都将成为数据库管理员，但理解 DCL 的功能仍然非常有用。它包含用于管理数据库访问的命令，例如 CREATE USER 和 DROP USER，以及用于分配或撤销用户权限的 GRANT 和 REVOKE。设置密码是该语言的另一个方面，您将有机会进行练习（尽管只是基础层面）。

The other subset of commands under DCL is concerned with safeguarding the database's valuable data. These include BACKUP TABLE and RESTORE TABLE, which are critical operations for ensuring that data can be recovered in the event of accidental deletions or other data loss incidents. Alongside these, commands like DESCRIBE tablename, which reveals the schema of a table, and USE db_name, which selects the database to work with, are part of your practical lab exercises. These tools and commands collectively contribute to the proficiency required in managing and protecting data within a database environment.

DCL 下的另一组命令则关注保护数据库的重要数据。这些命令包括 BACKUP TABLE 和 RESTORE TABLE，它们是确保在意外删除或其他数据丢失事件中可以恢复数据的关键操作。除此之外，DESCRIBE table_name（用于揭示表的架构）和 USE db_name（用于选择要工作的数据库）等命令也是您实践实验练习的一部分。这些工具和命令共同构成了在数据库环境中管理和保护数据所需的技能.









![image-20240324050554790](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050554790.png)

Mastering SQL requires a specific mindset, and I'm here to offer some straightforward strategies to enhance your approach to constructing SQL queries. Firstly, always consider the database schema as your roadmap; it's crucial for understanding where data resides and how tables relate to one another. This first step sets the foundation for where to look for the information you need.

掌握 SQL 需要一种特定的思维方式，我将在这里提供一些简单易行的策略来帮助您改进构建 SQL 查询的方法。首先，始终将数据库架构视为您的路线图； 了解数据的位置以及表之间如何相互关联对于理解数据至关重要。第一步是为您需要的信息奠定基础，明确去哪里寻找它。

The next step involves using the structure of the SELECT statement, or any other SQL command, as a template. It's vital that each statement is syntactically correct—it should 'compile', so to speak. Finally, you will fill out the parts of the SELECT statement, tailoring it with the necessary conditions to meet the requirements of your query. By following these steps—mapping out the schema, using statement structures as templates, and then customizing the query with specific conditions—you'll effectively build your SQL queries.

下一步涉及使用 SELECT 语句或任何其他 SQL 命令的结构作为模板。至关重要的是，每个语句在语法上都必须正确 - 它应该可以像编程语言一样被“编译”。最后，您将填写 SELECT 语句的部分，并根据查询需求添加必要的条件对其进行定制。通过遵循这些步骤 - 映射架构、使用语句结构作为模板，然后使用特定条件自定义查询 - 您将可以有效地构建您的 SQL 查询。













![image-20240324050641833](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20240324050641833.png)

In this example, we're faced with a somewhat complex schema comprising five tables that include students, instructors, and the courses they're involved in, mapped through various streams. The given task requires identifying the employers of students who are enrolled in courses at locations with a capacity of more than 20 people, and also identifying these specific locations. Initially, this task may seem daunting, but by breaking it down systematically, the SQL query needed to retrieve this information becomes manageable. The process involves linking the tables to track the locations through the streams and the students to their employers, filtering the results where the location capacity exceeds the specified threshold. This methodical approach simplifies the process of constructing the required SQL statement.

在这个例子中，我们面临着一个稍微复杂的架构，它包含五个表，其中包含学生、教师以及他们通过各种流参与的课程。给定的任务需要识别那些注册了容量超过 20 人的课程的学生的雇主，并识别这些特定的地点。乍一看，这项任务可能令人生畏，但通过系统地分解它，检索这些信息的 SQL 查询变得易于管理。该过程涉及链接表格以通过流跟踪位置并将学生链接到他们的雇主，并过滤容量超过指定阈值的结果。这种有条理的方法简化了构建所需 SQL 语句的过程。







![image-20240324050801227](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050801227.png)

To approach the problem of identifying which employers hire students attending courses at locations with a capacity greater than 20, we start by understanding the query's requirements. We're looking for 'Employer' and 'Location' fields across 'Student', 'Stream', and 'StudentStream' tables, with the condition that the 'Capacity' should be over 20. By mapping the database schema, we ascertain the necessary connections between these tables.

为了识别哪些雇主雇佣了在容量超过 20 人的场地 attending 课程的学生，我们首先需要理解查询的需求。我们正在寻找跨越“学生”、“流”和“学生流”表的“雇主”和“地点”字段，条件是“容量”应大于 20。通过映射数据库架构，我们可以确定这些表之间必要的连接。

We then construct our SQL query using the schema as our guide. We join the 'Student' and 'StudentStream' tables on 'StudentID' and link that to the 'Stream' table to reach the 'Location' and 'Capacity' information. The SELECT statement is formulated to extract 'Employer' and 'Location' from these interconnected tables, ensuring that we only include records where the 'Capacity' exceeds 20.

然后，我们使用架构作为指南来构建 SQL 查询。我们使用“StudentID”连接“学生”和“学生流”表，并将其链接到“流”表以到达“地点”和“容量”信息。SELECT 语句被用来从这些互连的表中提取“雇主”和“地点”，确保我们只包含“容量”超过 20 的记录。

The final query uses INNER JOIN to piece together the related data, or alternatively, NATURAL JOIN could be used where common attributes exist between tables, like 'StudentID' in 'Student' and 'StudentStream'. By 🚗efully constructing this query, following the SQL statement structure—first selecting the desired fields, then specifying the tables to pull from, and lastly applying the condition—we effectively retrieve the required data.

最终查询使用 INNER JOIN 将相关数据拼接在一起，或者也可以在表之间存在公共属性（例如“学生”和“学生流”中的“StudentID”）的情况下使用 NATURAL JOIN。通过仔细构建此查询，遵循 SQL 语句结构 - 首先选择所需字段，然后指定要提取数据的表，最后应用条件 - 我们将有效地检索所需数据。









![image-20240324050919140](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324050919140.png)

You have been assigned the task of finding the phone number of an instructor who teaches a course priced at over $10,000 and in which student ID 202 is enrolled. To truly retain this knowledge, practicing by attempting to solve this challenge independently is essential. Therefore, take a moment to pause and try this exercise, as hands-on experience proves to be the most effective method for learning and remembering how to write SQL queries.

你的任务是找到一位讲授价格超过10,000美元课程的讲师的电话号码，同时学生ID 202也报名参加了这门课程。为了真正掌握这一知识，独立尝试解决这一挑战至关重要。因此，请暂停片刻并尝试进行此练习，因为实践证明，亲身实践是学习和记忆如何编写SQL查询的最有效方法。





![image-20240324051611659](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324051611659.png)
To summarize, we have covered the fundamental SQL statements. Moving forward, I strongly encourage you to engage in two key activities. Firstly, make use of the lab material provided. This serves as a hands-on practice opportunity that you can undertake in the comfort of your own home, progressing at your own pace. Whether you choose to tackle the most complex queries with confidence or start with simpler statements and gradually build your proficiency, we have designed the material to cater to various skill levels in SQL. Secondly, if you encounter any difficulties, I recommend watching our comprehensive SQL video lecture, which covers a wide range of SQL aspects. Ultimately, when it comes to mastering SQL, it boils down to continuous practice.

总结一下，我们已经介绍了基本的SQL语句。接下来，我强烈建议你参与两个关键活动。首先，利用提供的实验材料。这是一个可以在自己舒适的家中进行的实践机会，你可以按照自己的节奏进行。无论你是自信地选择处理最复杂的查询，还是从简单的语句开始并逐步提高熟练度，我们都设计了适合不同SQL技能水平的材料。其次，如果你遇到任何困难，我推荐你观看我们的综合SQL视频讲座，它涵盖了广泛的SQL方面。最终，当涉及到掌握SQL时，归结为持续的实践。











![image-20240324051723354](https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240324051723354.png)

In our examinations, the focus will not be on rote memorization of each SQL statement or the definitions of terms like GROUP BY, HAVING, or nesting. Instead, the emphasis will be on the application of these concepts. You'll be expected to write SQL queries for a variety of use cases, an essential skill that will serve you well in your future 🚗eer. Thank you for your attention, and I look forward to our next session.

在我们的考试中，重点不是死记硬背每条 SQL 语句或 GROUP BY、HAVING 或嵌套等术语的定义。相反，重点将放在这些概念的应用上。您需要为各种使用情况编写 SQL 查询，这是一项基本技能，对您未来的职业生涯大有裨益。感谢您的关注，我期待着下一堂课的到来。