---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/data-and-delta-lake/"}
---


#### Data Lake 

What is Data Lake ? 

**A Data Lake** is a **centralized storage repository** that holds large volumes of raw data in its native format — structured (CSV, Parquet), semi-structured (JSON, Avro), or unstructured (images, video, logs).

**Analogy:** Think of a **Data Lake** like a real lake. You can dump anything into it: clean water (structured data like CSVs), muddy water (semi-structured data like JSONs), and even old boots and logs (unstructured data like images, PDFs). It's all stored in one place in its natural, raw format.


![Pasted image 20250701222143.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250701222143.png)

Q. Why do we need to store the data in data lake and not in data warehouse ? 

Ans: Because it is expensive , inflexible and does not scale well for all data types and use cases. Generally data lakes are object storage and are usually cheap. Data lakes support delta lakes via open source or EMR.
Examples : AWS S3 ( most widely used) , Azure Data Lake Storage , GCS ( Google Cloud Storage).

Q. What data lake platforms are you familiar with ? 


> “I’ve worked with **Google Cloud Storage (GCS)** as the foundational object storage layer for data lake architectures on GCP. It supports storing massive volumes of raw, semi-structured, or structured data like JSON, CSV, Avro, and Parquet. On top of GCS, I’ve built analytical and ETL pipelines using **Dataproc** (managed Spark/Hadoop) and **Dataflow** (Apache Beam) for both batch and real-time processing.

> For querying and analytics, I’ve used **BigQuery** with **external tables** that read directly from GCS. This allows for cost-efficient exploration of large datasets without first loading them into a warehouse. In addition, we used **Delta Lake on GCS**, deployed via open-source Spark, for use cases that required **ACID guarantees**, **time travel**, and **schema evolution** — such as GDPR-compliant deletes and slowly changing dimensions.

> So in GCP, my data lake stack typically looked like:
> 
> **GCS** for raw/bronze storage,
>     
> **Dataproc/Dataflow** for compute/ETL,
>     
> **BigQuery external tables** or **Delta Lake over GCS** for analytics,
>     
>  With metadata management via **Data Catalog**.”





Why was it created ? 

>To overcome the limitations of traditional Data Warehouses, which were expensive and could only handle clean, structured data. Businesses wanted to store everything, just in case it became useful later for Machine Learning or analytics.


A data lake without proper management becomes a data swamp.

Why does it become a swamp?

- No Reliability: A job writing data might fail halfway through, leaving behind corrupted or partial files. You can't trust the data.
- No Data Quality: You can dump anything in. There are no rules (schemas) to enforce what the data should look like. This is often called "schema-on-read," which can be slow and unpredictable.
- No Consistency: You can't read from a table while someone else is writing to it. The reader might see partial data or fail completely. This makes it impossible to mix analytics (BI) with data engineering (ETL).


Interview Question : "So, if you have a massive folder of a million CSV files on S3, and one of your data-writing jobs fails halfway through, what's the state of your data? How do you fix it?"

Ans : The data is corrupt, and fixing it is a nightmare. You have to manually find and delete the bad files



#### Delta Lake



**Analogy:** **Delta Lake** is like adding a sophisticated water filtration and bottling plant on the shore of your lake. It doesn't change the lake itself (the data is still in S3), but it adds a layer of control, quality, and history. It takes the messy water and makes it clean, reliable, and ready to use.

- It's **NOT** a new storage system. It's an **open-source storage layer** that runs on top of your existing Data Lake (S3, ADLS, etc.).
    
- It brings **Reliability, Quality, and Performance** to your data lake files (which are typically stored in Parquet format).
    
- **The Magic Ingredient:** It does this by creating a **transaction log (_delta_log)** alongside your data files. 

This log is the **single source of truth**. When you query a Delta table, the engine first reads the log to find out which data files are part of the current, valid version of the table.


![Pasted image 20250701231104.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250701231104.png)






How does it help us ? 




![Pasted image 20250701231016.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250701231016.png)


1.  **It brings RELIABILITY with ACID Transactions**

> Problem it solves : Failed jobs corrupting data.
> What is it : ACID (Atomicity, Consistency, Isolation, Durability) is a database guarantee. For Delta Lake, the most important one to know is **Atomicity**.
> 
>  **How it works:** An operation (like writing 100 new files) is a "transaction." This transaction is only complete when a new file is successfully written to the _delta_log_. If the job fails at file 99/100, the log is never updated.
> 
>  **The result:** Operations are **all-or-nothing**. A job either completes 100% or it fails and leaves the table in the exact state it was in before. No more partial or corrupt data.
> 

