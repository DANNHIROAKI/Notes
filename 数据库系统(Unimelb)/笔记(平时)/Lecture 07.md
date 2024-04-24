# Lectre7: Relational Algebra

**5 Basic Operations**

# 0. Overview

> ## 0.1. Operations
>
> >  |   Operations   | Notation | Explanation                                             |
> >  | :------------: | :------: | ------------------------------------------------------- |
> >  |   Selection    | $\sigma$ | Horizontal filtering, selects a subset of *rows*        |
> >  |   Projection   |  $\pi$   | Vertical filtering, retain (保留) only wanted *columns* |
> >  | Cross-product  |   $×$    | Combine two relations                                   |
> >  | Set-difference |   $-$    | Tuples in one relation, but not in the other            |
> >  |     Union      |  $\cup$  | Tuples in one relation and/or in the other              |
> >
> >  Relations$\xrightarrow{\text{Operations}}$Other Relations
>
> ## 0.2. Example Instances
>
> > **1️⃣**Boat Table​
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/6cc9db1d8c85bdb05d1e6b3153e2b556.png" alt="6cc9db1d8c85bdb05d1e6b3153e2b556" style="zoom: 67%;" />  
> >
> > **2️⃣**Sailor Table: Separated into 2 groups, S1 (left) & S2 (right)
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/3761f40d45ef46d95bbb2059d3a72b48.png" alt="3761f40d45ef46d95bbb2059d3a72b48" style="zoom: 67%;" />  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711891750690.jpg" alt="1711891750690" style="zoom:67%;" />  
> >
> > **3️⃣**Reserves(预定) Table: 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711891903359.jpg" alt="1711891903359" style="zoom:67%;" /> 

# 1. Selection & Projection: Within one Table

> ## 1.1. Projection ($\pi$)
>
> > ### 1.1.1. Abstract
> >
> > > **1️⃣**Goal:  Filters out and retains only the attributes listed in the projection list
> > >
> > > **2️⃣**Other Function: Eliminating duplicates, example below
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/6cc9db1d8c85bdb05d1e6b3153e2b556.png" alt="6cc9db1d8c85bdb05d1e6b3153e2b556" style="zoom: 67%;" /> 
> > >
> > > 1. Projecting only `bname` attribute
> > > 2. Two items will be duplicate$\to{}$combine them to one
> > >
> > > **3️⃣**Feature: Projection is a costly operation, DB won't operate it by default.
> >
> > ### 1.1.2. Example for S2 ($\sigma$)
> >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711891750690.jpg" alt="1711891750690" style="zoom:67%;" /> $\large\large \xrightarrow{\pi_{\text{age}}(S_2)}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401023305604.png" alt="image-20240401023305604" style="zoom:60%;" /> $\large\large \xrightarrow{\text{Removed duplicates}}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401023545442.png" alt="image-20240401023545442" style="zoom: 67%;" />
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711891750690.jpg" alt="1711891750690" style="zoom:67%;" /> $\large\large \xrightarrow{\pi_{\text{sname, rating}}(S_2)}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401023821688.png" alt="image-20240401023821688" style="zoom: 67%;" />
>
> ## 1.2. Selection
>
> > ### 1.2.1. Abstract
> >
> > > **1️⃣**Goal:  Retain records that fulfill a specific condition
> > >
> > > **2️⃣**Duplicates: MUST NO duplication, because relation itself is a set of unique records
> >
> > ### 1.2.2. Example
> >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401025827011.png" alt="image-20240401025827011" style="zoom:50%;" /> $\large\large \xrightarrow{\sigma_{rating\geq{}9}(S_2)}$​​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/39f6ffcb25a774de649e5fdbcbbf97c6.png" alt="39f6ffcb25a774de649e5fdbcbbf97c6" style="zoom:50%;" />   
> >
> > ### 1.2.3. Condition expressions in SQL
> >
> > > **1️⃣**Arithmetic expressions: `>, <, >=, <=, =, !=`
> > >
> > > **2️⃣**AND/OR clauses: `AND = Λ` , `OR = V`
> > >
> > > E.g. $\sigma_{rating\geq{}9\land{}age<50}(S_2)$​
>
> ## 1.3. Projection & Selection: Select Specific Subset
>
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401031241564.png" alt="image-20240401031241564" style="zoom:50%;" /> $\large\large \xrightarrow{\pi_{\text{sname, rating}}(\sigma_{rating\geq{}9}(S_2))}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401031348650.png" alt="image-20240401031348650" style="zoom:50%;" />

# 2. Union, Set Difference & Intersection

