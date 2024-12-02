# Overview #

## The Snowflake Platform ##
> Snowflake is a cloud native data platform delivered as a service
* purpose built for the cloud
* no management of hardware, updates and patches
* automatic optimizations (partitioning)

### Snowflake Releases ###
* Snowflake releases new features **_weekly_**
* The deployments happen transparently in the background; users experience no downtime or disruption of service, and are always assured of running on the most-recent release with access to the latest features.

### Data Platform Workloads ###
* Data Warehouse
  * Structured and relational data
  * ANSI standard SQL
  * ACID compliant transactions
  * Data stored in databases, schemas and tables
* Data Lake
  * Separation and scalability of storage and compute
  * Schema does not need to be defined up front
  * Native processing of semi-structured data formats
* Data Engineering
  * Data ingestion
    * batch: `COPY INTO`
    * streaming: Snowpipe
  * Separate compute clusters for different workloads
  * Tasks and Streams for data pipelines
  * Security features:
    * all data encrypted at rest and in transit
    * row-based access control
    * column-level masking
    * Multi-Factor Authentication
* Data Science
  * Centralized storage removes data management roadblocks
  * Partner ecosystem data science tools can integrate natively with Snowflake
    * Amazon SageMaker
    * DataRobot
    * Dataiku
* Data Exchange
  * Secure data sharing between accounts in the same cloud provider region
  * Data Marketplace
  * Data Exchange
  * BI with Snowflake partner ecosystem tools
* Data Applications
  * Connectors and drivers
  * UDFs and Stored Procedures
  * External UDFs (AWS Lambda or Azure Functions)
  * Snowpark

## Connecting to Snowflake
* Snowsight: Snowflake web interface/console
* SnowSQL: The Snowflake CLI client
* ODBC: client, requires a Snowflake driver
* JDBC: client, requires a Snowflake driver
* SDK (Node, Python, Kafka, Spark, Go…)

## Features ##
* ANSI SQL compliant OLAP data warehouse
* Multi-cluster, shared data architecture
* Self-tuning and self-healing
* Pay only for what you use: storage and compute
* Compute (Virtual Warehouses) can be
  * `Scaled Up/Down` by resizing the warehouse on the fly to accommodate complex, heavy workloads
  * `Scaled Out` by using multi-cluster Warehouses allowing Snowflake to automatically spin up additional compute clusters to handle large concurrent query workloads
  * Automatically suspended and resumed to accommodate periods of inactivity
* Time Travel (see below for more info)
* Fail-Safe (see below for more info)
* Live data sharing
  * Private 1:1 and 1:many data sharing
  * Public data marketplaces
  * Secure private data exchanges
* Multi-cloud: AWS, Azure, GCP
* Zero-Copy Cloning
  * Objects that can be cloned: Databases, Schemas and Tables
  * Only metadata is copied
  * Once an object is cloned, the individual copies evolve separately and their storage increases only by the amount of data that was modified in each copy.

### Time Travel ###
* Allows access to previous versions of the data - the data in its state at a point of time in the past
* A minimum of Enterprise edition required for time travel longer than 1 day, up to 90 days
* The period of time travel can be set by database, schema or table

### Fail-Safe ###
* A data backup feature
* Snowflake admins can recover and restore data up to 7 days after the Time Travel period expires (not configurable)
* Accessible only by contacting Snowflake support administrators
* Only available for permanent tables
* Fail-Safe incurs additional storage costs for the extra 7 days of data retention

## Free Trial ##
You can apply for a 30-day free trial at the following link https://signup.snowflake.com/. Trying out the Enterprise edition is recommended.


# SnowPro Core Certification MCQs

# SnowPro Core Certification MCQs (Based on Overview Content)

### 1. What is the primary responsibility of an organization in Snowflake?  
- A. Managing schemas and databases  
- B. Managing multiple Snowflake accounts  
- C. Handling network security  
- D. Monitoring virtual warehouses  
**Answer:** B

