# Data Storage Layer #

The Data Storage Layer is Snowflake managed cloud-based storage available in the major cloud providers (AWS, Azure and GCP).
* Full native support for semi-structured data: JSON, AVRO, ORC, XML and Parquet
* Customers are billed for data based on its compressed size as well as any files existing in internal (Snowflake-managed) stages

## Micro-Partitions ##
All data in Snowflake tables is automatically divided into micro-partitions. Micro-Partitions are contiguous units of storage that generally hold a maximum of 16MB or encrypted, compressed data (uncompressed the data is between 50-500MB), organized in a columnar way.

![](../images/MicroPartitionDataOrganization.png)

* Micro-partitions are IMMUTABLE and versioned
  * In order to modify data in a micro-partition, a new version of the micro-partition must be created. The older micro-partition is then marked for deletion
  * This is what enables the time travel feature in Snowflake
* Data is compressed within each micro-partition
  * Within each partition, data is organized in a hybrid columnar format
  * On load, data is automatically analyzed for the optimal compression scheme based on its format
  * Organizing data based on columns containing data of the same type allows for efficient compression which reduces storage and I/O
* Snowflake attempts to preserve natural data co-location
  * Since data is organized in micro-partitions on load, some natural data clustering and optimization occurs
* Table records are contained entirely within a micro-partition, they cannot span multiple partitions

### Query Pruning ###
The metadata stored in the Cloud Services Layer allows Snowflake to minimize the data it needs to load from the storage layer in order to resolve a query. Snowflake performs two types of pruning:
* micro-partition pruning - reading only the micro-partition files needed to resolve a query
* column pruning - only reading the columns that we need from each micro-partition

The Cloud Services Layer metadata also allows the processing of certain queries (e.g, MIN, MAX) at the Cloud Services Layer without engaging the compute or storage layers. The metadata tracked is:

* At the table level:
  * row count
  * table size (in bytes)
  * micro-partition references and table versions
* At the Micro-Partition level:
  * MIN/MAX (range of values) in each column
    * this allows micro-partition pruning during query optimization
  * number of distinct values
  * NULL count

## Data Storage Billing ##
* Customers are billed for actual (compressed) storage used based on the daily average, billed per Terabyte per month
  * On-demand Pricing: Billed in arrears at $40 per Terabyte per month with a minimum charge of $25
  * Pre-purchased Capacity:
    * Billed upfront for committed storage capacity
    * Price varies depending on cloud platform
    * Customer is notified when they have consumed 70% of pre-purchased capacity

## MCQs

### 1. How is data stored in Snowflake?
- A. In a flat file structure
- B. In micro-partitions that are compressed and organized in a columnar format
- C. In a row-based storage format
- D. In a proprietary binary format without compression
<details><summary>Answer</summary> B. In micro-partitions that are compressed and organized in a columnar format</details>

### 2. What is the maximum size of a micro-partition in Snowflake?
- A. 1 MB
- B. 16 MB of compressed data
- C. 500 MB of uncompressed data
- D. 100 MB of compressed data
<details><summary>Answer</summary> B. 16 MB of compressed data</details>

### 3. Which of the following statements is true regarding micro-partitions in Snowflake?
- A. Micro-partitions can span across multiple rows
- B. Data in micro-partitions is immutable and versioned
- C. Micro-partitions are stored in a non-columnar format
- D. Data in micro-partitions can be updated in-place
<details><summary>Answer</summary> B. Data in micro-partitions is immutable and versioned</details>

### 4. What is the purpose of Snowflake’s data compression within micro-partitions?
- A. To reduce storage and improve performance by minimizing I/O
- B. To increase the speed of the compute layer
- C. To allow for data redundancy and replication
- D. To store more data without increasing costs
<details><summary>Answer</summary> A. To reduce storage and improve performance by minimizing I/O</details>

### 5. How does Snowflake improve query performance using pruning techniques?
- A. By loading all data from all micro-partitions into memory
- B. By skipping unnecessary columns and micro-partitions during query execution
- C. By parallelizing all queries across all micro-partitions
- D. By keeping only the latest version of data in memory
<details><summary>Answer</summary> B. By skipping unnecessary columns and micro-partitions during query execution</details>

### 6. Which of the following types of pruning is used by Snowflake during query optimization?
- A. Query pruning
- B. Column pruning
- C. Row pruning
- D. Table pruning
<details><summary>Answer</summary> B. Column pruning</details>

