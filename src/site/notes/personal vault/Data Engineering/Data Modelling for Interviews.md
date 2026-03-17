---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/data-modelling-for-interviews/"}
---





###    Index

#### **Part 1: Relational Modeling (For All Applications & Databases)**

- **Chapter 1: The Core Idea**
    
    - 1.1. **The 3 Models:** Conceptual (Business Ideas), Logical (The Rules), Physical (The Code).
        
    - 1.2. **Entities:** The main "nouns" of your system (e.g., User, Product).
        
    - 1.3. **Attributes:** The properties of an entity (e.g., User has email, name).
        
- **Chapter 2: Building Relationships**
    
    - 2.1. **Keys are Everything:**
        
        - **Primary Key (PK):** The unique ID for each record. (Natural vs. Surrogate).
            
        - **Foreign Key (FK):** The PK of another table, used to create a link.
            
    - 2.2. **Relationship Rules (Cardinality):**
        
        - One-to-One (1:1)
            
        - One-to-Many (1:N)
            
        - Many-to-Many (M:N) -> **Must be solved with a "Junction Table"**.
            
    - 2.3. **Crow's Foot Notation:** The visual language for drawing relationships.
        
- **Chapter 3: Normalization (The "Don't Repeat Yourself" Principle)**
    
    - 3.1. **Why Normalize?** To prevent data errors (anomalies) and reduce redundancy.
        
    - 3.2. **The 3 Essential Rules:**
        
        - **1st Normal Form (1NF):** Make all values atomic (one value per cell). No lists in a column.
            
        - **2nd Normal Form (2NF):** All columns must depend on the whole primary key (only matters for composite keys).
            
        - **3rd Normal Form (3NF):** No non-key attribute can depend on another non-key attribute. **This is the standard goal.**
            
    - 3.3. **Denormalization:** Intentionally breaking the rules (usually 3NF) to improve read speed. It's a trade-off.
        
- **Chapter 4: The Physical Blueprint**
    
    - 4.1. **Choosing Data Types:** INT, VARCHAR, TEXT, BOOLEAN, TIMESTAMP, DECIMAL.
        
    - 4.2. **Indexes:** How to speed up SELECT queries (at the cost of slower writes). Always index Foreign Keys.
        
    - 4.3. **DDL (Data Definition Language):** The CREATE TABLE statements that bring your model to life.
        

---

#### **Part 2: Dimensional Modeling (For Analytics & BI)**

- **Chapter 5: Modeling for Reports (OLAP)**
    
    - 5.1. **Facts vs. Dimensions:**
        
        - **Facts:** The numbers you want to analyze (SalesAmount, Quantity).
            
        - **Dimensions:** The context you use to filter and group (Customer, Product, Date).
            
    - 5.2. **The Star Schema:** The #1 pattern. A central Fact Table surrounded by Dimension Tables. It's simple, fast, and denormalized.
        
    - 5.3. **Slowly Changing Dimensions (SCD):** How to handle changes in descriptive data over time.
        
        - **Type 1: Overwrite.** (Loses history).
            
        - **Type 2: Add a new row.** (Preserves history, most common).
            

---

#### **Part 3: Modern Modeling (NoSQL & Trade-offs)**

- **Chapter 6: Relational vs. NoSQL**
    
    - 6.1. **When to use NoSQL?** For massive scale, flexible/unstructured data, or specific query patterns.
        
    - 6.2. **Document Modeling (e.g., MongoDB):**
        
        - The key decision: **Embed vs. Reference**.
            
        - Embed related data for fast reads. Reference it for data consistency and to avoid huge documents.
            
    - 6.3. **Graph Modeling (e.g., Neo4j):**
        
        - Use for data that is all about connections (social networks, fraud rings, recommendations).
            
        - Model with **Nodes** and **Relationships**.
            

---



Theory 


___


## Relational Modelling


### Chapter 1 : The core Idea


The **core idea** is that data modeling happens in **three progressive layers**, each with a different audience and purpose:


**1.1 The Three Models** 

	a. Conceptual Model
	b. Logical Model
	c. Physical Model

**The Analogy:** Building a custom house.

1. **Conceptual:** You and the architect sit down for coffee. You say, "I want a 3-bedroom house with an open kitchen and lots of natural light." You both draw a rough bubble diagram on a napkin. **No details, just the big ideas.**

Think of this as the **whiteboard sketch** — you're not thinking about columns or keys yet, just the concepts.

    
2. **Logical:** The architect goes back and creates the formal blueprints. Every room is defined, walls have dimensions, doors and windows are placed. The plan shows how a person moves from the kitchen to the living room. It's a complete, detailed plan, but you haven't picked out the brand of faucet or the color of the paint yet. **All the rules, none of the specific materials.**

This model is **platform-independent** but includes all the logic required to enforce data integrity.

    
3. **Physical:** You give the blueprints to the construction company. They create a plan for your specific plot of land, specifying "We will use 2x4 pine studs, Brand X windows, and a concrete foundation of Y thickness." **All the specific, technical implementation details.**

This is where **performance, cost, and tooling** (e.g., Databricks, Snowflake, Postgres) come into play.



#### A) The Conceptual Model: The "Coffee Chat"

- **What it is:** A high-level view that captures the core business concepts and rules in plain language.
    
- **Purpose:** To make sure everyone (especially non-technical stakeholders) agrees on what we are building.
    
- **Example (Music Service):**
    
    - An **Artist** releases an **Album**.
        
    - An **Album** contains multiple **Songs**.
        
    - A **User** can create a **Playlist**.
        
    - A **Playlist** is a collection of **Songs**.
        
    
    Visually, it's just this:
    

#### **B) The Logical Model: The "Blueprint"**

- **What it is:** A detailed, technology-independent map of your data. This is where the real "modeling" work happens.
    
- **Purpose:** To define the precise structure, relationships, and rules that govern your data to ensure its integrity.
    
- **Example (Music Service):**
    
    - We introduce the rules. An Album must be linked to an Artist. How? By putting the Artist's unique ID (ArtistID) inside the Album entity. This link is called a **Foreign Key**.
        
    - We list out all the pieces of information (**Attributes**) we need for each entity.
        
    - This is the stage where we ensure the model is **Normalized** (which we'll master in Chapter 3).
        
    
    (Note : keys are identified, and all attributes are listed).
    

#### **C) The Physical Model: The "Construction Plan"**

- **What it is:** The concrete implementation of the logical model for a specific database technology (e.g., PostgreSQL, MySQL, Oracle).
    
- **Purpose:** To generate the final code (CREATE TABLE statements) to build the database.
    
- **Example (Music Service, using PostgreSQL):**
    
    - The Album's AlbumID will be a SERIAL PRIMARY KEY.
        
    - The Title will be a VARCHAR(200).
        
    - The ReleaseDate will be a DATE type.
        
    - We decide to add an **index** on ReleaseDate to make searching for albums by year faster.




**1.2 Entities**

An entity is a class of person, place, object, event, or concept about which you need to store data. Think of it as the template for a future database table.

- **Tip:** A common point of confusion is **Entity vs. Instance**.
    
    - **Entity (The Template):** Artist
        
    - **Instance (The Data):** 'Daft Punk', 'Taylor Swift', 'Kendrick Lamar' are all instances of the Artist entity.

##### **Exercise 1: Identifying Entities**

**Scenario:** You're building a system for a University.

> "The university has many **departments**, like Computer Science and History. Each department offers several **courses**. **Professors** are hired to teach these courses. **Students** can enroll in many courses each semester, and for each enrollment, they receive a final grade."

**Your Task:** List the 4 main entities from the scenario above.

Ans : departments , courses , professors ,students


**1.3 Attributes** 

#### **Types of Attributes ( Advanced Examples):**

1. **Simple / Atomic Attribute:** The most common type. It holds a single, indivisible piece of information.
    
    - **Example:** For our Song entity, the Title is a simple attribute. The DurationInSeconds is another.
        
2. **Composite Attribute:** An attribute that can be broken down into smaller, logical parts.
    
    - **Example:** For our User entity, we might have an Address attribute. But Address is really composed of Street, City, State, and ZipCode.
        
    - **Why it matters:** In a logical model, you almost always break composite attributes down into their simple components. This makes searching and filtering much more powerful (e.g., "Find all users in California").
        
3. **Multi-valued Attribute:** An attribute that can hold multiple values for a single entity instance. **Might be a  beginner's trap!**
    
    - **Example:** Imagine we want to add Genre to our Song entity. A song like "Bohemian Rhapsody" could be 'Rock', 'Opera', and 'Pop'. How do you store that?
        
    - **Wrong Way:** A Genre column with the text "Rock, Opera, Pop". This is a nightmare to search and analyze. This breaks the rules of normalization.
        
    - **Right Way ( Chapter 2):** You realize that Genre cannot be a simple attribute of Song. This signals that you will need a separate Genre entity and a linking table to connect Songs to Genres. **If you spot a multi-valued attribute, it's a huge clue that your model needs another table.**
        
4. **Derived Attribute:** An attribute whose value can be calculated or derived from another attribute.
    
    - **Example:** For our User entity, we might store DateOfBirth. Do we also need to store Age?
        
    - **Answer:** Usually no. Age can be calculated from DateOfBirth and the current date. Storing it creates redundant data that can become outdated. You would only store it if it's computationally very expensive to calculate and needed frequently (a performance trade-off called Denormalization, which we'll cover later).