### 2. What is the unique identifier assigned to each Snowflake account within an organization?  
- A. Account Name  
- B. Account Locator  
- C. Account URL  
- D. Account Token  
**Answer:** B

### 3. Which of the following is a feature of Snowflake Organizations?  
- A. Data replication across different accounts  
- B. Centralized billing across multiple accounts  
- C. Automatic schema creation  
- D. Cross-region failover for free  
**Answer:** B

### 4. Which Snowflake role is responsible for managing organizational-level tasks such as account creation and billing?  
- A. SYSADMIN  
- B. ACCOUNTADMIN  
- C. SECURITYADMIN  
- D. ORGADMIN  
**Answer:** D

### 5. What type of object is considered a logical grouping of related data objects in Snowflake?  
- A. Virtual Warehouse  
- B. Database  
- C. Organization  
- D. Schema  
**Answer:** D

### 6. What is the purpose of the "Information Schema" in Snowflake?  
- A. Storing large datasets  
- B. Managing user accounts  
- C. Providing metadata and system information  
- D. Creating virtual warehouses  
**Answer:** C

### 7. Which of the following Snowflake objects is a logical grouping of tables, views, and other database objects?  
- A. Virtual Warehouse  
- B. Account  
- C. Schema  
- D. Organization  
**Answer:** C

### 8. What is a key feature of the Enterprise Edition of Snowflake compared to the Standard Edition?  
- A. Support for Time Travel  
- B. Availability of Fail-Safe  
- C. Multi-cluster warehouses  
- D. Unlimited data storage  
**Answer:** C

### 9. What is the default retention period for Time Travel in Snowflake Standard Edition?  
- A. 1 day  
- B. 7 days  
- C. 30 days  
- D. 90 days  
**Answer:** A

### 10. What is the purpose of a Transient Database in Snowflake?  
- A. For long-term storage with Fail-Safe support  
- B. For temporary data storage without Fail-Safe  
- C. For permanent storage with high redundancy  
- D. For storing encrypted backup data  
**Answer:** B

### 11. What is a key difference between Transient and Permanent objects in Snowflake?  
- A. Transient objects have longer Time Travel retention periods  
- B. Transient objects are not included in Fail-Safe  
- C. Transient objects can only store unstructured data  
- D. Transient objects cannot be cloned  
**Answer:** B

### 12. Which type of Snowflake schema is used to store externally managed data?  
- A. Transient Schema  
- B. Permanent Schema  
- C. External Schema  
- D. Public Schema  
**Answer:** C

### 13. What is the primary purpose of a Fail-Safe in Snowflake?  
- A. To replicate data across regions  
- B. To allow users to recover data beyond the Time Travel period  
- C. To encrypt data at rest  
- D. To automatically scale virtual warehouses  
**Answer:** B

### 14. How many days of Fail-Safe are available in Snowflake's Enterprise Edition?  
- A. 1 day  
- B. 3 days  
- C. 7 days  
- D. 10 days  
**Answer:** C

### 15. Which of the following allows a Snowflake account to be part of multiple organizations?  
- A. Account Replication  
- B. Cross-Account Data Sharing  
- C. Account Locator Sharing  
- D. Account Linking  
**Answer:** D

### 16. Which of the following is NOT a core component of Snowflake?  
- A. Cloud Services Layer  
- B. Compute Layer  
- C. Storage Layer  
- D. On-Premises Layer  
**Answer:** D

### 17. Which Snowflake feature allows account administrators to monitor usage across multiple accounts in an organization?  
- A. Data Marketplace  
- B. Cross-Account Billing  
- C. Resource Monitoring  
- D. Organizational Usage Monitoring  
**Answer:** D

### 18. In Snowflake, what does a Database contain?  
- A. Virtual Warehouses  
- B. Users and Roles  
- C. Tables, Schemas, and Views  
- D. Account Information  
**Answer:** C

### 19. What is the primary function of a Snowflake Virtual Warehouse?  
- A. Storing data  
- B. Query execution and compute  
- C. Managing network security  
- D. Monitoring organizational usage  
**Answer:** B