### 7. What kind of data is Snowflake designed to handle in its Data Storage Layer?
- A. Only structured data
- B. Only semi-structured data like JSON and AVRO
- C. Both structured and semi-structured data
- D. Only unstructured data like images
<details><summary>Answer</summary> C. Both structured and semi-structured data</details>

### 8. What is the cost structure for Snowflake storage based on?
- A. Data ingestion speed
- B. Uncompressed data size
- C. Compressed data size and files in internal stages
- D. Number of queries executed
<details><summary>Answer</summary> C. Compressed data size and files in internal stages</details>

### 9. How is Snowflake’s storage billed in terms of usage?
- A. By the total number of queries executed
- B. By compressed data size, billed per terabyte per month
- C. By the number of micro-partitions used
- D. By the number of warehouses running
<details><summary>Answer</summary> B. By compressed data size, billed per terabyte per month</details>

### 10. What happens if the credit usage for Snowflake’s pre-purchased storage capacity exceeds 70%?
- A. Snowflake automatically suspends storage usage
- B. Snowflake sends a notification to the customer
- C. The customer is automatically billed for additional storage
- D. The customer cannot use any more storage
<details><summary>Answer</summary> B. Snowflake sends a notification to the customer</details>

### 11. Which of the following is tracked at the micro-partition level in Snowflake?
- A. Data compression type
- B. Number of distinct values
- C. Row count
- D. Table size
<details><summary>Answer</summary> B. Number of distinct values</details>

### 12. What is the role of micro-partition pruning in Snowflake’s query optimization?
- A. It eliminates redundant rows in the data
- B. It skips unnecessary columns and micro-partitions
- C. It reduces storage space for data
- D. It compresses data in real-time
<details><summary>Answer</summary> B. It skips unnecessary columns and micro-partitions</details>

### 13. How does Snowflake organize data within micro-partitions?
- A. Row-based storage
- B. Columnar storage
- C. Hybrid row-column storage
- D. File-based storage
<details><summary>Answer</summary> B. Columnar storage</details>

### 14. Which of the following data types is **NOT** natively supported by Snowflake's Data Storage Layer?
- A. JSON
- B. XML
- C. AVRO
- D. Parquet
<details><summary>Answer</summary> None (All are supported natively)</details>

### 15. What is the billing model for Snowflake’s data storage?
- A. Based on the number of queries processed
- B. Based on data volume stored (compressed size)
- C. Based on the number of tables in the warehouse
- D. Based on data retrieval speed
<details><summary>Answer</summary> B. Based on data volume stored (compressed size)</details>

### 16. What happens when Snowflake performs a query on a table with micro-partitions?
- A. It reads all the data from all micro-partitions
- B. It prunes unnecessary micro-partitions and columns based on metadata
- C. It scans only the first micro-partition
- D. It loads all micro-partitions into memory before processing
<details><summary>Answer</summary> B. It prunes unnecessary micro-partitions and columns based on metadata</details>

### 17. How does Snowflake handle immutable micro-partitions?
- A. It merges micro-partitions with the same data
- B. It creates a new version of the micro-partition when data is modified
- C. It deletes the old partition once the new one is created
- D. It allows modification of the existing partition
<details><summary>Answer</summary> B. It creates a new version of the micro-partition when data is modified</details>

### 18. What is the size of data that Snowflake stores in micro-partitions (uncompressed)?
- A. 10-50 MB
- B. 50-500 MB
- C. 1-2 GB
- D. 500 MB-2 GB
<details><summary>Answer</summary> B. 50-500 MB</details>

### 19. When does Snowflake reset the credit usage for data storage?
- A. After the completion of each query
- B. Every time a micro-partition is created
- C. At the specified frequency (e.g., daily, weekly)
- D. After 12 AM UTC every day
<details><summary>Answer</summary> C. At the specified frequency (e.g., daily, weekly)</details>

### 20. Which of the following actions does Snowflake perform during query optimization for micro-partitions?
- A. Loading the entire table into memory
- B. Reading only the relevant micro-partitions based on metadata
- C. Scanning every column in the table
- D. Ignoring metadata and scanning all micro-partitions
<details><summary>Answer</summary> B. Reading only the relevant micro-partitions based on metadata</details>

### 21. Which of the following best describes the storage format used by Snowflake for semi-structured data?
- A. Text files
- B. Row-based storage
- C. Columnar storage with compression
- D. JSON storage only
<details><summary>Answer</summary> C. Columnar storage with compression</details>