Exercise : 

**Scenario:**

> "We need to build a system for a local library. The library has many **books**. Each book has an ISBN (a unique code), a title, and a publication year. We also need to know the book's authors. An author has a full name and a date of birth. The library also has **members**. We need to track each member's full name, their address, and their email address. When a member borrows a book, we need to create a **loan** record, which notes the date it was checked out and the date it is due back."

**Your Task:**

1. **Identify Entities:** List the main entities you see.
    
2. **List Attributes for Each Entity:** For each entity, list its attributes.
    
3. **Identify Different Attribute Types:** Look at your attribute lists.
    
    - Do you see any **composite** attributes that should be broken down?
        
    - Do you see any **multi-valued** attributes? (This is the tricky one!) What does this tell you about your initial entity list?
        
    - Do you see any potential **derived** attributes that we should probably not store?




*Answer :* 

**Entities:**

- Author
    
- Book
    
- Member
    
- Loan
    

**Attributes for each Entity:**

- **Author:** FullName, DateOfBirth
    
- **Member:** FullName, EmailAddress, Street, City, State, PostalCode
    
- **Book:** ISBN, Title, PublicationYear
    
- **Loan:** CheckoutDate, DueDate
    

**Relationships :**

- An Author can write many Books.
    
- A Book can be written by many Authors.
    
- A Member can have many Loans.
    
- A Book can be on many Loans (over time).
    
- A Loan links one Member to one Book at a specific point in time.


**Type of Attributes :**
- **Simple/Atomic Attributes:**
    
    - DateOfBirth: The author's date of birth. It's a single, indivisible value.
        
- **Composite Attributes:**
    
    - FullName: This is composed of smaller parts (FirstName, LastName) and should be broken down in the logical model for better searching and sorting.
        

- **Simple/Atomic Attributes:**
    
    - EmailAddress: The unique email for the member.
        
- **Composite Attributes:**
    
    - FullName: Same as for Author, this should be broken down into FirstName and LastName.
        
    - Address: This is a classic composite attribute that must be broken down into its constituent parts for any useful filtering (e.g., sending mailers to a specific city). The breakdown is:
        
        - Street
            
        - City
            
        - State
            
        - PostalCode
            

- **Simple/Atomic Attributes:**
    
    - ISBN: The International Standard Book Number. Although it's a number, it's used as a unique identifier, not for calculations. It's a single value.
        
    - Title: The title of the book.
        
    - PublicationYear: The year the book was published.
        
