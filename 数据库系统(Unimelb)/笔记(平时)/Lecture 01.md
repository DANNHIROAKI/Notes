
# L01 Introduction to Databases

# 1. Date :vs: Information

>  | Conception  | Definition                                                   | Examlpe                | Core Feature     |
>  | ----------- | ------------------------------------------------------------ | ---------------------- | ---------------- |
>  | Date        | Facts that are stored and recorded                           | text/images/video..... | Available/Known  |
>  | Information | data presented in context (上下文)<br/>data that has been summerized | #                      | Processed/Useful |
>
>  <img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240312192807808.png" alt="image-20240312192807808" style="zoom:55%;" /> $\xrightarrow{Data\xrightarrow{Process}Information}$​<img src="https://raw.githubusercontent.com/DANNHIROAKI/New-Picture-Bed/main/img/image-20240312193007260.png" alt="image-20240312193007260" style="zoom:37%;" /> 

# 1. Metadata (元数据)

> **1️⃣**Overview
>
> | Conception | Data about data                             |
> | ---------- | ------------------------------------------- |
> | Function   | Describe the property/structure of data     |
> | Feature    | Ensure the consistancy (一致性) and meaning |
>
> **2️⃣**Example:  
>
> 1. Database: Library
>
> 2. Data: Book
>
> 3. Metadata: Labels of each book, describing books title/author/year
>
> | Metadata item |        Metadata content        |
> | :-----------: | :----------------------------: |
> |     Title     | 2 The Dictionary of Lost Words |
> |    Author     |          Pip Williams          |
> |     Year      |              2023              |

# 1. Database

> **1️⃣**Definition: Large integrated and ==structured==(nicely orgnized) collection of data 
>
> **2️⃣**Usage: Store data / Extract useful data

# 1.4. DBMS (Database Management System)

> **1️⃣**Definition: Software that is designed to store/manage/facilitate access to databases.
>
> **2️⃣**Example: My SQL
>
> **3️⃣**Advantages:
>
> 1. Data independence: Separate data & proram
> 2. Minimal data redundancy(冗余): Through normalization of data
> 3. Improve data consistency
> 4. Improve data sharing:  multiple users simply attach on top of DB
> 5. Reduce program maintenance