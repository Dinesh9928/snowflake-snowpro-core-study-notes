# Virtual Warehouses #

Virtual Warehouses (VWs) are the compute layer in Snowflake's architecture. It is a logical wrapper around a cluster of servers with CPU, memory and disk. Data Warehouses are used to perform queries and DML operations (like loading data into tables).
* The actual compute underlying the VW is fully managed by Snowflake
* While running, even if idle, a VW consumes Snowflake credits - you are charged for compute
* VWs can be Standard or Multi-Cluster Warehouses (MCW). Standard Warehouses have only a single compute cluster and cannot scale out

## SCALE OUT/IN: Multi-Cluster Warehouses (MCW) ## 
MCWs can automatically `SCALE OUT/IN` by spawning (or shutting down) additional compute clusters
* used for handling spikes in concurrent compute requests
* MCWs are a feature of Snowflake Enterprise Edition and higher
* MCWs can set to scale out to a maximum of 10 clusters
* The number of credits billed is calculated based on the size and the number of warehouses that run within the time period
* MCWs scale out under two Scaling Policies - Standard and Economy
  * Standard policy: starts additional clusters if a query is queueing or expected to queue
  * Economy policy: will start an additional cluster only after at least 6 minutes of workload have been queued up for the future cluster
* Incoming queries are load-balanced across the available running clusters

## SCALE UP/DOWN: Resizing Warehouses ##
A Virtual Warehouse can `SCALE UP/DOWN` (be resized), manually, at any time, even while running
  * used for handling complex, long-running queries or ingesting large data sets
  * when resized while running, currently executing queries will complete at the current size; queued and new queries will execute at the new size
  * Efficient scale up/down should give approximately linearly proportionate results, e.g. running a query on a double-sized warehouse should cut the query run time in half.
  * It is recommended to keep queries and workloads of similar complexity and with similar compute demands on the same warehouse to simplify compute resource size management
  * Resizing can be included in SQL scripts so a warehouse can be scaled up/down depending on the demands of subsequent queries

## Creating Warehouses ##
* One can create an unlimited number of warehouses in their account.
* XS (Extra Small) is the smallest size of VW with one server per cluster.
  * each size up doubles the number of servers in the cluster as well as the number of Snowflake credits consumed
  * sizes range from XS, through S, M, L, XL, 2XL up to 6XL (currently)
    * the default size for warehouses created through the web interface is XL
    * the default size for warehouses created using SQL is XS
    * 5XL and 6XL warehouses are generally available in AWS regions, and in preview in US Government and Azure regions.
* A VW can be set to auto-suspend when idle for a configurable number of minutes.
  * By default, the auto-suspend option is enabled.
  * Through the web UI auto-suspend can be set as low as 5 minutes
  * Through SQL auto-suspend can be set to the minimum auto-suspend setting of 1 minute (60 seconds)
  * VWs can also be started or suspended manually
* A VW can also be set to auto-resume when a new query is submitted
  * Auto-resume is enabled by default 
  * A VW will automatically start when first created
  * In SQL a VW can be created in suspended mode by setting the `INITIALLY_SUSPENDED = TRUE` option
* When a VW is first created, it has, by default, the USAGE permission granted to the role that owns/creates it since ownership defaults to the object creator
 