- **Multi-valued Attributes (The Critical Insight!):**
    
    - Author(s): The initial analysis revealed that a book can have one or more authors. **This cannot be an attribute of the Book entity.** This discovery tells us that a simple link from Book to Author won't work. We will need a more complex relationship, which is a key problem to solve in Chapter 2.
        

- **Simple/Atomic Attributes:**
    
    - CheckoutDate: The date the book was borrowed.
        
- **Derived Attributes (Potentially):**
    
    - DueDate: The date the book is due back.
        
        - **Reasoning:** This is likely calculated based on a business rule (e.g., CheckoutDate + 14 days).
            
        - **Decision:** For practicality and query performance, we will treat it as a simple attribute and store the calculated value at the time of the loan. This is a conscious denormalization decision.


### Chapter 2: Building Relationships

**Goal:** To learn the fundamental techniques for linking entities together using keys and to visually represent the rules of these relationships using a standard notation.

---

### **2.1. Keys are Everything: The Foundation of Relationships**

Keys are special attributes that give our tables structure and power.

#### **A) Primary Key (PK)**

**Theory:**  
A Primary Key is an attribute (or a set of attributes) that uniquely identifies every single row in a table. Think of it as the "Social Security Number" or "Student ID" for a record.

- **Rule #1:** A PK's value **must be unique** for each row. You can't have two authors with the same AuthorID.
    
- **Rule #2:** A PK **cannot be empty or NULL**. Every record must have one.
    

**Natural vs. Surrogate Keys (A Critical Choice):**

- **Natural Key:** A key made from an attribute that already exists in the real world.
    
    - Example: Using a book's ISBN as its Primary Key.
        
    - Problem: What if the ISBN is entered incorrectly and needs to be changed? Changing a PK is very difficult and risky, as all other tables that link to it would need to be updated.
        
- **Surrogate Key:** A key that has no business meaning. It's just an artificial, unique number (usually an auto-incrementing integer like 1, 2, 3...) that the database generates.
    
    - Example: Creating a new attribute called BookID.
        
    - **Best Practice:** **For 99% of cases, always use a Surrogate Key.** They are stable, simple, efficient, and have no real-world baggage. We will use the naming convention EntityNameID (e.g., AuthorID, MemberID).
        

#### **B) Foreign Key (FK)**

**Theory:**  
A Foreign Key is the "magic" that creates a relationship. It is a column in one table that holds the value of the Primary Key from another table. It's a pointer or a link.

**Simple Example:**  
Let's model Cuisine and Recipe from our earlier exercise.

- The Cuisine table has its own Primary Key: CuisineID.
    
    - Cuisine (CuisineID PK, CuisineName)
        
    - Instances: (1, 'Italian'), (2, 'Mexican')
        
- The Recipe table has its own Primary Key: RecipeID.
    
- To link a recipe to its cuisine, we add a **Foreign Key** column, CuisineID, to the Recipe table.
    
- **Recipe** (RecipeID PK, Title, Instructions, CuisineID FK)
    
- An instance of a recipe for "Spaghetti Carbonara" would look like: (101, 'Spaghetti Carbonara', '...', **1**)
    
    - That **1** in the CuisineID column is the Foreign Key, pointing directly to the 'Italian' record in the Cuisine table.
        

---

### **2.2. Relationship Rules (Cardinality)**

**Theory:**  
Cardinality defines the numerical rules of a relationship between two entities, A and B. It answers the question, "For one instance of A, how many instances of B can there be?"

1. **One-to-One (1:1):**
    
    - Rule: One instance of A can be linked to at most one instance of B, and vice-versa.
        
    - Example: User and UserProfile. Each user has exactly one profile.
        
    - Implementation: Usually, these are just combined into a single table. They are only separated if one part is optional or for security/organization.
        
2. **One-to-Many (1:N):**
    
    - Rule: One instance of A can be linked to many instances of B, but one instance of B can only be linked to one instance of A.
        
    - Example: One Cuisine can have many Recipes, but a Recipe belongs to only one Cuisine. This is the most common relationship type.
        
    - Implementation: You place the Foreign Key on the "many" side of the relationship. (CuisineID is placed in the Recipe table).
        