### 22. What is the primary factor that Snowflake uses to determine the compression scheme for data in micro-partitions?
- A. The size of the data
- B. The format of the data
- C. The row count
- D. The number of distinct values in a column
<details><summary>Answer</summary> B. The format of the data</details>

### 23. How does Snowflake handle the time travel feature for data in micro-partitions?
- A. It uses a snapshot copy of data to allow time travel
- B. It modifies the micro-partition directly
- C. It re-creates partitions every time a query is run
- D. It maintains multiple versions of each micro-partition
<details><summary>Answer</summary> D. It maintains multiple versions of each micro-partition</details>

### 24. What is the purpose of column pruning in Snowflake's query optimization process?
- A. To read only the required columns for a query and reduce I/O
- B. To remove unnecessary columns from the database
- C. To re-arrange the columns for better compression
- D. To ensure that all columns are indexed
<details><summary>Answer</summary> A. To read only the required columns for a query and reduce I/O</details>

### 25. How is data compressed in Snowflake’s micro-partitions?
- A. Compression is done manually by users
- B. Compression is automatically determined based on data type and format
- C. Data is not compressed within micro-partitions
- D. Compression only occurs when data exceeds 1 GB
<details><summary>Answer</summary> B. Compression is automatically determined based on data type and format</details>

### 26. How is Snowflake billed for storage usage?
- A. Billed based on the number of queries executed
- B. Billed based on uncompressed data size
- C. Billed based on the number of micro-partitions created
- D. Billed based on the compressed size of the data stored
<details><summary>Answer</summary> D. Billed based on the compressed size of the data stored</details>

### 27. Which of the following is **NOT** a factor considered when determining the cost of Snowflake's storage?
- A. Compressed data size
- B. Frequency of queries run on the data
- C. Number of micro-partitions
- D. Cloud platform (AWS, Azure, GCP)
<details><summary>Answer</summary> B. Frequency of queries run on the data</details>

### 28. What is the storage billing for Snowflake's on-demand pricing model?
- A. Billed monthly per query processed
- B. Billed daily per row count
- C. Billed per Terabyte of compressed data stored, with a minimum charge of $25
- D. Billed based on the number of micro-partitions accessed
<details><summary>Answer</summary> C. Billed per Terabyte of compressed data stored, with a minimum charge of $25</details>

### 29. What is the primary benefit of storing data in micro-partitions?
- A. Improved indexing
- B. Enhanced query performance due to automatic pruning and columnar organization
- C. Faster compression algorithms
- D. Reduced redundancy of data
<details><summary>Answer</summary> B. Enhanced query performance due to automatic pruning and columnar organization</details>

### 30. Which Snowflake feature relies on the metadata of micro-partitions to optimize queries?
- A. Time Travel
- B. Query Pruning
- C. Data Sharing
- D. Automatic Clustering
<details><summary>Answer</summary> B. Query Pruning</details>

### 31. What is the default size of Snowflake's micro-partitions?
- A. 1MB
- B. 10MB
- C. 16MB
- D. 50MB
<details><summary>Answer</summary> C. 16MB</details>

### 32. When is Snowflake’s cloud storage usage billed?
- A. Monthly, based on total data volume
- B. Annually, based on total data volume
- C. Daily, based on average data volume
- D. Only when data is queried
<details><summary>Answer</summary> C. Daily, based on average data volume</details>

### 33. How does Snowflake optimize query performance with micro-partitions?
- A. By compressing all data
- B. By pruning unneeded micro-partitions and columns
- C. By caching query results
- D. By indexing data in a B-tree format
<details><summary>Answer</summary> B. By pruning unneeded micro-partitions and columns</details>

### 34. Which of the following is **NOT** stored as part of Snowflake’s metadata for micro-partitions?
- A. Row count
- B. Table size
- C. Column data types
- D. NULL count
<details><summary>Answer</summary> C. Column data types</details>

### 35. What is the main purpose of Snowflake’s data compression in micro-partitions?
- A. To save on compute costs
- B. To reduce storage costs and improve query performance
- C. To prevent data loss
- D. To speed up data loading
<details><summary>Answer</summary> B. To reduce storage costs and improve query performance</details>

### 36. In Snowflake, what determines whether a micro-partition is read during a query?
- A. The number of columns in the partition
- B. The range of values in each column
- C. The row count in the partition
- D. The partition's location in the storage
<details><summary>Answer</summary> B. The range of values in each column</details>