### 20. How does Snowflake handle storage and compute?  
- A. They are tightly coupled for efficiency  
- B. They are decoupled and can scale independently  
- C. Storage is handled on local machines  
- D. Compute is handled by third-party applications  
**Answer:** B

### 21. Which component of Snowflake is responsible for query optimization and metadata management?  
- A. Compute Layer  
- B. Cloud Services Layer  
- C. Storage Layer  
- D. Network Layer  
**Answer:** B

### 22. What type of data storage does Snowflake utilize for storing structured and semi-structured data?  
- A. Relational Database Storage  
- B. Cloud Object Storage  
- C. Local Disk Storage  
- D. In-Memory Storage  
**Answer:** B

### 23. Which of the following statements is true about Snowflake’s compute layer?  
- A. It scales automatically based on query complexity.  
- B. It stores data persistently.  
- C. It consists of virtual warehouses that provide compute resources.  
- D. It provides multi-factor authentication.  
**Answer:** C

### 24. What is the maximum Time Travel retention period for Snowflake’s Enterprise Edition?  
- A. 7 days  
- B. 30 days  
- C. 90 days  
- D. Unlimited  
**Answer:** C

### 25. Which Snowflake edition is recommended for organizations with highly sensitive data requiring the highest level of security?  
- A. Standard Edition  
- B. Enterprise Edition  
- C. Business Critical Edition  
- D. Free Trial Edition  
**Answer:** C

### 26. In Snowflake, which layer is responsible for orchestrating transactions and metadata?  
- A. Storage Layer  
- B. Compute Layer  
- C. Cloud Services Layer  
- D. Data Transfer Layer  
**Answer:** C

### 27. What happens when a Snowflake Virtual Warehouse is suspended?  
- A. It deletes all stored data.  
- B. It stops consuming compute resources.  
- C. It becomes read-only.  
- D. It switches to a backup warehouse.  
**Answer:** B

### 28. Which feature in Snowflake allows you to query semi-structured data like JSON and Parquet directly?  
- A. Data Replication  
- B. Virtual Warehouses  
- C. Native Support for Semi-Structured Data  
- D. External Tables  
**Answer:** C

### 29. What feature enables Snowflake customers to share data securely with external organizations?  
- A. Virtual Private Cloud (VPC)  
- B. Secure Data Sharing  
- C. Multi-Factor Authentication (MFA)  
- D. Data Replication  
**Answer:** B

### 30. Which of the following is a benefit of Snowflake’s multi-cluster architecture?  
- A. Centralized data storage  
- B. High availability and scalability  
- C. Manual scaling of resources  
- D. Single-user access  
**Answer:** B

### 31. What is the role of Snowflake’s Storage Layer?  
- A. Execute SQL queries  
- B. Store data persistently in cloud storage  
- C. Manage user roles and permissions  
- D. Optimize query execution  
**Answer:** B

### 32. How does Snowflake implement data redundancy?  
- A. By maintaining multiple copies of data across cloud regions  
- B. By using local hard drives for backups  
- C. By caching frequently used queries  
- D. By encrypting data at rest  
**Answer:** A

### 33. What is the function of Snowflake’s Result Cache?  
- A. Store metadata information  
- B. Cache the results of previously executed queries  
- C. Provide temporary storage for data processing  
- D. Manage failover scenarios  
**Answer:** B

### 34. Which layer of Snowflake provides the ability to access, monitor, and manage your account?  
- A. Storage Layer  
- B. Cloud Services Layer  
- C. Compute Layer  
- D. Metadata Layer  
**Answer:** B

### 35. What is a key advantage of Snowflake’s decoupled storage and compute architecture?  
- A. Increased data redundancy  
- B. Independent scaling of storage and compute  
- C. Faster disk I/O operations  
- D. In-memory query execution  
**Answer:** B

### 36. In Snowflake, what is a database used for?  
- A. Managing user accounts  
- B. Storing and organizing tables, views, and schemas  
- C. Controlling network traffic  
- D. Monitoring system performance  
**Answer:** B

