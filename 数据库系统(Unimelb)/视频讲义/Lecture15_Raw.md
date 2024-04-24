$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200018205.png" alt="image-20240420200018205" style="zoom:50%;" /> 

By the end of this lecture, you should be able to:

- Define normalization

- Explain and identify database anomalies

- Define and identify functional dependencies

- Normalize relations to:

  - 1st Normal Form (1NF)

  - 2nd Normal Form (2NF)

  - 3rd Normal Form (3NF)

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200344665.png" alt="image-20240420200344665" style="zoom: 50%;" /> 

What happens if we don’t normalize?

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200413890.png" alt="image-20240420200413890" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200500352.png" alt="image-20240420200500352" style="zoom: 67%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200528649.png" alt="image-20240420200528649" style="zoom:50%;" /> 

**1️⃣**Consider the following denormalized table (relation) :

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200554975.png" alt="image-20240420200554975" style="zoom:50%;" /> 

**2️⃣**Insertion Anomaly: A new course cannot be added until at least one student has enrolled (which comes first student or course?)

**3️⃣**Deletion Anomaly: If student 425 withdraws, we lose all record of course C400 and its fee!

**4️⃣**Update Anomaly: If the fee for course C200 changes, we have to change it in multiple records (rows), else the data will be inconsistent.

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200742972.png" alt="image-20240420200742972" style="zoom: 50%;" /> 

A technique used to remove undesired redundancy from databases (Break one large table into several smaller tables).

**A relation is normalized if all determinants are candidate keys**

==How do we normalise?==

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200854186.png" alt="image-20240420200854186" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200907549.png" alt="image-20240420200907549" style="zoom:67%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200930798.png" alt="image-20240420200930798" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420200940727.png" alt="image-20240420200940727" style="zoom: 60%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201005070.png" alt="image-20240420201005070" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201014112.png" alt="image-20240420201014112" style="zoom: 60%;" />  

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201033164.png" alt="image-20240420201033164" style="zoom:60%;" /> 

Break into two But… How do we connect?

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201103664.png" alt="image-20240420201103664" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201118774.png" alt="image-20240420201118774" style="zoom: 50%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201133252.png" alt="image-20240420201133252" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201218399.png" alt="image-20240420201218399" style="zoom: 60%;" />  

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201315200.png" alt="image-20240420201315200" style="zoom: 50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201327080.png" alt="image-20240420201327080" style="zoom:60%;" />  

 ```
 
 ```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201342977.png" alt="image-20240420201342977" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201357519.png" alt="image-20240420201357519" style="zoom:60%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201419010.png" alt="image-20240420201419010" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201440430.png" alt="image-20240420201440430" style="zoom:60%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201502211.png" alt="image-20240420201502211" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420201511623.png" alt="image-20240420201511623" style="zoom:60%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420202602446.png" alt="image-20240420202602446" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420202611940.png" alt="image-20240420202611940" style="zoom:60%;" />  

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420202733461.png" alt="image-20240420202733461" style="zoom:50%;" /> 

We can name the relations now

- Customer (**CustomerNumber**, CustomerName, CustomerAddress)
- Clerk (**ClerkNumber**, ClerkName)
- Product (**ProductNumber**, ProductDescription)
- Invoice (**InvoiceNumber**, Date, CustomerNumber, ClerkNumber)
- InvoiceLineltem (**InvoiceNumber**, **ProductNumber**, UniPrice, Quantity)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420202940987.png" alt="image-20240420202940987" style="zoom:50%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420203006007.png" alt="image-20240420203006007" style="zoom:50%;" /> 

**1️⃣**A functional dependency concerns values of attributes in a relation

**2️⃣**A set of attributes X determines another set of attributes Y if each value of X is associated with only one value of Y

1. Written X → Y
2. X determines Y (If I know X then I also know Y)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420203555503.png" alt="image-20240420203555503" style="zoom:50%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420204129054.png" alt="image-20240420204129054" style="zoom:50%;" /> 

$A(\underline{X}, \underline{Y}, Z, D)$

**1️⃣**Determinants $(X, Y \rightarrow Z)$

- the attribute(s) on the left hand side of the arrow

**2️⃣**Key and Non-Key attributes

- each attribute is either part of the primary key or it is not

**3️⃣**Partial functional dependency $(Y \rightarrow Z)$

- a functional dependency of one or more non-key attributes upon part (but not all) of the primary key

**4️⃣**Transitive dependency $(Z \rightarrow D)$

- a functional dependency between 2 (or more) non-key attributes

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420204423706.png" alt="image-20240420204423706" style="zoom:50%;" /> 

Functional dependencies can be identified using Armstrong’s Axioms

$A=(X 1, X 2, \ldots, X n)$ and $B=(Y 1, Y \mid 2, \ldots, Y n)$​