3. **Many-to-Many (M:N):**
    
    - Rule: One instance of A can be linked to many instances of B, AND one instance of B can be linked to many instances of A.
        
    - Example (from our Library): One Book can have many Authors, and one Author can have many Books.
        
    - **THE GOLDEN RULE:** You **cannot** implement an M:N relationship directly.
        
    - **The Solution (Junction Table):** You MUST create a third table, called a **Junction Table** (or Linking Table), that sits in the middle. This table's primary purpose is to hold the Foreign Keys from the two tables it connects.
        
        - To solve the Book-Author problem, we create a new table called BookAuthor.
            
        - Each row in BookAuthor will link one BookID to one AuthorID.
            
        - This breaks the single M:N relationship into two 1:N relationships:
            
            - Book has a 1:N relationship with BookAuthor.
                
            - Author has a 1:N relationship with BookAuthor.
                

---

### **2.3. Crow's Foot Notation: The Visual Language**

**Theory:**  
This is the standard way to draw relationships on an Entity-Relationship Diagram (ERD).

- **The "Many" side:** A three-pronged fork, the "crow's foot."
    
- **The "One" side:** A single perpendicular line.
    
- **Optionality:**
    
    - A **circle (O)** means the relationship is optional (zero or...).
        
    - A **line (|)** means the relationship is mandatory (one or...).
        

**Examples:**



![[Pasted image 20250712010221.png\|Pasted image 20250712010221.png]]

Example : 

![[Pasted image 20250712093226.png\|Pasted image 20250712093226.png]]



---

### **Chapter 2 Exercise: Applying the Rules to the Library System**

Now, let's turn our Chapter 1 conceptual analysis into a formal **Logical Model**.

**Your Task:**  
Using our refined entities (Author, Book, Member, Loan) and their attributes:

1. **Assign Keys:**
    
    - For each entity, define a **Surrogate Primary Key** (e.g., AuthorID).
        
    - For each relationship we identified (e.g., a Loan connects a Member and a Book), determine where the **Foreign Keys** should go.
        
2. **Solve the Many-to-Many Problem:**
    
    - We know Book and Author have a M:N relationship. Define the new **Junction Table** you will need to create. What attributes will this table have?
        
3. **List the Final Tables and Attributes:**
    
    - Write out the final list of all your tables (including the new junction table).
        
    - For each table, list all of its attributes, clearly marking which ones are **PK** (Primary Key) and which are **FK** (Foreign Key).
        
    - (Bonus) Try to draw the relationships between the tables using simple text-based Crow's Foot Notation like --|--<.


### Chapter 3 


**3.1** **Why Normalize the Data ?** 
**Theory:**  
An "un-normalized" or "badly" designed table can cause serious problems called **data anomalies**. Imagine if we had a single, messy table like this:

|   |   |   |   |   |
|---|---|---|---|---|
|LoanID|MemberName|MemberEmail|BookTitle|CheckoutDate|
|101|John Smith|[john@email.com](https://www.google.com/url?sa=E&q=mailto%3Ajohn%40email.com)|The Lord of the Rings|2023-10-01|
|102|Jane Doe|[jane@email.com](https://www.google.com/url?sa=E&q=mailto%3Ajane%40email.com)|Dune|2023-10-02|
|103|John Smith|[john@email.com](https://www.google.com/url?sa=E&q=mailto%3Ajohn%40email.com)|The Hobbit|2023-10-03|

This design suffers from major anomalies:

1. **Update Anomaly:** If John Smith changes his email, you have to find and update **every single row** where he appears. If you miss one, the data becomes inconsistent. **Problem:** Data redundancy leads to update nightmares.
    
2. **Insertion Anomaly:** How can you add a new member, "Peter Pan," who hasn't borrowed a book yet? You can't, because you need a BookTitle to create a row. **Problem:** You can't store information about one entity without information about another.
    
3. **Deletion Anomaly:** If Jane Doe returns her only book, "Dune," and you delete row 102, you don't just lose the loan record—you lose all information that Jane Doe even exists! **Problem:** Deleting one piece of information unintentionally deletes another.
    

**The Solution:** Normalization fixes this by separating data into distinct tables for each entity (Member, Book, Loan), just like we did in Chapter 2. Normalization is the formal theory that justifies why our intuitive approach was correct.

---


**3.2 The Normal Forms**

###### **A) First Normal Form (1NF): Make all values atomic.**

- **Rule:** A table is in 1NF if every cell contains a single, indivisible (atomic) value. There can be no repeating groups or lists in a column.
    
- **Simple Example (Violation):**
    
    |   |   |   |
    |---|---|---|
    |RecipeID|Title|Ingredients|
    |101|Carbonara|'Spaghetti, Eggs, Bacon'|
    
- **How to Fix:** Create a separate table for the repeating data (Ingredient) and link it back.
    
    - Recipe Table: (RecipeID, Title)
        
    - RecipeIngredient Table: (RecipeID, IngredientName)


#### **Second Normal Form (2NF): All columns must depend on the whole primary key.**

- **Rule:** A table must be in 1NF, and all of its non-key attributes must be fully functionally dependent on the entire primary key.
    
- **When it Matters:** This rule is only relevant for tables with a **composite primary key** (a PK made of two or more columns).
    
You’re in a classroom, and you’re listing **which student is in which club**.  
If you also list the **student's birthday**, that's not about the club. It's just about the student.

### Bad Example:

|BookID|AuthorID|AuthorDOB|
|---|---|---|
|55|10|1952-03-11|

- Primary Key = (`BookID`, `AuthorID`)
    
- But `AuthorDOB` only depends on `AuthorID` → it doesn’t care about `BookID`.


###  Fix:

Move the birthday to the **Author** table:

- **BookAuthor Table**: `BookID | AuthorID`
    
- **Author Table**: `AuthorID | AuthorName | DOB`
        
- **How to Fix:** Move the partially dependent attribute to the table where it belongs. AuthorDateOfBirth belongs in the Author table.


#### **C) Third Normal Form (3NF): No non-key attribute can depend on another non-key attribute.**

- **Rule:** A table must be in 2NF, and all its attributes must be dependent only on the primary key, not on any other non-key attribute. This is called avoiding **transitive dependency**.
    
- **The Slogan:** "Every non-key attribute must provide a fact about the key, the whole key, and nothing but the key."

### Analogy:

You know the **student's name** and their **school counselor’s name**. But now you also include the **counselor’s phone number** — that’s not about the student, it’s about the counselor. It's **one step removed**


| AuthorID | AuthorName | AgentName  | AgentPhone |
| -------- | ---------- | ---------- | ---------- |
| 10       | S. King    | C. Verrill | 555-1234   |

- `AgentPhone` doesn’t depend directly on `AuthorID`
    
- It depends on `AgentName` → That’s a **transitive dependency**
    

###  Fix:

Break it up!

- **Author Table**: `AuthorID | AuthorName | AgentID`
    
- **Agent Table**: `AgentID | AgentName | AgentPhone`
    

Now everything is stored **where it truly belongs**.


**3.3. Denormalization: Intentionally Breaking the Rules**

**Theory:**  
Denormalization is the process of intentionally violating normalization rules (usually 3NF) to improve performance. It's a trade-off: you sacrifice some data integrity and "purity" to make your database faster for reading data.

- **When to use it:** In reporting databases (Data Warehouses) or in high-traffic applications where you need to avoid complex joins to get commonly requested data.
    
- **Simple Example:**  
    Imagine an e-commerce site's Order page. It needs to show the ProductName for each item in the order.
    
    - **Normalized Way (3NF):** You would join Order -> OrderItem -> Product tables just to get the ProductName.
        
    - **Denormalized Way:** You could add a ProductName column directly to the OrderItem table. This duplicates data (the product name now exists in two places), but it means you can render the order page without an extra join, making it much faster.