### 37. Which Snowflake feature allows you to recover data after accidental deletion or corruption?  
- A. Data Replication  
- B. Fail-Safe  
- C. Time Travel  
- D. External Tables  
**Answer:** C

### 38. How does Snowflake ensure the security of data stored in the cloud?  
- A. By using third-party security services  
- B. By encrypting data in transit and at rest  
- C. By storing data on local disks  
- D. By restricting data to a single cloud region  
**Answer:** B

### 39. What is a key characteristic of Snowflake’s Business Critical Edition?  
- A. Unlimited storage capacity  
- B. Support for HIPAA and PCI compliance  
- C. Free data sharing  
- D. Auto-scaling across multiple regions  
**Answer:** B

### 40. Which Snowflake feature automatically scales the size of a Virtual Warehouse based on query load?  
- A. Auto-Suspend  
- B. Auto-Resume  
- C. Multi-Cluster Warehouses  
- D. Fail-Safe  
**Answer:** C

### 41. Which type of Snowflake table is designed for temporary data that does not require Fail-Safe?  
- A. Permanent Table  
- B. Transient Table  
- C. External Table  
- D. Temporary Table  
**Answer:** B

### 42. What is a key feature of Snowflake’s External Tables?  
- A. They support Time Travel.  
- B. They allow querying data stored in external locations like cloud storage.  
- C. They are included in Fail-Safe.  
- D. They automatically scale compute resources.  
**Answer:** B

### 43. Which feature in Snowflake helps users understand and monitor the metadata of their database objects?  
- A. Database Cache  
- B. Virtual Warehouses  
- C. Information Schema  
- D. Fail-Safe  
**Answer:** C

### 44. How does Snowflake handle updates and patches to its service?  
- A. Users must manually apply updates.  
- B. Updates are automatically applied with no downtime.  
- C. Updates require a system restart.  
- D. Updates are available only in Business Critical Edition.  
**Answer:** B

### 45. Which of the following statements is true about Snowflake’s architecture?  
- A. It stores data on local disks.  
- B. It uses a shared-nothing architecture.  
- C. It separates storage, compute, and cloud services.  
- D. It requires manual scaling of resources.  
**Answer:** C

### 46. What is the primary purpose of Snowflake’s Fail-Safe period?  
- A. To back up data to a local machine.  
- B. To allow recovery of data beyond the Time Travel period.  
- C. To improve query performance.  
- D. To secure data in transit.  
**Answer:** B

### 47. Which layer in Snowflake is responsible for managing authentication and metadata services?  
- A. Compute Layer  
- B. Storage Layer  
- C. Cloud Services Layer  
- D. Data Processing Layer  
**Answer:** C

### 48. In Snowflake, what is the purpose of Time Travel?  
- A. To increase data redundancy.  
- B. To recover previous versions of data within a specified retention period.  
- C. To migrate data between cloud providers.  
- D. To optimize query execution plans.  
**Answer:** B

### 49. Which of the following best describes Snowflake’s compute resources?  
- A. In-memory storage units  
- B. Virtual Warehouses  
- C. Distributed file systems  
- D. Multi-cloud data warehouses  
**Answer:** B

### 50. What is a key difference between Snowflake and traditional data warehouses?  
- A. Snowflake does not support SQL queries.  
- B. Snowflake requires on-premises hardware.  
- C. Snowflake decouples compute and storage.  
- D. Snowflake lacks data encryption.  
**Answer:** C

### 51. Which of the following Snowflake objects can be cloned to create an exact copy?  
- A. Virtual Warehouses  
- B. Compute Nodes  
- C. Databases, Tables, and Schemas  
- D. Information Schemas  
**Answer:** C

### 52. How does Snowflake achieve high availability for its services?  
- A. By storing data on local disks.  
- B. By using cloud-based infrastructure across multiple regions.  
- C. By requiring users to manage failover.  
- D. By limiting usage to one cloud provider.  
**Answer:** B

### 53. What is the purpose of Snowflake’s Secure Data Sharing feature?  
- A. To allow users to share credentials with external users.  
- B. To replicate data across virtual warehouses.  
- C. To share data securely between Snowflake accounts without data movement.  
- D. To store encrypted data in multiple regions.  
**Answer:** C