### 37. How does Snowflake handle unmodified micro-partitions?
- A. They are deleted after a certain period
- B. They are marked immutable and not modified
- C. They are compressed again to save space
- D. They are kept in a read-only state
<details><summary>Answer</summary> B. They are marked immutable and not modified</details>

### 38. Which of the following best describes how Snowflake’s Time Travel feature works?
- A. It allows you to travel through time to see historical data
- B. It creates multiple versions of micro-partitions for data retention
- C. It stores all changes to data as separate records
- D. It replicates data across different time zones
<details><summary>Answer</summary> B. It creates multiple versions of micro-partitions for data retention</details>

### 39. What happens to the storage used by data in Snowflake after a query is executed?
- A. Storage usage increases due to query logs
- B. Storage is not impacted by query execution
- C. Storage is cleared after each query
- D. Storage increases temporarily until the query is completed
<details><summary>Answer</summary> B. Storage is not impacted by query execution</details>

### 40. Which of the following is **NOT** a storage feature of Snowflake?
- A. Automatic data compression
- B. Data partitioning based on columns
- C. Hybrid row and columnar storage
- D. Manual data clustering
<details><summary>Answer</summary> D. Manual data clustering</details>

### 41. What happens when a micro-partition is marked for deletion in Snowflake?
- A. It is deleted immediately.
- B. It is kept for the duration of the Time Travel retention period.
- C. It is permanently deleted from storage.
- D. It is archived in a separate storage area.
<details><summary>Answer</summary> B. It is kept for the duration of the Time Travel retention period.</details>

### 42. How is Snowflake storage charged for data compression?
- A. Snowflake charges for both compressed and uncompressed data size.
- B. Snowflake only charges for the compressed size of the data.
- C. Snowflake charges based on the number of micro-partitions.
- D. Snowflake charges separately for each column in a table.
<details><summary>Answer</summary> B. Snowflake only charges for the compressed size of the data.</details>

### 43. What feature of Snowflake allows it to perform query pruning?
- A. Table clustering
- B. Columnar storage
- C. Micro-partition metadata
- D. Data encryption
<details><summary>Answer</summary> C. Micro-partition metadata</details>

### 44. How are Snowflake storage costs determined?
- A. Based on the number of queries executed.
- B. Based on the uncompressed size of the data.
- C. Based on the amount of data stored and its compression ratio.
- D. Based on the cloud platform used.
<details><summary>Answer</summary> C. Based on the amount of data stored and its compression ratio.</details>

### 45. Which of the following is **NOT** a supported file format in Snowflake's Data Storage Layer?
- A. JSON
- B. AVRO
- C. ORC
- D. Excel
<details><summary>Answer</summary> D. Excel</details>

### 46. What is the storage pricing model for Snowflake?
- A. Billed based on active queries.
- B. Billed based on daily average storage usage.
- C. Billed monthly with a flat fee for any amount of storage.
- D. Billed hourly based on data processing requirements.
<details><summary>Answer</summary> B. Billed based on daily average storage usage.</details>

### 47. Which action does Snowflake take when it needs to modify data in an existing micro-partition?
- A. It updates the data in place.
- B. It creates a new micro-partition and marks the old one for deletion.
- C. It creates a new version of the entire table.
- D. It updates the data in the same micro-partition and compresses it.
<details><summary>Answer</summary> B. It creates a new micro-partition and marks the old one for deletion.</details>

### 48. What is the default billing unit for Snowflake data storage?
- A. Per megabyte
- B. Per gigabyte
- C. Per terabyte per month
- D. Per query
<details><summary>Answer</summary> C. Per terabyte per month</details>

### 49. How does Snowflake ensure efficient query processing with large datasets?
- A. By using horizontal scaling
- B. By storing data in large, uncompressed files
- C. By storing data in micro-partitions that are pruned and compressed
- D. By caching all data in memory
<details><summary>Answer</summary> C. By storing data in micro-partitions that are pruned and compressed</details>

### 50. How does Snowflake handle the storage of semi-structured data?
- A. Semi-structured data is automatically converted into tables.
- B. Semi-structured data is stored as-is in its native format, such as JSON or XML.
- C. Semi-structured data is not supported in Snowflake.
- D. Semi-structured data must be transformed into a columnar format before storage.
<details><summary>Answer</summary> B. Semi-structured data is stored as-is in its native format, such as JSON or XML.</details>