Q. "Okay, so if that job writing a million files to a **Delta table** on S3 fails halfway through, what's the state of the data?"

Ans : "The table is perfectly fine. It remains in the state it was in before the job started. The failed transaction was never committed to the log, so to any user querying the table, it's as if the failed job never even happened."


2.   **It brings DATA QUALITY with Schema Enforcement & Evolution**

> Problem It solves : Dumping messy inconsistent data into the lake.
> What is it : Data Lake knows your table's scheme (column names and data types) should look like.
> 
>  **Schema Enforcement:** By default, if you try to write data that doesn't match the schema (e.g., putting a name like "Bob" into an age column), Delta Lake will reject the write and throw an error. This protects your table's quality.
>  
>   **Schema Evolution:** Business needs change. If you need to add a new column, you can easily tell Delta Lake to evolve the schema to accept the new data without having to rewrite the entire old table.
>




3.   **It brings CONSISTENCY for BI and AI with Snapshot Isolation**

> - **Problem it solves:** Can't read and write to the same table at the same time.
>   How it works :  When you start a query, it gets a "snapshot" of the table based on the latest version in the transaction log. It will only see that version, completely isolated from any new data being written by another process. This allows you to run a long analytics query on a stable version of the data while a streaming job is simultaneously adding new rows.
> 


4.  **The "Wow" Feature: Time Travel (Data Versioning)**

> What is it : - Since the _delta_log_ contains the entire history of every transaction, you can query your data **as it existed at any point in time**.
>
>**Why it's amazing :** 
>
>- **Debugging:** "A bug was introduced yesterday at 3 PM. Let's query the data from 2:59 PM to see what it looked like before the bug."
>
>- **Auditing:** "I need to generate the Q3 financial report exactly as the data stood on September 30th."
>
>- **Rollbacks:** "We pushed a bad data update! Let's instantly restore the table to the version from an hour ago."




#### Architectures 


Before delving into architectures , we need to understand a sample E2E pipeline flow

Lets assume the business wants to :
	See Dashboards ( Daily sales per region)
	Train ML models (fraud detection)
	Run reports for Executives ( Internal)

But raw data lives in unorganized stores ( assume ) 


**Step by Step flow**

1. Ingestion 

Pull data from :
	MySQl , Oracle , Kafka , Rest API's or Internal Tools

Store this in some object storage : 
	Amazon S3 , Azure Data Lake Storage , Google Cloud Storage 
This is our data lake. 


2. Storage format and Governance

"How do we ensure files do not get corrupted , how to update/ delete or read them with consistency ? "

We will add data lake:
	Delta Lake/Iceberg/ Hudi (ACID transactions)
	Schema Evoltion
	TIme Travel
	Version Control

So, now our data lake S3 has become lakehouse with delta.

3.  Cleaning and Modelling ( ETL and ELT)

"How do we clean and transform the raw data into useful tables"

|Layer|Meaning|Example Data|
|---|---|---|
|**Bronze**|Raw zone|Ingested JSON logs|
|**Silver**|Cleaned zone|Parsed, joined tables|
|**Gold**|Business zone|Ready for reporting (sales by region, churn rate, etc.)|
This is called the **Medallion Architecture** (by Databricks).



4. Logical Modelling 

“How do we organize clean data for business use?”

Now we need to **structure the cleaned data** into something intuitive.

|Model|Style|Example|
|---|---|---|
|**Kimball / Star Schema**|Fact + dimensions|`fact_sales` joined with `dim_product`, `dim_date`|
|**Inmon**|Normalized warehouse|3NF schema across whole company|
|**Data Vault**|Hubs, links, satellites|Good for change tracking, compliance, audit logs|

5. Consumption 

Bi Tools -> Tableau , PowerBI , Looker
Dashboards
Reports
Ad Hoc SQL 

These users query gold layer of the medallion flow