### 54. Which Snowflake object provides compute resources to execute queries?  
- A. Database  
- B. Schema  
- C. Virtual Warehouse  
- D. Information Schema  
**Answer:** C

### 55. What is a key benefit of Snowflake’s Multi-Cluster Warehouse?  
- A. It stores data in-memory for faster access.  
- B. It automatically scales based on query load.  
- C. It manages data encryption keys.  
- D. It provides manual control over storage size.  
**Answer:** B

### 56. Which of the following Snowflake editions offers features like Tri-Secret Secure and Data Encryption at Rest and In Transit?  
- A. Standard Edition  
- B. Enterprise Edition  
- C. Business Critical Edition  
- D. Free Trial Edition  
**Answer:** C

### 57. What happens when a Snowflake Virtual Warehouse is resumed?  
- A. It automatically scales to maximum capacity.  
- B. It starts consuming compute resources.  
- C. It restores lost data from Fail-Safe.  
- D. It moves data to cold storage.  
**Answer:** B

### 58. Which layer in Snowflake is responsible for scaling compute resources independently of data storage?  
- A. Cloud Services Layer  
- B. Compute Layer  
- C. Storage Layer  
- D. Metadata Layer  
**Answer:** B

### 59. Which Snowflake object stores data in a structured format?  
- A. Schema  
- B. Table  
- C. View  
- D. Virtual Warehouse  
**Answer:** B

### 60. What type of data can Snowflake store and query?  
- A. Only structured data  
- B. Only unstructured data  
- C. Both structured and semi-structured data  
- D. Only data in flat files  
**Answer:** C

### 61. Which feature in Snowflake enables querying data without loading it into Snowflake's storage?  
- A. Secure Views  
- B. Time Travel  
- C. External Tables  
- D. Fail-Safe  
**Answer:** C

### 62. How does Snowflake ensure that data in transit is secure?  
- A. By using local firewalls  
- B. By implementing TLS encryption  
- C. By storing data in isolated data centers  
- D. By disabling network access  
**Answer:** B

### 63. What type of Snowflake role is ACCOUNTADMIN?  
- A. A role for managing data replication  
- B. A role for administering user accounts and overall account-level settings  
- C. A role limited to querying metadata  
- D. A role for managing external tables only  
**Answer:** B

### 64. Which Snowflake object is directly responsible for executing SQL queries?  
- A. Virtual Warehouse  
- B. Schema  
- C. Information Schema  
- D. Fail-Safe  
**Answer:** A

### 65. What is a key advantage of using Snowflake’s Information Schema?  
- A. It provides direct data recovery features.  
- B. It offers metadata about the database objects and system usage.  
- C. It stores sensitive user data securely.  
- D. It automatically optimizes query execution.  
**Answer:** B

### 66. What happens to data in a Snowflake Temporary Table after the session ends?  
- A. It is moved to permanent storage.  
- B. It is automatically deleted.  
- C. It is archived in the Fail-Safe.  
- D. It remains accessible for 24 hours.  
**Answer:** B

### 67. What is a key feature of Snowflake’s Standard Edition?  
- A. Unlimited access to Multi-Cluster Warehouses  
- B. Basic security features and standard data encryption  
- C. Advanced security and Tri-Secret Secure  
- D. Support for HIPAA compliance  
**Answer:** B

### 68. Which of the following Snowflake features is designed to optimize the execution of repetitive queries?  
- A. Data Replication  
- B. Result Cache  
- C. Fail-Safe  
- D. External Tables  
**Answer:** B

### 69. How does Snowflake handle scaling for large workloads?  
- A. By automatically increasing storage allocation  
- B. By manually provisioning additional servers  
- C. By allowing Virtual Warehouses to scale out using Multi-Cluster Warehouses  
- D. By replicating queries across regions  
**Answer:** C

