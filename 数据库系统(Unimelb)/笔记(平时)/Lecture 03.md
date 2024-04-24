# L03 ER Modelling

# 1. Basic ER modeling concepts

> ## 1.1. Entity and its Attributes
>
> > **1️⃣**Entity
> >
> > 1. Conception: Real-world object distinguishable from other objects,
> > 2. Feature: described by a set of attributes in database
> >
> > **2️⃣**Entity Set
> >
> > 1. Conception: Collection of the same type entity.
> >
> > 2. Feature: Entities in the same set——
> >
> >    - Has same attributes
> >    - Each entity has a key
> >
> > **3️⃣**Example
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319002335462.png" alt="image-20240319002335462" style="zoom:50%;" />
> >
> > 1. Attributes of a Entity: SSN(Key attribute), Name, Age
> > 2. Entity: Employee
> > 3. Entity set: Employee 1, Employee 2,.......Employee n
>
> ## 1.2. Relationship
>
> > **1️⃣**Relationship: 
> >
> > 1. Conception: Association among two or more entities.
> > 2. Feature: Has its own attributes 
> > 3. Example: **Employee 1** ==works in== **Department 1**.
> >
> > **2️⃣**Relationship Set: 
> >
> > 1. Conception: Collection of relationships of the same type
> > 2. Example: **Employees** ==works in== **Departments**.
> >
> > **3️⃣**Example: Relationship Set with the (optional) description attribute "since"
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319012329413.png" alt="image-20240319012329413" style="zoom:50%;" /> 
>
> ## 1.3. Relationship Roles
>
> > **1️⃣**Same entity set can be in different relationship sets
> >
> > E.g. Red arrow, A employee (entity) can work in (relationship 1) or report to (relationship 1) another entity.
> >
> > **2️⃣**Same entity set can have different roles
> >
> > E.g. Blue arrow, in "reports to" relationship, there are two roles for the employee: supervisor (上级) and subordinate(下属). 
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319013123774.png" alt="image-20240319013123774" style="zoom: 50%;" />  

# 2. Constraints

> ## 2.1. Key Constraints: How many objects each side
>
> > **0️⃣**Overview: Circle—Entity Set, Point in Circle—Entity
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319171748574.png" alt="image-20240319171748574" style="zoom:50%;" /> 
> >
> > **1️⃣**Many to many: 
> >
> > 1. E.g. Employee$\xrightarrow{\text{Work in}}$Many departments; Many employees$\xleftarrow{\text{Has}}$​Department
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319182206982.png" alt="image-20240319182206982" style="zoom:50%;" />  
> >
> > 2. Diagram: "Many" represented by a <font color='red'>(red)</font> straight line<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319175958694.png" alt="image-20240319175958694" style="zoom:50%;" /> 
> >
> > **2️⃣**One to many (==Key constrain==): One entity set have a single entity in each relationship.
> >
> > 1. E.g. Manager$\xrightarrow{\text{Manage}}$Many departments; One Manager$\xleftarrow{\text{Be managed by}}$​Department.
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319182141553.png" alt="image-20240319182141553" style="zoom:50%;" />  
> >
> > 2. Diagram: Represented in red arrow.
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319181022213.png" alt="image-20240319181022213" style="zoom:50%;" /> 
>
> ## 2.2. Participation Constraints
>
> > **1️⃣**Total participation: All entities of one set participate in a certain relationship.
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319182837603.png" alt="image-20240319182837603" style="zoom: 67%;" /> 
> >
> > **2️⃣**Partial participation: ==NOT== all entities of one set participate in a certain relationship.
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319183010102.png" alt="image-20240319183010102" style="zoom: 67%;" />  NOT everyone can be manager.
> >
> > **3️⃣**Diagram: Total participation is represented by bold line
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319185302286.png" alt="image-20240319185302286" style="zoom:50%;" /> 
>
> ## 2.3. Weak Entity
>
> > **1️⃣**Weak Entity Core features:
> >
> > 1. Owner entity: Weak entity Cannot exist independently and relies on another entity (==Owner entity==)
> > 2. Weak key: The key that weak entity possess, combined with the key of the owner entity, uniquely identifies the weak entity
> >
> > **2️⃣**Identifying relationship (Weak Relationship) Core features
> >
> > 1. Connect the weak entity and owner entity.
> > 2. All Entities in weak entity must participate in such relationship.
> >
> > **3️⃣**Diagram elements: 
> >
> > 1. Weak Entity: Represented by **BOLD** rectangle
> > 2. Weak Relationship: Represented by **BOLD** diamond
> > 3. Weak Key: Depicted by a dashed underline(虚下划线)
> >
> > **4️⃣**E.g. Insurance policies for university employees and their dependents(家属)
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319192431916.png" alt="image-20240319192431916" style="zoom:50%;" /> 
> >
> > 1. ←, Each dependent only can be affiliated to one Employee. 
> > 2. Bold←, All dependents are affiliated to one Employee
>
> ## 2.4. Ternary (三元) Relationships
>
> > **1️⃣**Conception: Three entity parts, participate in one relationship
> >
> > **2️⃣**Example: A contract that has been established between a department and a supplier for a specified quantity of parts(部件)
> >
> > <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319194238726.png" alt="image-20240319194238726" style="zoom:50%;" /> 
>
> ## 2.5. Special Attributes Type
>
> > **1️⃣**Multi-valued attributes: 
> >
> > 1. Conception: Attributes that have multiple(but not infinite) values of the same type
> >
> > 2. Diagram: Depicted here as two (whether 3/4/5 phone numbers) nested(镶嵌) bubbles
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319195306061.png" alt="image-20240319195306061" style="zoom: 40%;" />  
> >
> > **2️⃣**Composite(复合) attributes:
> >
> > 1. Conception: Have structure inside
> >
> > 2. E.g. 
> >
> >    <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240319200455024.png" alt="image-20240319200455024" style="zoom: 50%;" /> 

# 3. Conceptual Design

> ## 3.1. Design choices
>
> > **1️⃣**Concept be modeled as: Entity vs. Attribute & Entity vs. Relationship
> >
> > **2️⃣**Relationship: Binary vs. Ternary
> >
> > **3️⃣**E.g. "Address" is attribute of employee / entity
> >
> > 1. Be an entity: When address has structured interior like a postcode, we have multiple addresses for one employee
> > 2. Be an attribute:Each employee has a single simple address string
>
> ## 3.2. Notes on the ER design
>
> > **1️⃣**There are often many ways to model a given scenario, with no standard. What we use is Peter Chen's 
> >
> > **2️⃣**Regarded as a high-level description of data to be stored