Example : 

                        +-------------------+
     Source Systems     |  Payments DB, API |
                        |  Kafka, Logs      |
                        +---------+---------+
                                  |
                           [1] Ingest
                                  |
                            v
                   +-----------------------+
                   |     Data Lake (S3)    |  ← Raw files (CSV, JSON)
                   | + Delta/Iceberg/Hudi  |  ← ACID, schema, etc.
                   +-----------------------+
                                  |
                         [2] Transform with Spark / DLT
                                  |
                        ┌──────────────────────┐
                        │ Bronze (Raw)         │  ← No modeling
                        │                      │
                        │ Silver (Cleaned)     │  ← Start of Logical Modeling
                        │  • De-duplication    │
                        │  • Joins             │
                        │  • Slowly Changing   │
                        │  • Data Vault Hubs   │
                        │                      │
                        │ Gold (BI-ready)      │  ← Full Logical Modeling
                        │  • Star Schema       │
                        │  • Aggregates        │
                        │  • Time Dimensions   │
                        └──────────────────────┘
                                  |
                           [3] Consumption
                                  |
            +----------------------------------------------+
            | Dashboards (Tableau, Power BI)               |
            | Ad hoc SQL (Databricks SQL, Redshift, etc.)  |
            | ML models (Feature Store, SageMaker, etc.)   |
            +----------------------------------------------+



Slowly changing : refers to **data that changes over time**, but **not rapidly** — like customer addresses, product prices, employee titles, etc.
Data Vault : It is modern data modelling methodology.
	
	 Tracking changes (SCD!)
	 Handling complex source systems
	 Building auditable, historical, agile data warehouses




#### Medallion Architecture 

A logical layering pattern for organizing data pipelines inside a lakehouse.

The goal is to incrementally and progressively improve the quality, structure, and reliability of data as it flows through the system. Popularized by Databricks for use with Delta Lake, its name comes from its three distinct layers, or "medals": **Bronze**, **Silver**, and **Gold**.

Think of it like a refinery for data. You start with raw crude oil (Bronze), refine it into more useful products like gasoline (Silver), and finally create highly specialized, high-value products like jet fuel or chemical feedstocks (Gold).


![Pasted image 20250702054911.png](/img/user/personal%20vault/Attachments/Pasted%20image%2020250702054911.png)


|Layer|Purpose|Role in Modeling|
|---|---|---|
|**Bronze**|Raw data (no transforms)|No modeling|
|**Silver**|Cleaned, joined, deduplicated data|Data Vault / normalized|
|**Gold**|BI-ready, aggregated, metrics|Star Schema / Kimball|

##### 1. Bronze Layer (Raw Data)

- **State of the Data:** Raw, "as-is," often in its original format (e.g., JSON, CSV, raw database logs).
- **Structure:** The schema of the data here typically mirrors the source system's schema. It’s an append-only, immutable historical archive of all incoming data.
- **Key Operations:**

    - **Ingestion:** Batch or streaming data lands here.
    - **Archiving:** Serves as a long-term, low-cost archive. Because it’s the raw source, you can always rebuild your downstream tables from this layer if there's a mistake.
    - **Basic Metadata:** Captures source metadata like ingestion timestamp.
        
- **Primary Users:** Data Engineers.
- **Analogy:** This is the unfiltered water from the river. You wouldn't drink it, but it's the source of everything.


##### 2. Silver Layer (Cleansed & Conformed Data)

The data from the Bronze layer is cleaned, validated, and enriched to create the Silver layer. This layer provides a more reliable, queryable "single source of truth" for a wide range of analytics.

- **State of the Data:** Cleansed, validated, filtered, enriched, and conformed.
- **Structure:** Data is typically organized into tables with well-defined schemas. Bad records are handled (either filtered out or quarantined), and data types are corrected (e.g., strings converted to timestamps).
- **Key Operations:**
    
    - **Cleaning:** Handling nulls, removing duplicates, correcting data entry errors.
    - **Enrichment:** Joining different Bronze tables to create a more complete view (e.g., joining a customers table with an orders table).
    - **Conforming:** Standardizing fields (e.g., ensuring all country codes follow the ISO standard).
        
- **Primary Users:** Data Scientists, Machine Learning Engineers, and Data Analysts.
- **Analogy:** This is the drinkable tap water. It has been filtered, treated, and is safe and reliable for general consumption.


##### 3. Gold Layer (Curated & Business-Level Data)

- **State of the Data:** Aggregated, business-focused, and highly optimized for performance.
- **Structure:** Often organized into "data marts" or aggregated views tailored for specific business functions like sales, marketing, or finance. The structure is often a star schema or other dimensional model.
- **Key Operations:**
    
    - **Aggregation:** Calculating key business metrics like monthly recurring revenue (MRR), daily active users (DAU), or customer lifetime value (CLV).
    - **Final Transformations:** Applying business-specific logic and calculations.
    - **Performance Optimization:** Data is structured to provide fast query results for dashboards and reports.
        
- **Primary Users:** Business Analysts, Data Analysts, and Executives (via BI dashboards).
    
- **Analogy:** This is the premium bottled mineral water. It's not just for drinking; it's designed for a specific, high-value purpose and is perfectly presented.