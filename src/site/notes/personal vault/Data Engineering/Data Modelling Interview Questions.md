---
{"dg-publish":true,"permalink":"/personal-vault/data-engineering/data-modelling-interview-questions/"}
---






**data modeling questions asked by Meta in data engineer interviews**

- Present a design of a gaming company database.
- Design a database for an app.
- Design a relational database for a ride-sharing app.
- Given data, design a table schema for this data to be used by a data scientist to query metrics such as process with max average elapsed time, and so they can plot each process.
- Create DDL (table and foreign keys) for several tables in a provided ERD. The ERD contains at least one many-to-many relationship.
- Design a database schema for an app which provides a ride-sharing service, explain which tables, field types, and keys will need to be saved.

*SQL questions*

- How do you join two tables with all the information on the left one unchanged?
- Does database view occupy the disk space?
- Given full authority to "make it work", import a large data set with duplicates into a warehouse while meeting the requirements of a business intelligence designer for query speed.
- Find the top 10 colleges/companies that an average social person interacts with.
- The ORDER BY command in SQL is automatically set in what format if you didn't set it: Ascending or Descending?
- When you want to delete or add a column of a table in a database, what command will you use?
- What command would you want to use if you want to keep all the info on the left table?
- If you want to combine two columns after removing two duplicates, would you use UNION or UNION ALL?
- What is the term used to select non-duplicates in SQL?
- Given a raw data table, how would you write the SQL to perform the ETL to get data into a desired format?
- Perform a merge-sort with SQL only.
- A table has two data entries every day for # of apples and oranges sold. Write a query to get the difference between the apples and oranges sold on a given day.
- Given a database schema showing product sales: calculate what percent of our sales transactions had a valid promotion applied. And what % of sales happened on the first and last day of the promotion?
- Display the most common name in a table.
- Write a query that returns product_family, units_sold, percentage of promoted.
- Write a query that returns percentage of unsold product_category.
- Write a function to count the number of times each character appears in a string and rewrite the string in that format.
- Given a table of users and whom they sent friend requests to, what is the average number of friend requests sent in the last week?
- Given a schema progressively ask SQL queries ranging from basic to window functions.



---


*few More interview Questions*