1. Reflexivity: $B \subseteq A \Rightarrow A \rightarrow B$
    Example: Student_ID, name $\rightarrow$ name
2. Augmentation: $A \rightarrow B \Rightarrow \mathrm{AC} \rightarrow B C$
    Example: Student_ID -> name => Student_ID, surname ->name, surname
3. Transitivity: $A \rightarrow B$ and $\mathrm{B} \rightarrow C \Rightarrow A \rightarrow C$
    Example: ID -> birthdate and birthdate $->$ age then ID ->age

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420204545756.png" alt="image-20240420204545756" style="zoom:50%;" /> 

**1️⃣**First Normal Form: Keep atomic data/Remove repeating groups

**2️⃣**Second Normal Form: Remove partial dependencies

**3️⃣**Third Normal Form: Remove transitive dependencies

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420205447190.png" alt="image-20240420205447190" style="zoom:50%;" /> 

**1️⃣**Remove Repeating Groups
1. repeating groups of attributes cannot be represented in a flat, two dimensional table

2. removing cells with multiple values (keep atomic data)

**2️⃣**Example: **Order-Item (Order#, Customer#, (Item#, Desc, Qty))**

1. Order-Item (Order#, Customer#, (Item#, Desc, Qty))

2. Break them into two, Use PK/FK to connect
   - Use PK/FK to connect
   - Order (Order#, Customer#)

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420205815886.png" alt="image-20240420205815886" style="zoom:50%;" /> 

**1️⃣**Remove Partial Dependencies

1. a non-key attribute cannot be identified 
2. by part of a composite key

**2️⃣**Example: **Order-Item (Order#, Item#, Desc, Qty)**

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210352304.png" alt="image-20240420210352304" style="zoom:50%;" /> 

Item (Item\#, Desc)
Order-Item (Order\#, Item\#, Qty)

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210006965.png" alt="image-20240420210006965" style="zoom:50%;" /> 

**Order-Item (Order#, Item#, Desc, Qty)**

| Order# | Item# | Desc   | Qty  |
| :----- | :---- | :----- | :--- |
| 27     | 873   | nut    | 2    |
| 28     | 402   | bolt   | 1    |
| 28     | 873   | nut    | 10   |
| 30     | 495   | washer | 50   |

- UPDATE change item desc in many places
- DELETE data for last item lost when last order for that item is deleted
- INSERT cannot add new item until it is ordered

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210119080.png" alt="image-20240420210119080" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210138225.png" alt="image-20240420210138225" style="zoom:50%;" /> 

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210156581.png" alt="image-20240420210156581" style="zoom:50%;" /> 

**1️⃣**Remove Transitive Dependencies 

1. a non-key attribute cannot be identified 
2. by another non-key attribute

**2️⃣**Example: Employee (Emp\#, Ename, Dept\#, Dname)

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420210410088.png" alt="image-20240420210410088" style="zoom: 33%;" /> 

Employee (Emp\#, Ename, Dept\#)
Department( Dept\#, Dname)

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211123277.png" alt="image-20240420211123277" style="zoom:50%;" /> 

Example: Employee (Emp\#, Ename, Dept\#, Dname)

| Emp# | Ename | Dept# | Dname   |
| :--- | :---- | :---- | :------ |
| 10   | Smith | D5    | MIS     |
| 20   | Jones | D7    | Finance |
| 25   | Smith | D7    | Finance |
| 30   | Black | D8    | Sales   |

- UPDATE change dept name in many places
- DELETE data for dept lost when last employee for that dept is deleted
- INSERT cannot add new dept until an employee is allocated to it

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211226955.png" alt="image-20240420211226955" style="zoom:50%;" /> 

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211251622.png" alt="image-20240420211251622" style="zoom:50%;" /> 

```

```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211413376.png" alt="image-20240420211413376" style="zoom:50%;" /> 

**1️⃣**Remove repeating groups

**2️⃣**Remove partial dependencies

**3️⃣**Remove transitive dependencies

```

```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211452648.png" alt="image-20240420211452648" style="zoom:50%;" /> 

**1️⃣**Normalisation:
- Normalised relations contains a minimum amount of redundancy and allow users to insert, modify, and delete rows in tables without errors or inconsistencies (anomalies)

**2️⃣**Denormalization:

1. The pay-off: query speed.
2. The price: extra work on updates to keep redundant data consistent.
3. Denormalization may be used to improve performance of time-critical operations.

 ```
 
 ```


$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240420211554957.png" alt="image-20240420211554957" style="zoom:50%;" /> 

```

```

$$
\textcolor{red}{\Huge{-------}\colorbox{yellow}{分割线}\Huge{-------}}
$$