### 70. What is the default behavior of a Snowflake Virtual Warehouse when it becomes idle?  
- A. It continues to consume compute resources indefinitely.  
- B. It automatically suspends to stop compute usage.  
- C. It shuts down permanently.  
- D. It scales down to the smallest size.  
**Answer:** B

### 71. Which layer in Snowflake provides services like authentication, metadata management, and query optimization?  
- A. Compute Layer  
- B. Storage Layer  
- C. Cloud Services Layer  
- D. Network Layer  
**Answer:** C

### 72. What is a key difference between a Permanent Table and a Transient Table in Snowflake?  
- A. Transient Tables have longer retention periods.  
- B. Transient Tables do not have Fail-Safe.  
- C. Permanent Tables can only store semi-structured data.  
- D. Transient Tables are encrypted with different algorithms.  
**Answer:** B

### 73. Which of the following best describes Snowflake’s Zero-Copy Cloning feature?  
- A. It creates an in-memory copy of a table.  
- B. It creates a logical pointer to the original data without duplicating it.  
- C. It automatically replicates data across regions.  
- D. It converts structured data to semi-structured formats.  
**Answer:** B

### 74. What does Snowflake’s Compute Layer consist of?  
- A. Physical servers  
- B. Virtual Warehouses  
- C. Storage containers  
- D. Information Schemas  
**Answer:** B

### 75. How can a Snowflake user resume a suspended Virtual Warehouse?  
- A. By issuing a manual command or automatically via a query request  
- B. By waiting for 24 hours  
- C. By restoring it from Fail-Safe  
- D. By creating a new Virtual Warehouse  
**Answer:** A

### 76. In Snowflake, what is the function of a Database Object?  
- A. It provides compute resources for queries.  
- B. It organizes data into schemas, tables, and views.  
- C. It handles external network requests.  
- D. It manages user authentication.  
**Answer:** B

### 77. What is the primary benefit of Snowflake’s Data Sharing feature?  
- A. It replicates data across regions for free.  
- B. It allows sharing data with other Snowflake accounts without copying data.  
- C. It provides offline access to sensitive data.  
- D. It encrypts data using an external key management system.  
**Answer:** B

### 78. Which type of Snowflake warehouse configuration is best suited for unpredictable workloads?  
- A. Single-Cluster Warehouse  
- B. Multi-Cluster Warehouse  
- C. Temporary Warehouse  
- D. External Warehouse  
**Answer:** B

### 79. What is one of the core responsibilities of the ORGADMIN role in Snowflake?  
- A. Managing network firewalls  
- B. Administering user accounts across all Snowflake organizations  
- C. Monitoring data replication  
- D. Enforcing schema-level security  
**Answer:** B

### 80. Which Snowflake feature allows administrators to monitor resource usage and set limits to prevent overuse?  
- A. Secure Data Sharing  
- B. Resource Monitors  
- C. Result Cache  
- D. Fail-Safe  
**Answer:** B

### 81. What is the primary purpose of Snowflake’s Fail-Safe feature?  
- A. To optimize query performance  
- B. To provide disaster recovery beyond Time Travel  
- C. To encrypt data in transit  
- D. To allow manual backups by users  
**Answer:** B

### 82. In Snowflake, which of the following statements is true regarding data partitioning?  
- A. Users must manually define partitions.  
- B. Data is automatically partitioned by Snowflake.  
- C. Partitioning is only available for structured data.  
- D. Partitioning requires additional configuration during table creation.  
**Answer:** B

### 83. What is a key benefit of Snowflake’s multi-cloud support?  
- A. Provides hybrid on-premises and cloud solutions  
- B. Allows organizations to store data across multiple cloud providers  
- C. Eliminates the need for encryption  
- D. Requires a single virtual warehouse for all regions  
**Answer:** B

### 84. How does Snowflake manage scaling when using a Multi-Cluster Warehouse?  
- A. By automatically adding and removing clusters based on query load  
- B. By allowing only manual scaling of resources  
- C. By scaling storage based on data size  
- D. By suspending idle queries  
**Answer:** A