- **Amazon**: Design a petabyte-scale dimensional model with hybrid SCD implementations for Amazon marketplace[](https://www.projectpro.io/article/data-modeling-interview-questions-and-answers/597)
- **Netflix**: Model slowly changing dimensions with effective dating for content catalog management[](https://prepfully.com/interview-guides/netflix-data-engineer)
- **Uber**: Create bridge tables for many-to-many relationships with weighted allocations for driver-rider matching[](https://datalemur.com/blog/uber-sql-interview-questions)
- **Meta**: Design conformed dimensions across multiple business units with master data management[](https://datalemur.com/blog/meta-data-scientist-interview-guide)
- **Microsoft**: Model hierarchical dimensions with recursive relationships for organizational analytics[](https://prepfully.com/interview-guides/microsoft-data-engineer)
- **Apple**: Create factless fact tables for App Store event tracking and coverage analysis[](https://datalemur.com/blog/apple-sql-interview-questions)
- **Google**: Design aggregate fact tables with rollup strategies for search volume analytics[](https://digitaldefynd.com/IQ/useful-data-engineering-case-studies/)
- **Airbnb**: Model multi-grain fact tables for booking analytics with different detail levels[](https://datalemur.com/blog/airbnb-sql-interview-questions)


---

*Amazon Style Data related questions





**Amazon data engineer interview questions: Data management**

**Data modeling**

- How do you create a schema that would keep track of a customer address where the address changes?
- Design a data model in order to track product from the vendor to the Amazon warehouse to delivery to the customer.
- Design a data model for a retail store.
- Create the required tables for an online store: define the necessary relations, identify primary and foreign keys, etc.
- How do you manage a table with a large number of updates, while maintaining the availability of the table for a large number of users?
- Should we apply normalization rules on a star schema?
- What's a chasm trap?

**Data warehousing**

- Give a schema for a data warehouse. 
- Design a data warehouse to capture sales.
- Design a data warehouse to help a customer support team manage tickets.
- Can you design a simple OLTP architecture that will convince the Redbus team to give X project to you?

**Data pipelines**

- Given scenario A, how would you design the pipeline for ingesting this data?
- Given a schema, create a script from scratch for an ETL to provide certain data, writing a function for each step of the process.
- How would you build a data pipeline around an AWS product, which is able to handle increasing data volume?

---

**Google data engineer interview questions: Data management**

- What type of technology would you need to build YouTube?
- How would you design a video streaming service architecture?
- Build and design your own tree.
- How do you create a schema that would keep track of a customer address where the address changes?
- Design a data model in order to track product from the vendor to the Amazon warehouse to delivery to the customer.
- Give a schema for a data warehouse. 
- Can you design a simple OLTP architecture that will convince the Redbus team to give X project to you?
- Given a schema, create a script from scratch for an ETL to provide certain data, writing a function for each step of the process.
- How would you build a data pipeline around an AWS product, which is able to handle increasing data volume?
- When is Hadoop better than PySpark?
- How do you integrate data from multiple systems?
- How do you design a comprehensive backup strategy for a million-scale data storage?




More Questions : https://datalemur.com/

https://medium.com/@consoleflare/top-10-most-asked-data-modeling-interview-questions-answers-f346ca126f82
 Read this article for general flow of questions.



---
This is fetched from 20+ sources by perplexity and complied to tackle different style of questions.
need not necessarily relevant to DE 2 Amazon data modelling round.



## **Redshift-Specific Data Modeling & Architecture**

1. **Design a petabyte-scale dimensional model in Redshift for Amazon's marketplace with hybrid SCD implementations (Types 0-7) handling 1M+ sellers and 500M+ products**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - How would you implement distribution and sort keys for optimal performance?
        
    - Design the fact and dimension tables with proper encoding and compression
        
    - Handle slowly changing dimensions for seller ratings and product categories
        
2. **Create a star schema vs snowflake schema trade-off analysis for Amazon Prime Video analytics in Redshift**[](https://www.getgalaxy.io/learn/glossary/how-to-design-star-vs-snowflake-schemas-in-redshift)
    
    - When would you choose star schema for viewing metrics vs snowflake for content metadata?
        
    - Design both schemas and justify query performance implications
        
    - Handle denormalization for fast dashboard queries vs normalized storage efficiency
        
3. **Model Amazon's order lifecycle data warehouse with substitutions, partial fulfillment, split deliveries, and Prime SLA tracking**[](https://www.datainterview.com/blog/amazon-data-engineer-interview)
    
    - Design fact tables for order events with proper grain selection
        
    - Handle late-arriving data for delivery confirmations and returns
        
    - Implement bridge tables for many-to-many product relationships
        
4. **Design a Redshift data model for Amazon Advertising with multi-touch attribution and campaign performance**[](https://www.adaface.com/blog/aws-redshift-interview-questions/)
    
    - Model impression, click, and conversion events with proper time dimensions
        
    - Handle attribution windows and cross-device tracking
        
    - Design aggregate tables for real-time bidding decisions
        
5. **Create a data warehouse schema for Amazon Web Services billing and usage analytics**[](https://www.mytectra.com/interview-question/big-data-on-aws-interview-question-and-answer)
    
    - Model resource consumption across 200+ AWS services
        
    - Handle variable pricing models and Reserved Instance allocations
        
    - Design for cost optimization reporting and chargebacks
        

## **Advanced Dimensional Modeling Concepts**

6. **Implement conformed dimensions across Amazon's multiple business units (Retail, AWS, Prime Video, Advertising)**[](https://aws.amazon.com/blogs/big-data/dimensional-modeling-in-amazon-redshift/)
    
    - Design master customer dimension with cross-business unit keys
        
    - Handle data governance and lineage across domains
        
    - Implement slowly changing hierarchies for organizational changes
        
7. **Design factless fact tables for Amazon's inventory tracking and coverage analysis**[](https://www.learnovita.com/dimensional-data-modeling-interview-questions-and-answers)
    
    - Model stockout events and availability monitoring
        
    - Track promotional coverage across products and regions
        
    - Handle snapshot fact tables for inventory levels
        
8. **Create bridge tables for Amazon's complex product categorization with weighted allocations**[](https://aws.amazon.com/blogs/big-data/dimensional-modeling-in-amazon-redshift/)
    
    - Handle products in multiple categories with allocation percentages
        
    - Model time-varying category relationships
        
    - Design for efficient category performance analysis
        
9. **Model Amazon's multi-currency financial fact tables with exchange rate handling and hedging**[](https://www.learnovita.com/dimensional-data-modeling-interview-questions-and-answers)
    
    - Design for consistent reporting across 15+ currencies
        
    - Handle historical exchange rates and restatements
        
    - Implement hedge accounting for financial derivatives
        
10. **Design aggregate fact tables with rollup strategies for Amazon's search and recommendation analytics**[](https://www.adaface.com/blog/aws-redshift-interview-questions/)
    
    - Pre-compute search volume by keyword, category, and time
        
    - Handle real-time vs batch aggregation requirements
        
    - Design for sub-second dashboard response times
        

## **Performance & Scalability Design**

11. **Optimize Redshift query performance for Amazon's customer 360 analytics with 1B+ customer records**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - Design distribution keys to minimize data movement
        
    - Implement sort keys for time-series query patterns
        
    - Handle vacuum and analyze strategies for large tables
        
12. **Design workload management (WLM) queues for Amazon's mixed analytics workloads**[](https://www.adaface.com/blog/aws-redshift-interview-questions/)
    
    - Configure queues for ETL, dashboard, and ad-hoc analytics
        
    - Handle query prioritization and resource allocation
        
    - Design for peak holiday traffic scaling
        
13. **Model Amazon's clickstream data warehouse handling 10M+ events per minute**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - Design partitioning strategy for time-series data
        
    - Handle late-arriving events and out-of-order processing
        
    - Implement data archival and lifecycle management
        
14. **Create a data model for Amazon's supply chain optimization with real-time inventory tracking**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - Design for fulfillment center, in-transit, and customer inventory
        
    - Handle demand forecasting and replenishment analytics
        
    - Model supplier performance and lead time variability
        
15. **Design Redshift Spectrum integration for Amazon's data lake analytics**[](https://www.getgalaxy.io/learn/glossary/how-to-design-star-vs-snowflake-schemas-in-redshift)
    
    - Model external tables for S3-based data lake access
        
    - Handle schema evolution and partition management
        
    - Design for cost-effective querying of historical data
        

## **Complex Business Logic Modeling**

16. **Model Amazon's subscription business analytics (Prime, AWS, Digital Services)**[](https://aws.amazon.com/blogs/big-data/dimensional-modeling-in-amazon-redshift/)
    
    - Handle subscription lifecycle events and state transitions
        
    - Design churn prediction and customer lifetime value models
        
    - Track cross-selling and upselling opportunities
        
17. **Design a data model for Amazon's marketplace seller analytics platform**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - Model seller performance metrics and rankings
        
    - Handle fee calculations and revenue sharing
        
    - Design for seller self-service analytics
        
18. **Create a warehouse model for Amazon's logistics and delivery optimization**[](https://www.datainterview.com/blog/amazon-data-engineer-interview)
    
    - Design for route optimization and delivery time prediction
        
    - Model carrier performance and cost analytics
        
    - Handle last-mile delivery tracking and customer satisfaction
        
19. **Model Amazon's fraud detection data warehouse with transaction scoring**[](https://www.adaface.com/blog/aws-redshift-interview-questions/)
    
    - Design for real-time fraud indicators and historical patterns
        
    - Handle machine learning feature engineering requirements
        
    - Model investigation workflows and case management
        
20. **Design a data model for Amazon's customer service analytics and optimization**[](https://aws.amazon.com/blogs/big-data/dimensional-modeling-in-amazon-redshift/)
    
    - Model contact center interactions across channels
        
    - Handle sentiment analysis and resolution tracking
        
    - Design for agent performance and training analytics
        

## **Data Integration & ETL Patterns**

21. **Design ETL processes for Amazon's Change Data Capture (CDC) implementation in Redshift**[](https://www.datainterview.com/blog/amazon-data-engineer-interview)
    
    - Handle schema evolution from upstream OLTP systems
        
    - Design for near-real-time data warehouse updates
        
    - Implement conflict resolution and data quality checks
        
22. **Model Amazon's master data management with golden records for products and customers**[](https://igotanoffer.com/blogs/tech/amazon-data-engineer-interview)
    
    - Design data stewardship workflows and approval processes
        
    - Handle duplicate detection and entity resolution
        
    - Implement data quality scorecards and monitoring
        
23. **Create data lineage and impact analysis models for Amazon's data governance**[](https://aws.amazon.com/blogs/big-data/dimensional-modeling-in-amazon-redshift/)
    
    - Track data flow from sources to business reports
        
    - Handle regulatory compliance and audit requirements
        
    - Design for automated data classification and tagging
        

---