## Compute Credits ##
> For more info, see [Understanding Compute Cost](https://docs.snowflake.com/en/user-guide/cost-understanding-compute)
* Customer-managed compute is charged per second with a 60-second minimum each time the warehouse starts
* Snowflake-created serverless (background) compute is charged per second with no minimum.
* Credit cost varies depending on cloud provider and region
* One server in the VW cluster uses one credit per hour, so, for example, an XS (1 server) will use one credit per hour while a large (8 servers) will use 8 credist per hour

### Cloud Services Compute Billing ###
* Customers are charged for cloud computing (e.g. in the Cloud Services Layer) that exceeds 10% of the total compute costs for the account.
* Use the `WAREHOUSE_METERING_HISTORY` view to see how much cloud compute your account is using

## Transactions ##
A transaction is a sequence of SQL statements that are committed or rolled back as a unit. All statements in the transaction are either applied (i.e., committed) or undone (i.e., rolled back) together. For this reason, transactions are ACID (Atomicity, Consistency, Isolation & Durability) compliant.

* If a session is disconnected for whatever reason and a transaction remains in a detached state which cannot be committed or rolled back, Snowflake takes 4 hours to abort a transaction and roll it back.
* You can abort a running transaction with the system function `SYSTEM$ABORT_TRANSACTION `
* Each transaction has independent scope
* Snowflake does not support nested Transactions, although it supports nested Stored Procedure calls

  ### MCQs

**1. What is the main purpose of a Virtual Warehouse in Snowflake?**  
- A. Data storage  
- B. Query execution and DML operations  
- C. Metadata management  
- D. Cloud cost optimization  
<details><summary>Answer</summary> B</details>

**2. What happens if a Virtual Warehouse (VW) is idle but running?**  
- A. It stops consuming credits.  
- B. It continues consuming Snowflake credits.  
- C. It auto-resizes to minimize costs.  
- D. It switches to fail-safe mode.  
<details><summary>Answer</summary> B</details>

**3. Which feature is exclusive to Multi-Cluster Warehouses (MCW)?**  
- A. Auto-suspend  
- B. Scaling out/in  
- C. Scaling up/down  
- D. Load balancing within a single cluster  
<details><summary>Answer</summary> B</details>

**4. What is the smallest size of a Virtual Warehouse in Snowflake?**  
- A. XS  
- B. S  
- C. M  
- D. Micro  
<details><summary>Answer</summary> A</details>

**5. Which scaling policy starts an additional cluster only after at least 6 minutes of queued workload?**  
- A. Standard  
- B. Economy  
- C. Adaptive  
- D. Elastic  
<details><summary>Answer</summary> B</details>

**6. Which size is the default for Virtual Warehouses created using SQL?**  
- A. XS  
- B. S  
- C. M  
- D. XL  
<details><summary>Answer</summary> A</details>

**7. What happens when a Virtual Warehouse is resized while running?**  
- A. Current queries are re-executed at the new size.  
- B. Current queries complete at the current size; new queries use the new size.  
- C. Current queries are canceled.  
- D. Queued queries are paused until resizing completes.  
<details><summary>Answer</summary> B</details>

**8. Which feature ensures that a Virtual Warehouse automatically starts when a query is submitted?**  
- A. Auto-suspend  
- B. Auto-resume  
- C. Scaling policy  
- D. Initialization mode  
<details><summary>Answer</summary> B</details>

**9. What is the minimum auto-suspend setting through SQL?**  
- A. 1 second  
- B. 1 minute  
- C. 5 minutes  
- D. 60 minutes  
<details><summary>Answer</summary> B</details>

**10. How is the credit usage calculated for Multi-Cluster Warehouses?**  
- A. Based only on the largest cluster in use.  
- B. Based on the size and number of clusters running during the time period.  
- C. Based on the average size of the warehouse.  
- D. Based on the number of concurrent users.  
<details><summary>Answer</summary> B</details>

**11. What is the minimum chargeable runtime for a Virtual Warehouse per start?**  
- A. 10 seconds  
- B. 30 seconds  
- C. 60 seconds  
- D. 120 seconds  
<details><summary>Answer</summary> C</details>

**12. What does resizing a warehouse primarily achieve?**  
- A. Reduces storage costs.  
- B. Optimizes the network latency.  
- C. Allows for faster query execution or data ingestion.  
- D. Improves Time Travel retention.  
<details><summary>Answer</summary> C</details>

**13. Which of the following is true about nested transactions in Snowflake?**  
- A. Nested transactions are supported.  
- B. Nested transactions are partially supported.  
- C. Nested transactions are not supported.  
- D. Nested transactions depend on the SQL version used.  
<details><summary>Answer</summary> C</details>

**14. How long does Snowflake wait before aborting a detached transaction?**  
- A. 1 hour  
- B. 2 hours  
- C. 4 hours  
- D. 8 hours  
<details><summary>Answer</summary> C</details>

**15. Which scaling method is used to handle complex, long-running queries?**  
- A. Scaling up/down  
- B. Scaling out/in  
- C. Scaling horizontally  
- D. Scaling linearly  
<details><summary>Answer</summary> A</details>

**16. How does Snowflake calculate the credits for a Virtual Warehouse?**  
- A. By the number of queries executed.  
- B. By the size of the warehouse and its runtime.  
- C. By the amount of data stored.  
- D. By the complexity of the query workload.  
<details><summary>Answer</summary> B</details>

**17. What is the default size for Virtual Warehouses created through the web interface?**  
- A. XS  
- B. S  
- C. L  
- D. XL  
<details><summary>Answer</summary> D</details>

**18. Which of the following is true about Auto-Resume for Virtual Warehouses?**  
- A. It is disabled by default.  
- B. It is used to resume suspended warehouses when a query is submitted.  
- C. It only works with Multi-Cluster Warehouses.  
- D. It prevents warehouses from being resized.  
<details><summary>Answer</summary> B</details>

**19. What happens when queries are submitted to a Multi-Cluster Warehouse (MCW)?**  
- A. All queries run on the same cluster.  
- B. Queries are load-balanced across all active clusters.  
- C. New clusters are immediately created for every query.  
- D. Queries queue until one cluster finishes.  
<details><summary>Answer</summary> B</details>

**20. What is the minimum size of a Multi-Cluster Warehouse?**  
- A. One cluster  
- B. Two clusters  
- C. Four clusters  
- D. Ten clusters  
<details><summary>Answer</summary> A</details>

**21. Which policy allows a Multi-Cluster Warehouse to start additional clusters only after workload queues for at least 6 minutes?**  
- A. Standard Policy  
- B. Economy Policy  
- C. Load-Balancing Policy  
- D. High-Priority Policy  
<details><summary>Answer</summary> B</details>

**22. What happens to currently running queries during a warehouse resize?**  
- A. They are rerun on the new size.  
- B. They continue to execute at the original size.  
- C. They are terminated.  
- D. They are paused until the resize is complete.  
<details><summary>Answer</summary> B</details>

**23. Which scaling operation involves adding or removing compute clusters in a Multi-Cluster Warehouse?**  
- A. Scale Up/Down  
- B. Scale Out/In  
- C. Scale Forward/Backward  
- D. Dynamic Scaling  
<details><summary>Answer</summary> B</details>

**24. How does Snowflake charge for cloud services compute that exceeds 10% of total compute costs?**  
- A. By applying a fixed surcharge.  
- B. By charging per query executed.  
- C. By using Snowflake credits.  
- D. By increasing storage fees.  
<details><summary>Answer</summary> C</details>

**25. What is the default suspend setting for a Virtual Warehouse when created via the web UI?**  
- A. 1 minute  
- B. 5 minutes  
- C. 10 minutes  
- D. Disabled  
<details><summary>Answer</summary> B</details>

**26. How many credits does a 2XL warehouse with four servers consume per hour?**  
- A. 2 credits  
- B. 4 credits  
- C. 8 credits  
- D. 16 credits  
<details><summary>Answer</summary> D</details>

**27. What happens when a Virtual Warehouse is manually suspended?**  
- A. It remains idle and consumes credits.  
- B. It stops consuming credits.  
- C. It disables queries from being run.  
- D. It scales down automatically.  
<details><summary>Answer</summary> B</details>

**28. What feature ensures queries are distributed evenly across active compute clusters in a Multi-Cluster Warehouse?**  
- A. Load Balancing  
- B. Auto-Resume  
- C. Scale Up  
- D. Auto-Suspend  
<details><summary>Answer</summary> A</details>

**29. Which statement is true about Multi-Cluster Warehouses?**  
- A. They are available in all Snowflake editions.  
- B. They require manual cluster creation.  
- C. They are limited to a maximum of 10 clusters.  
- D. They automatically resize clusters for storage optimization.  
<details><summary>Answer</summary> C</details>

**30. How can one set a Virtual Warehouse to be created in a suspended state using SQL?**  
- A. Use the `SUSPENDED = TRUE` parameter.  
- B. Use the `INITIALLY_SUSPENDED = TRUE` parameter.  
- C. Create the warehouse and immediately suspend it.  
- D. Use the `CREATE_SUSPENDED = TRUE` command.  
<details><summary>Answer</summary> B</details>

**31. What is the primary purpose of a Virtual Warehouse in Snowflake?**  
- A. Store data  
- B. Perform compute operations  
- C. Manage user roles  
- D. Enable data replication  
<details><summary>Answer</summary> B</details>

**32. Which is the smallest size available for a Virtual Warehouse in Snowflake?**  
- A. XS  
- B. S  
- C. M  
- D. L  
<details><summary>Answer</summary> A</details>

**33. What is the minimum auto-suspend setting for a Virtual Warehouse configured using SQL?**  
- A. 1 second  
- B. 60 seconds  
- C. 5 minutes  
- D. 10 minutes  
<details><summary>Answer</summary> B</details>

**34. Which of the following editions supports Multi-Cluster Warehouses?**  
- A. Standard Edition  
- B. Enterprise Edition  
- C. Developer Edition  
- D. Business Critical Edition  
<details><summary>Answer</summary> B, D</details>

**35. What determines the compute cost of a Virtual Warehouse?**  
- A. Storage size used  
- B. Number of queries executed  
- C. Virtual Warehouse size and runtime  
- D. Time Travel duration  
<details><summary>Answer</summary> C</details>

**36. How does Snowflake handle queries during a warehouse scale-up operation?**  
- A. Queries are terminated and restarted.  
- B. Running queries continue at the old size.  
- C. Running queries automatically resize.  
- D. Queries are placed in a queue.  
<details><summary>Answer</summary> B</details>

**37. What is the default behavior for auto-resume when creating a Virtual Warehouse?**  
- A. Disabled  
- B. Enabled  
- C. Configurable during creation  
- D. Enabled only for Multi-Cluster Warehouses  
<details><summary>Answer</summary> B</details>

**38. How does the Standard Scaling Policy for Multi-Cluster Warehouses operate?**  
- A. Adds clusters only after workload queues for 10 minutes.  
- B. Starts clusters if a query is queued or expected to queue.  
- C. Balances load without adding clusters.  
- D. Stops unused clusters immediately.  
<details><summary>Answer</summary> B</details>

**39. What is the minimum runtime billed each time a Virtual Warehouse starts?**  
- A. 1 second  
- B. 1 minute  
- C. 5 minutes  
- D. 10 minutes  
<details><summary>Answer</summary> B</details>

**40. Which of the following actions can you perform on a running Virtual Warehouse?**  
- A. Resize it  
- B. Suspend it  
- C. Delete it  
- D. Scale it out  
<details><summary>Answer</summary> A, B</details>