### 85. Which Snowflake feature allows users to revert a table to a previous state?  
- A. Time Travel  
- B. Fail-Safe  
- C. Data Replication  
- D. Secure Data Sharing  
**Answer:** A

### 86. What happens to a suspended Snowflake Virtual Warehouse?  
- A. It stops consuming compute resources.  
- B. It deletes all cached query results.  
- C. It moves data to Fail-Safe.  
- D. It automatically scales down storage.  
**Answer:** A

### 87. Which of the following Snowflake features helps optimize resource usage and control costs?  
- A. Resource Monitors  
- B. External Tables  
- C. Fail-Safe  
- D. Multi-Cluster Warehouses  
**Answer:** A

### 88. What type of data structure is typically used in Snowflake to organize and store data?  
- A. Views  
- B. Tables  
- C. JSON files  
- D. Network files  
**Answer:** B

### 89. What is one of the major advantages of Snowflake’s Zero-Copy Cloning feature?  
- A. Clones are physically stored in a new location.  
- B. Clones only require metadata pointers, not physical duplication.  
- C. Clones are automatically replicated across regions.  
- D. Clones cannot be altered independently of the original object.  
**Answer:** B

### 90. Which Snowflake feature provides insights into system usage, query history, and storage consumption?  
- A. Result Cache  
- B. Information Schema  
- C. Secure Views  
- D. Resource Monitors  
**Answer:** B

### 91. In Snowflake, how is semi-structured data such as JSON or Parquet typically stored?  
- A. In flat files  
- B. In external tables only  
- C. Directly in Snowflake tables with automatic optimization  
- D. In virtual warehouses  
**Answer:** C

### 92. Which of the following best describes Snowflake’s virtual warehouses?  
- A. They store user data permanently.  
- B. They execute SQL queries and provide compute resources.  
- C. They manage metadata and data replication.  
- D. They serve as a backup for storage layers.  
**Answer:** B

### 93. What is a key advantage of Snowflake’s native support for semi-structured data?  
- A. Requires no schema definition  
- B. Allows for dynamic querying with SQL  
- C. Converts all semi-structured data to CSV automatically  
- D. Requires specialized file formats for data loading  
**Answer:** B

### 94. How does Snowflake ensure data is highly available and reliable across regions?  
- A. By maintaining separate copies of data in each virtual warehouse  
- B. By replicating data across multiple availability zones in a cloud region  
- C. By requiring users to manage backups manually  
- D. By only supporting a single cloud provider per account  
**Answer:** B

### 95. What type of Snowflake role is designed to manage account-level operations, including billing and virtual warehouse management?  
- A. SYSADMIN  
- B. ACCOUNTADMIN  
- C. SECURITYADMIN  
- D. ORGADMIN  
**Answer:** B

### 96. What happens when a Snowflake query is executed and the result is stored in the Result Cache?  
- A. The result is available for future identical queries without recomputation.  
- B. The result is automatically encrypted and archived.  
- C. The query result is shared across multiple Snowflake accounts.  
- D. The result is stored permanently in cloud storage.  
**Answer:** A

### 97. How does Snowflake handle schema changes in tables containing large volumes of data?  
- A. It requires a full reload of the table.  
- B. It dynamically applies schema changes without data reloading.  
- C. It only supports schema changes during off-peak hours.  
- D. It stores the old schema in an external file.  
**Answer:** B

### 98. Which Snowflake feature allows organizations to securely and instantly share data with external partners without physical data movement?  
- A. Secure Data Sharing  
- B. Fail-Safe  
- C. Resource Monitors  
- D. External Tables  
**Answer:** A

### 99. How does Snowflake implement automatic failover in case of a regional outage?  
- A. By maintaining live data replication across regions  
- B. By caching queries locally  
- C. By suspending all compute operations  
- D. By reverting to on-premises storage  
**Answer:** A

### 100. Which Snowflake table type is recommended for staging and temporary data that does not require Time Travel or Fail-Safe?  
- A. Permanent Table  
- B. Transient Table  
- C. Temporary Table  
- D. External Table  
**Answer:** B
