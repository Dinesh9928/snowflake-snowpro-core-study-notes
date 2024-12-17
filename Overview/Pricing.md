# Pricing #

Snowflake pricing and cost are based on the actual usage. You pay for what you use and scale storage and compute independently.

## Storage Costs ##
All customers are charged a flat monthly fee per Terabyte for the data they store in Snowflake. Storage cost is measured using the [average amount of storage used per month, after compression](https://www.snowflake.com/en/data-cloud/pricing-options/), for all customer data stored in Snowflake.

The storage includes:
* Data stored in tables, even temporary ones
* Historical data for Time Travel 
* Historical data for Fail-Safe 
* Internal Stages

## Compute Costs ##
Customers pay for virtual warehouses using Snowflake credits. The cost of these credits depends on the Snowflake edition that you are using, and the cloud region of the Snowflake account, see https://www.snowflake.com/pricing/. For example, the costs per credit for an account in US East (North Virginia) on AWS were:
* Standard Edition: $2.00 per credit
* Enterprise Edition: $3.00 per credit
* Business Critical Edition: $4.00 per credit

The number of credits that a virtual warehouse consumes is determined by the following:
* The warehouse size
* The number of clusters (for multi-cluster warehouses)
* The time each server in each cluster runs
  * Server runtime is billed by second
  * Server runtime is billed WITH A ONE-MINUTE MINIMUM

![](../images/VirtualWarehousesCreditsPerHour.png)

## Cloud Services Costs ##
Customers pay for cloud services using Snowflake credits. The typical utilization of cloud services (up to 10% of daily compute credits) is included for free, meaning most customers will not see incremental charges for cloud service usage.

## Data Transfer Costs ##
Customers who wish to move or copy their data between regions or cloud providers will incur data transfer charges. Features such as External Tables, External Functions, and Data Lake Export may incur data transfer charges.

## Purchasing the Snowflake Service ## 
* On-Demand: Billed in monthly arrears, customers are charged a fixed rate only for the amount of services consumed
* Pre-paid: Pre-purchase Capacity, which involves a commitment to Snowflake. The Capacity purchased is then consumed monthly, providing lower prices and a long-term price guarantee, among other advantages.

![](../images/OnDemandVsPrePurchaseCapacityForStorage.webp)

---
### 1. How is Snowflake's pricing model structured?
A. Fixed monthly fee based on usage tier  
B. Per-minute usage billing  
C. Pay-as-you-go, based on actual usage  
D. Annual subscription only  
<details><summary>Answer</summary> C. Pay-as-you-go, based on actual usage</details>

### 2. What is included in Snowflake's storage costs?
A. Only data stored in permanent tables  
B. Data stored in internal stages, Time Travel, and Fail-Safe  
C. Only data stored in external tables  
D. Only real-time streaming data  
<details><summary>Answer</summary> B. Data stored in internal stages, Time Travel, and Fail-Safe</details>

### 3. How is Snowflake storage cost calculated?
A. Based on peak storage usage per day  
B. Based on the average storage used per month after compression  
C. Flat rate based on storage capacity purchased  
D. By the number of rows stored  
<details><summary>Answer</summary> B. Based on the average storage used per month after compression</details>

### 4. What factor determines the number of credits a Snowflake virtual warehouse consumes?
A. The total number of users accessing it  
B. The size of the warehouse and runtime per server  
C. The number of databases connected to it  
D. The number of queries executed  
<details><summary>Answer</summary> B. The size of the warehouse and runtime per server</details>

### 5. How is server runtime billed for a Snowflake virtual warehouse?
A. By the hour  
B. By the second, with a one-minute minimum  
C. By the query  
D. By the day  
<details><summary>Answer</summary> B. By the second, with a one-minute minimum</details>

### 6. Which Snowflake edition charges $3.00 per credit in the US East (North Virginia) region on AWS?
A. Standard Edition  
B. Enterprise Edition  
C. Business Critical Edition  
D. Virtual Edition  
<details><summary>Answer</summary> B. Enterprise Edition</details>

### 7. What is the main difference between On-Demand and Pre-paid Snowflake services?
A. Pre-paid has no storage charges  
B. On-Demand provides a long-term price guarantee  
C. On-Demand is billed in monthly arrears, while Pre-paid offers discounted rates  
D. Pre-paid is billed based on hourly usage  
<details><summary>Answer</summary> C. On-Demand is billed in monthly arrears, while Pre-paid offers discounted rates</details>

### 8. What percentage of daily compute credits is typically included for free cloud services?
A. 1%  
B. 5%  
C. 10%  
D. 15%  
<details><summary>Answer</summary> C. 10%</details>

### 9. Which of the following may incur data transfer charges in Snowflake?
A. External Functions  
B. Creating views within the same database  
C. Querying internal tables  
D. Using cached data  
<details><summary>Answer</summary> A. External Functions</details>

### 10. What is NOT a factor affecting compute costs in Snowflake?
A. Virtual warehouse size  
B. Cloud provider region  
C. Number of users querying data  
D. Edition of Snowflake  
<details><summary>Answer</summary> C. Number of users querying data</details>

### 11. What happens if a customer exceeds the 10% free cloud service utilization?
A. They are billed additional credits  
B. The account is locked  
C. Compute resources are paused  
D. Data transfer is halted  
<details><summary>Answer</summary> A. They are billed additional credits</details>

### 12. Which Snowflake feature is priced based on regional data transfer charges?
A. Time Travel  
B. Virtual Warehouses  
C. Data Lake Export  
D. Schema Cloning  
<details><summary>Answer</summary> C. Data Lake Export</details>

### 13. Snowflake storage charges are determined by:
A. The number of queries executed  
B. Average monthly storage after compression  
C. The number of databases and schemas created  
D. Peak monthly storage before compression  
<details><summary>Answer</summary> B. Average monthly storage after compression</details>

### 14. Which of the following is a benefit of pre-purchasing Snowflake capacity?
A. Higher per-credit cost  
B. Guaranteed availability of compute resources  
C. Lower prices and long-term price guarantee  
D. Exempt from data transfer costs  
<details><summary>Answer</summary> C. Lower prices and long-term price guarantee</details>

### 15. Which of the following is included in Snowflake’s storage cost?
A. Database queries  
B. External stages  
C. Fail-Safe historical data  
D. Virtual warehouses  
<details><summary>Answer</summary> C. Fail-Safe historical data</details>

### 16. What does Snowflake charge customers for when copying data between cloud providers?
A. Compute only  
B. Storage only  
C. Data transfer  
D. Metadata storage  
<details><summary>Answer</summary> C. Data transfer</details>

### 17. How are cloud service costs typically handled in Snowflake?
A. Always billed separately  
B. Included for free up to a certain limit  
C. Charged per terabyte used  
D. Based on the number of active users  
<details><summary>Answer</summary> B. Included for free up to a certain limit</details>

### 18. What does Snowflake charge for in addition to compute and storage?
A. Warehouse setup fees  
B. Cloud service utilization beyond a threshold  
C. Monthly user access licenses  
D. Temporary table creation  
<details><summary>Answer</summary> B. Cloud service utilization beyond a threshold</details>

### 19. Which Snowflake pricing model is suitable for customers needing predictable long-term costs?
A. On-Demand  
B. Reserved  
C. Pre-paid capacity  
D. Spot Instance  
<details><summary>Answer</summary> C. Pre-paid capacity</details>

### 20. What affects the cost of a virtual warehouse in Snowflake?
A. Number of schemas accessed  
B. Runtime per server  
C. Number of databases connected  
D. Query complexity  
<details><summary>Answer</summary> B. Runtime per server</details>

---
### 21. How does Snowflake bill for virtual warehouse runtime?
A. By second with a 1-hour minimum  
B. By second with a 1-minute minimum  
C. By minute with a 5-minute minimum  
D. By minute with a 10-minute minimum  
<details><summary>Answer</summary> B. By second with a 1-minute minimum</details>

### 22. What type of charges can be incurred when moving data between Snowflake regions?
A. Metadata export fees  
B. Data transfer charges  
C. Virtual warehouse fees  
D. Schema migration costs  
<details><summary>Answer</summary> B. Data transfer charges</details>

### 23. What is the unit of storage measurement used by Snowflake for billing?
A. Peak monthly storage  
B. Daily data size  
C. Average monthly storage after compression  
D. Uncompressed data size  
<details><summary>Answer</summary> C. Average monthly storage after compression</details>

### 24. Which Snowflake edition is likely to have the lowest cost per credit?
A. Business Critical  
B. Standard  
C. Enterprise  
D. Reader Edition  
<details><summary>Answer</summary> B. Standard</details>

### 25. Which of the following determines compute costs in Snowflake?  
A. Number of users accessing the warehouse  
B. Virtual warehouse size and server runtime  
C. Number of storage layers  
D. Replication count  
<details><summary>Answer</summary> B. Virtual warehouse size and server runtime</details>

### 26. What are Snowflake customers charged for in terms of storage?
A. Queries executed on the database  
B. Data in temporary and internal stages  
C. Number of users accessing the database  
D. Compute credits used for data export  
<details><summary>Answer</summary> B. Data in temporary and internal stages</details>

### 27. What is the main advantage of pre-purchasing Snowflake capacity?
A. Higher prices but no commitments  
B. Lower prices and long-term price stability  
C. Free data transfer across regions  
D. Free warehouse creation  
<details><summary>Answer</summary> B. Lower prices and long-term price stability</details>

### 28. Snowflake Cloud Services costs are typically:
A. Fixed monthly fees regardless of usage  
B. Included for free up to 10% of compute credits used  
C. Billed separately on a daily basis  
D. Based only on the number of queries run  
<details><summary>Answer</summary> B. Included for free up to 10% of compute credits used</details>

### 29. Which Snowflake feature allows customers to reduce storage charges?
A. External Functions  
B. Data Compression  
C. Schema Cloning  
D. Automatic Data Backup  
<details><summary>Answer</summary> B. Data Compression</details>

### 30. Which Snowflake pricing model is best for customers needing flexibility with no long-term commitment?
A. Reserved Capacity  
B. On-Demand  
C. Pre-paid Contract  
D. Annual Subscription  
<details><summary>Answer</summary> B. On-Demand</details>

---
### 31. If a virtual warehouse of size `X-Large` (16 credits/hour) runs for 2.5 hours, how many Snowflake credits will it consume?  
A. 16 credits  
B. 32 credits  
C. 40 credits  
D. 42 credits  
<details><summary>Answer</summary> C. 40 credits (16 credits/hour * 2.5 hours)</details>

---

### 32. A customer stored an average of 10 TB of data in Snowflake for a month. If the storage cost is $23 per TB per month, what is the total monthly storage cost?  
A. $100  
B. $230  
C. $300  
D. $250  
<details><summary>Answer</summary> B. $230 (10 TB * $23)</details>

---

### 33. A `Medium` warehouse (4 credits/hour) runs for 3 hours. How many credits are consumed?  
A. 10 credits  
B. 12 credits  
C. 14 credits  
D. 16 credits  
<details><summary>Answer</summary> B. 12 credits (4 credits/hour * 3 hours)</details>

---

### 34. Calculate the monthly cost for running a `Small` warehouse (2 credits/hour) for 8 hours daily over 30 days. Assume a cost of $3 per credit.  
A. $1,440  
B. $1,560  
C. $1,400  
D. $1,500  
<details><summary>Answer</summary> A. $1,440 (2 credits/hour * 8 hours/day * 30 days * $3/credit)</details>

---

### 35. If a `Large` warehouse (8 credits/hour) runs for 45 minutes, how many credits are consumed?  
A. 4 credits  
B. 6 credits  
C. 8 credits  
D. 10 credits  
<details><summary>Answer</summary> A. 4 credits (8 credits/hour * 0.75 hours)</details>

---

### 36. A customer has a pre-paid contract of 1,000 credits per month. If they use 1,200 credits in a month, how many credits are charged at on-demand rates?  
A. 100 credits  
B. 200 credits  
C. 300 credits  
D. 400 credits  
<details><summary>Answer</summary> B. 200 credits (1,200 - 1,000 pre-paid)</details>

---

### 37. If the data transfer cost between Snowflake regions is $0.05 per GB and a customer transfers 500 GB of data, what is the total cost?  
A. $20  
B. $25  
C. $30  
D. $35  
<details><summary>Answer</summary> B. $25 (500 GB * $0.05)</details>

---

### 38. A customer stored 15 TB of data, and 20% of it is for Time Travel. If storage costs $22 per TB, what is the cost of Time Travel data storage?  
A. $55  
B. $60  
C. $66  
D. $70  
<details><summary>Answer</summary> C. $66 (15 TB * 0.2 * $22)</details>

---

### 39. A customer uses a `Multi-cluster Warehouse` with 3 clusters, each of size `Large` (8 credits/hour). If all clusters run for 2 hours, how many total credits are consumed?  
A. 32 credits  
B. 36 credits  
C. 48 credits  
D. 60 credits  
<details><summary>Answer</summary> C. 48 credits (8 credits/hour * 3 clusters * 2 hours)</details>

---

### 40. If a customer consumes 1,500 compute credits in a month and cloud services are billed at 5% of compute usage, how many credits are used for cloud services?  
A. 60 credits  
B. 70 credits  
C. 75 credits  
D. 80 credits  
<details><summary>Answer</summary> C. 75 credits (1,500 credits * 0.05)</details>

## MSQ:

### 1. Which of the following statements about Snowflake storage costs are correct?  
- A. Storage costs are based on the average amount of storage used per month.  
- B. Historical data for Time Travel is included in storage costs.  
- C. Customers are billed hourly for storage.  
- D. Storage is charged per Terabyte after compression.  
<details><summary>Answer</summary> A, B, D</details>

---

### 2. What factors affect compute costs in Snowflake?  
- A. Virtual warehouse size  
- B. Duration of warehouse runtime  
- C. Cloud services region  
- D. Number of databases in use  
<details><summary>Answer</summary> A, B, C</details>

---

### 3. Which of the following statements about Time Travel in Snowflake are correct?  
- A. It is charged separately from storage.  
- B. It allows access to historical data within a specific retention period.  
- C. It is not available for external tables.  
- D. It is available for all Snowflake editions.  
<details><summary>Answer</summary> B, C</details>

---

### 4. What are the benefits of pre-purchasing Snowflake Capacity?  
- A. Long-term price guarantee  
- B. Lower overall prices  
- C. Increased virtual warehouse speed  
- D. No additional cloud service costs  
<details><summary>Answer</summary> A, B</details>

---

### 5. Which factors contribute to Snowflake data transfer costs?  
- A. Movement of data between regions  
- B. Copying data between cloud providers  
- C. Data replication within the same region  
- D. Use of External Tables and Data Lake Export  
<details><summary>Answer</summary> A, B, D</details>

---

### 6. Which of the following are correct regarding virtual warehouse billing?  
- A. Billing is based on credits per second.  
- B. Billing includes a minimum of one minute of runtime.  
- C. Billing stops immediately after the warehouse is suspended.  
- D. The number of clusters affects total credit consumption.  
<details><summary>Answer</summary> B, D</details>

---

### 7. Which statements are correct about Snowflake editions and their impact on costs?  
- A. Enterprise Edition costs more per credit than Standard Edition.  
- B. Business Critical Edition has the highest credit cost.  
- C. Edition does not impact storage costs.  
- D. All editions have the same Time Travel retention policy.  
<details><summary>Answer</summary> A, B</details>

---

### 8. What are some characteristics of Snowflake’s on-demand pricing?  
- A. Customers are billed monthly in arrears.  
- B. Customers commit to a fixed capacity for a year.  
- C. Customers are charged a fixed rate only for the services they consume.  
- D. On-demand pricing offers a long-term price guarantee.  
<details><summary>Answer</summary> A, C</details>

---

### 9. What determines the credit consumption of a multi-cluster warehouse?  
- A. Number of clusters in use  
- B. Size of each cluster  
- C. Amount of data processed  
- D. Time each cluster is running  
<details><summary>Answer</summary> A, B, D</details>

---

### 10. Which of the following are true about Snowflake cloud services charges?  
- A. Cloud services are billed separately from compute.  
- B. Up to 10% of daily compute credits are included for free.  
- C. Cloud services are billed based on the number of users.  
- D. Most customers do not see incremental cloud service charges.  
<details><summary>Answer</summary> B, D</details>

---

### 11. Which statements about data replication and transfer costs in Snowflake are true?  
- A. Replicating data between different cloud providers incurs costs.  
- B. Transferring data between Snowflake accounts in the same region is free.  
- C. Data transfer costs are included in pre-purchased capacity.  
- D. External functions may incur data transfer costs.  
<details><summary>Answer</summary> A, B, D</details>

---

### 12. Which features of Snowflake compute usage can impact costs?  
- A. Warehouse auto-suspend time  
- B. Multi-cluster warehouse scaling  
- C. Query optimization level  
- D. Running concurrent queries  
<details><summary>Answer</summary> A, B, D</details>

---

### 13. What are the correct ways to reduce Snowflake compute costs?  
- A. Use a smaller warehouse size for smaller workloads.  
- B. Disable Time Travel for transient tables.  
- C. Use multi-cluster warehouses for every workload.  
- D. Set appropriate auto-suspend intervals.  
<details><summary>Answer</summary> A, B, D</details>

---

### 14. Which costs are typically included in Snowflake pre-purchase agreements?  
- A. Compute credits  
- B. Cloud service credits  
- C. Storage credits  
- D. Data transfer costs  
<details><summary>Answer</summary> A, B, C</details>

---

### 15. Which statements are correct regarding Snowflake credit usage tracking?  
- A. Credit usage can be monitored in real time.  
- B. Only compute credits are tracked.  
- C. Credit usage reports include warehouse utilization.  
- D. Historical credit usage can be exported for analysis.  
<details><summary>Answer</summary> A, C, D</details>

---
### 16. Which factors should you consider when selecting a Snowflake edition to optimize cost?  
- A. Required data retention for Time Travel  
- B. Frequency of multi-cluster warehouse usage  
- C. Cloud provider pricing differences  
- D. Total number of accounts in the organization  
<details><summary>Answer</summary> A, B, C</details>

---

### 17. What strategies can reduce data transfer costs in Snowflake?  
- A. Use External Tables within the same region.  
- B. Avoid cross-region replication unless necessary.  
- C. Use CTAS (Create Table As Select) to copy data between regions.  
- D. Limit external data exports.  
<details><summary>Answer</summary> A, B, D</details>

---

### 18. Which of the following are correct statements about Snowflake’s auto-scaling feature?  
- A. Auto-scaling increases the number of clusters based on query load.  
- B. Auto-scaling is available only for Enterprise Edition.  
- C. It automatically suspends idle clusters to reduce costs.  
- D. Auto-scaling improves query concurrency.  
<details><summary>Answer</summary> A, C, D</details>

---

### 19. What components are billed under Snowflake's cloud services?  
- A. Metadata management  
- B. Query parsing and optimization  
- C. Virtual warehouse creation  
- D. Data encryption and decryption  
<details><summary>Answer</summary> A, B, D</details>

---

### 20. What are common use cases for pre-purchasing Snowflake capacity?  
- A. Workloads with predictable compute requirements  
- B. Environments with variable, on-demand workloads  
- C. Long-term projects with consistent storage needs  
- D. Seasonal data processing peaks  
<details><summary>Answer</summary> A, C, D</details>

## True/False

### 1. Snowflake charges for virtual warehouses based on the size and duration of their usage.  
<details><summary>Answer</summary> True</details>

---

### 2. Storage costs in Snowflake are calculated based on the uncompressed size of the data stored.  
<details><summary>Answer</summary> False</details>

---

### 3. In Snowflake, compute costs are incurred even when a virtual warehouse is suspended.  
<details><summary>Answer</summary> False</details>

---

### 4. Pre-purchasing Snowflake capacity offers lower prices compared to on-demand pricing.  
<details><summary>Answer</summary> True</details>

---

### 5. Cloud services usage is free for up to 10% of daily compute credit utilization.  
<details><summary>Answer</summary> True</details>

---

### 6. Data transfer between different Snowflake accounts within the same region is free.  
<details><summary>Answer</summary> True</details>

---

### 7. Snowflake’s Time Travel feature is included in the compute cost of virtual warehouses.  
<details><summary>Answer</summary> False</details>

---

### 8. Snowflake provides a flat storage rate across all cloud providers and regions.  
<details><summary>Answer</summary> False</details>

---

### 9. Virtual warehouse runtime is billed in one-minute increments, with a one-minute minimum.  
<details><summary>Answer</summary> True</details>

---

### 10. External Tables in Snowflake do not incur any additional data transfer charges.  
<details><summary>Answer</summary> False</details>

---

### 11. Pre-paid capacity in Snowflake cannot be used across multiple cloud regions.  
<details><summary>Answer</summary> False</details>

---

### 12. Snowflake credits are the currency used for both compute and cloud service costs.  
<details><summary>Answer</summary> True</details>

---

### 13. Snowflake charges customers based on the maximum amount of storage used in a month.  
<details><summary>Answer</summary> False</details>

---

### 14. Larger virtual warehouses consume more credits per hour than smaller ones.  
<details><summary>Answer</summary> True</details>

---

### 15. Multi-cluster warehouses in Snowflake can help manage query concurrency.  
<details><summary>Answer</summary> True</details>

---

### 16. Time Travel data in Snowflake is billed at the same storage rate as regular table data.  
<details><summary>Answer</summary> True</details>

---

### 17. Customers are charged separately for metadata operations in Snowflake.  
<details><summary>Answer</summary> False</details>

---

### 18. On-demand Snowflake pricing requires upfront payment.  
<details><summary>Answer</summary> False</details>

---

### 19. Data transfer costs are incurred when replicating data across different cloud regions.  
<details><summary>Answer</summary> True</details>

---

### 20. Snowflake Reader Accounts allow providers to manage the compute costs for their consumers.  
<details><summary>Answer</summary> True</details>