> ## 2.1. Union, Set Difference(差集)
>
> > ### 2.1.1. Abstract
> >
> > > **1️⃣**Operation:
> > >
> > > 1. Union: Combines both relations together
> > > 2. Set-Difference: Retains rows of one relation that do not  appear in the other relation
> > >
> > > **2️⃣**Requirement: Union-Compatible (并集兼容)
> > >
> > > 1. Same number of fields (column, attributes)
> > > 2. Corresponding fields have the same type
> >
> > ### 2.1.2. Example
> >
> > > **1️⃣**Union: Combine and remove the duplicates items, order-independent
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902909279.jpg" alt="1711902909279" style="zoom: 40%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902927676.jpg" alt="1711902927676" style="zoom: 40%;" /> $\xrightarrow[or\\ S_2\cup{S_1}]{S_1\cup{S_2}}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711903012794.jpg" alt="1711903012794" style="zoom: 40%;" /> 
> > >
> > > **2️⃣**Set Difference: Order-dependent, not symmetrical (对称)
> > >
> > > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902909279.jpg" alt="1711902909279" style="zoom: 40%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902927676.jpg" alt="1711902927676" style="zoom: 40%;" /> $\xrightarrow{S_1-{S_2}}$​ <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711903146343.jpg" style="zoom:50%;" /> 
>
> ## 2.2. Compound Operator (复合运算): Intersection
>
> > **1️⃣**Operation: 
> >
> > 1. Retains rows that appear in *both* relations
> > 2. In basic expression, is $R\cap{}S=R-(R-S)$
> >
> > **2️⃣**Input: two relations meets the union-compatible
> >
> > **3️⃣**E.g. order-independent
> >
> >  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902909279.jpg" alt="1711902909279" style="zoom: 40%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902927676.jpg" alt="1711902927676" style="zoom: 40%;" /> $\xrightarrow[or\\ S_2\cap{S_1}]{S_1\cap{S_2}}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711908164999.jpg" alt="1711908164999" style="zoom:50%;" /> 

# 3. Cross product & Joins

> ## 3.1. Cross Product (×)
>
> > **1️⃣**Operation
> >
> > 1. Each row of one input is merged with each row from another input. 
> > 2. Output is a new relation with all attributes of *both* inputs
> > 3. Rows in the result: Card (Input 1) * Card (Input 2) 
> >
> > **2️⃣**E.g. two SIDs ?
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711902909279.jpg" alt="1711902909279" style="zoom: 40%;" /> <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711908857220.jpg" alt="1711908857220" style="zoom:50%;" /> $\xrightarrow{S_1×{R_1}}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711908813683.jpg" alt="1711908813683" style="zoom:50%;" /> 
> >
> > **3️⃣**Rename Operator $\rho$: Maintain Attributes Uniqueness
> >
> > 1. Parameters: Attributes' new name, Input relationship
> >
> > 2. E.g. $\rho{(C(1\to{sid1},5\to{sid2}),S_1×R_1)}$
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711909347952.jpg" alt="1711909347952" style="zoom:50%;" /> 
>
> ## 3.2. Compound Operator: Natural Join (aka Join)
>
> > **1️⃣**Operations of $R\bowtie{}S$​
> >
> > 1. Compute $R×S$, and find the attributes appear in both $R$ and $S$
> > 2. Find rows that those attributes have the same value
> > 3. Project to combine the same attributes
> >
> > **2️⃣**E.g. 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240401054601363.png" alt="image-20240401054601363" style="zoom:50%;" /> $\xrightarrow{\text{Step 2,3 of }R\bowtie{}S}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711909823605.jpg" alt="1711909823605" style="zoom: 40%;" />
> >
> > 1. Attributes `sid` appear in both relations
> > 2. Choose rows that with the same `sid`s value
> > 3. Combine 2 `sid`s to one `sid`
>
> ## 3.3. Other type of Join
>
> > **1️⃣**$\Theta$ Join (Condition Join): Cross-Product with Condition.
> >
> > 1. $R\bowtie{_{Condition}}S = \sigma_{Condition}(R×S)$​
> > 2. E.g. 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711908813683.jpg" alt="1711908813683" style="zoom:45%;" /> $\large\large\xrightarrow{R\bowtie{_{(S_1.sid<R_1.sid)}}S}$<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/1711911148407.jpg" alt="1711911148407" style="zoom:38%;" />
> >
> > **2️⃣**Equi-Join: 
> >
> > 1. Special case of condition join,  condition contains only *equalities* (等式)
> > 2. E.g. $R\bowtie{_{(S_1.sid=R_1.sid)}}S$