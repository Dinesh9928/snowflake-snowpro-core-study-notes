# Data Sharing #

> [Introduction to Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro.html)

Secure Data Sharing lets you share selected objects in a database in your (Provider) account with other (Consumer) Snowflake accounts. You can share the following Snowflake database objects:
* Tables 
* External tables 
* Secure views 
* Secure materialized views 
* Secure UDFs

## Shares ##

Shares are named Snowflake objects that encapsulate all of the information required to share database objects. Using `GRANT`s, shares encapsulate:
* the privileges that grant access to the database, schemas and specific objects we want to share
* the  consumer accounts that have access to the share and all  objects the share has access to.

Some sample SQL:
```postgres-psql
-- Create a Share and give SELECT privileges to table
CREATE SHARE myShare;
GRANT USAGE ON DATABASE myDb TO SHARE myShare;
GRANT USAGE ON SCHEMA myDb.public TO SHARE myShare;
GRANT SELECT ON TABLE myDb.public.myTable TO SHARE myShare;
-- Add consumer accounts to the share
ALTER SHARE myShare ADD ACCOUNTS = org1.consumer1,org1.consumer2;

-- Show all Shares in the account
SHOW SHARES;

-- See all the privileges a Share has
SHOW GRANTS TO SHARE myShare;

-- See the accounts (consumers) that have access to the Share
SHOW GRANTS OF SHARE myShare;
```

### Provider ###
* Shares data with others
* No cost to share other than the storage of the shared data
* Has full control over the data shared and the privileges they give to the share
* Can revoke privileges granted to the share at any time 
* Pays for the storage of the data they share

### Consumer ###
Accounts that receive the share/data.
* Shared data does not take up any storage in the Consumer account so Consumers don’t pay for its storage
* Time Travel is not available to Consumers on shared data
* Zero-Copy Cloning is not available to Consumers on shared data
* Consumers can, however, copy share data using CTAS (`CREATE TABLE ... AS SELECT`)

#### Full Account Consumer ####
* The only charges to consumers are for the compute resources (i.e. Virtual Warehouses) used to query the shared data

#### Reader Account Consumer ####
* Consumers who are not Snowflake customers and do not have a Snowflake account
* The Reader account is set up within the Provider's environment
  * Providers manage all aspects of the Reader Account - users, roles, permissions, warehouses, etc.
  * A reader account can only consume data from the provider account that created it
* Readers must use Warehouses from the Provider's account; the Provider incurs the compute cost
* Providers can track and manage the queries Readers can run and can monetize them by charging for the data share or the queries run

### Sharing Features ###
* All database objects shared between accounts are `READ-ONLY`,(i.e. the objects cannot be modified or deleted, including adding or modifying table data
* Data is shared live:
  * No data movement or copying
  * Consumers immediately see all updates
  * Data can be shared with an unlimited number of Consumers
* Shares can only be created by users
  * with the `ACCOUNTADMIN` role
  * or with a role that has been explicitly given the permission to create Shares
* Shares can be created between accounts but not within the same account
* Consumers cannot re-share a Share
* Snowflake does not charge for creating shares although Providers can charge Consumers or Reader Accounts to monetize their data
* Sharing is available across all [Snowflake Editions](Editions.md)
* The Consumer and Provider accounts must be in the same cloud and region to share data
  * A Provider can replicate their data to an account in another cloud and region if they want to share it with a Consumer in the other cloud/region.

### Sharing with Secure Views ###
The preferred way to share data is by using Secure Views:
* allows sharing only select rows and columns rather than entire tables
* allows providing different sets to different Consumers by filtering the data based on the values of the context functions `CURRENT_ACCOUNT()` and `CURRENT_ROLE()` which would return the Consumer account name and role, respectively.

## Types ##
* Public Data Marketplace
* Private Data Exchange

### **Multiple-Choice Questions (MCQs)**

**1. Which of the following objects can be shared using Secure Data Sharing in Snowflake?**  
- A. Tables  
- B. Views  
- C. Secure UDFs  
- D. Materialized views  

<details>**Answer** - A, B, C, D</details>

---

**2. Who can create a Share in Snowflake?**  
- A. ACCOUNTADMIN  
- B. SYSADMIN  
- C. Any user with CREATE SHARE privilege  
- D. Users with `ORGADMIN` role  

<details>**Answer** - A, C</details>

---

**3. Which Snowflake role is responsible for managing a Share in a Provider account?**  
- A. SHAREADMIN  
- B. ACCOUNTADMIN  
- C. SYSADMIN  
- D. PROVIDERADMIN  

<details>**Answer** - B. ACCOUNTADMIN</details>

---

**4. How is data shared between Snowflake accounts?**  
- A. By copying the data  
- B. Through virtual data replication  
- C. By granting access to tables and views  
- D. Using external stages  

<details>**Answer** - C. By granting access to tables and views</details>

---

**5. What is the main advantage of using Secure Views when sharing data?**  
- A. They allow sharing entire tables  
- B. They can filter data based on context functions  
- C. They enable Zero-Copy Cloning  
- D. They provide Time Travel support  

<details>**Answer** - B. They can filter data based on context functions</details>

---

**6. What is NOT true about data sharing in Snowflake?**  
- A. Data shared between accounts is read-only  
- B. Consumers can modify shared data  
- C. Consumers cannot perform Zero-Copy Cloning  
- D. Data is immediately available to Consumers  

<details>**Answer** - B. Consumers can modify shared data</details>

---

**7. In a Snowflake Data Share, who pays for the storage of shared data?**  
- A. Provider  
- B. Consumer  
- C. Both Provider and Consumer  
- D. Snowflake  

<details>**Answer** - A. Provider</details>

---

**8. Which of the following is a restriction for Consumer accounts receiving shared data?**  
- A. They can modify shared data  
- B. They must be in the same cloud and region as the Provider  
- C. They can use their own Virtual Warehouses to query the shared data  
- D. They can access the data indefinitely  

<details>**Answer** - B. They must be in the same cloud and region as the Provider</details>

---

**9. Which of the following is true about Reader Accounts in Snowflake?**  
- A. They allow external users to consume shared data without a Snowflake account  
- B. They can modify the shared data  
- C. They pay for the storage of shared data  
- D. They are created by the Consumer organization  

<details>**Answer** - A. They allow external users to consume shared data without a Snowflake account</details>

---

**10. Which of the following types of data can be shared using Secure Data Sharing?**  
- A. Internal data tables  
- B. External tables  
- C. Secure views  
- D. Temporary tables  

<details>**Answer** - A, B, C</details>

---
### 1. What is the primary function of Secure Data Sharing in Snowflake?
A. To transfer data to external storage
B. To allow the sharing of Snowflake database objects between accounts
C. To replicate data between databases
D. To create clones of tables in different accounts
<details><summary>Answer</summary> B. To allow the sharing of Snowflake database objects between accounts</details>

### 2. Which of the following is NOT an object that can be shared in Snowflake via Secure Data Sharing?
A. Tables  
B. Views  
C. Secure UDFs  
D. File Formats  
<details><summary>Answer</summary> D. File Formats</details>

### 3. Which role is required to create a Share in Snowflake?
A. SYSADMIN  
B. ACCOUNTADMIN  
C. SECURITYADMIN  
D. DBOWNER  
<details><summary>Answer</summary> B. ACCOUNTADMIN</details>

### 4. What is a key characteristic of shared data for Consumers?
A. The data can be modified by Consumers  
B. Consumers can add new tables to the shared data  
C. Shared data is read-only  
D. Data is copied to the Consumer account  
<details><summary>Answer</summary> C. Shared data is read-only</details>

### 5. Which Snowflake role has the ability to grant privileges to a Share?
A. ORGADMIN  
B. ACCOUNTADMIN  
C. SYSADMIN  
D. SECURITYADMIN  
<details><summary>Answer</summary> B. ACCOUNTADMIN</details>

### 6. How does the sharing model in Snowflake handle data storage for the Consumer?
A. Consumers incur storage costs for shared data  
B. Consumers do not incur storage costs for shared data  
C. Data is replicated to the Consumer's account  
D. Consumers have to manually download the data  
<details><summary>Answer</summary> B. Consumers do not incur storage costs for shared data</details>

### 7. Which of the following can be used to filter data in a Secure View when sharing it with Consumers?
A. `CURRENT_ACCOUNT()`  
B. `CURRENT_ROLE()`  
C. `TIME_TRAVEL()`  
D. Both A and B  
<details><summary>Answer</summary> D. Both A and B</details>

### 8. Can Consumers re-share data that they have received via a Share?
A. Yes, Consumers can re-share the data  
B. No, Consumers cannot re-share the data  
C. Only Reader accounts can re-share the data  
D. Only if they have the `ADMIN` role  
<details><summary>Answer</summary> B. No, Consumers cannot re-share the data</details>

### 9. What happens when data is shared with a Reader Account?
A. The Reader Account can modify the shared data  
B. The Reader Account uses the Provider’s warehouses for compute  
C. The Reader Account stores the data in its own account  
D. The Reader Account must manually sync data  
<details><summary>Answer</summary> B. The Reader Account uses the Provider’s warehouses for compute</details>

### 10. Which of the following is a valid SQL command to create a Share in Snowflake?
A. `CREATE SHARE myShare;`  
B. `GRANT SHARE myShare;`  
C. `CREATE DATA SHARE myShare;`  
D. `SHARE CREATE myShare;`  
<details><summary>Answer</summary> A. `CREATE SHARE myShare;`</details>

### 11. Which role is responsible for managing a Reader Account in Snowflake?
A. ACCOUNTADMIN  
B. SECURITYADMIN  
C. SYSADMIN  
D. PROVIDERADMIN  
<details><summary>Answer</summary> D. PROVIDERADMIN</details>

### 12. A Consumer Account can access data from a Share without using a Snowflake warehouse.
A. True  
B. False  
<details><summary>Answer</summary> B. False</details>

### 13. Snowflake shares data using a technique called:
A. Data Replication  
B. Data Cloning  
C. Data Movement  
D. Zero-Copy Cloning  
<details><summary>Answer</summary> D. Zero-Copy Cloning</details>

### 14. Which of the following is a valid restriction of data shared with a Reader Account?
A. It can only use the Provider’s compute resources  
B. It has full access to the shared data  
C. It can store the shared data locally  
D. It has the ability to modify the shared data  
<details><summary>Answer</summary> A. It can only use the Provider’s compute resources</details>

### 15. Which of the following statements about Shares in Snowflake is true?
A. Consumers can modify shared data  
B. Shares can be created between accounts in different regions  
C. Data sharing requires replication of data to the Consumer account  
D. Providers pay for compute resources used by the Consumer  
<details><summary>Answer</summary> B. Shares can be created between accounts in different regions</details>

### 16. What SQL statement is used to add consumers to a Share?
A. `ALTER SHARE myShare ADD CONSUMERS = consumer1, consumer2;`  
B. `CREATE SHARE myShare CONSUMERS = consumer1, consumer2;`  
C. `GRANT SHARE TO CONSUMERS;`  
D. `SHOW GRANTS TO SHARE myShare;`  
<details><summary>Answer</summary> A. `ALTER SHARE myShare ADD CONSUMERS = consumer1, consumer2;`</details>

### 17. Can a Consumer use a separate virtual warehouse to query shared data in Snowflake?
A. Yes, Consumers can use their own warehouse  
B. No, they must use the Provider’s warehouse  
C. Yes, but only for external tables  
D. No, they must create a replica of the data  
<details><summary>Answer</summary> B. No, they must use the Provider’s warehouse</details>

### 18. Which of the following is NOT a limitation for a Consumer account in Snowflake when using shared data?
A. Data cannot be copied to the Consumer account  
B. Time Travel is available for shared data  
C. Zero-Copy Cloning is not available for shared data  
D. Consumers cannot modify shared data  
<details><summary>Answer</summary> B. Time Travel is available for shared data</details>

### 19. When sharing data with a Consumer, what is NOT allowed by default?
A. Modifying shared data  
B. Querying the shared data  
C. Granting additional access to other accounts  
D. Using external stages to access shared data  
<details><summary>Answer</summary> A. Modifying shared data</details>

### 20. Which of the following accounts can use shared data without incurring storage costs?
A. Provider Account  
B. Consumer Account  
C. Reader Account  
D. Both B and C  
<details><summary>Answer</summary> D. Both B and C</details>

...


...


### **Multiple-Select Questions (MSQs)**

**1. What must be done to share data in Snowflake?**  
- A. Grant SELECT on specific objects  
- B. Create a Share object  
- C. Add Consumer accounts to the Share  
- D. Enable Time Travel on shared data  

<details>**Answer** - A, B, C</details>

---

**2. Which roles can manage a Share in the Provider account?**  
- A. ACCOUNTADMIN  
- B. SYSADMIN  
- C. Any user with the CREATE SHARE privilege  
- D. PROVIDERADMIN  

<details>**Answer** - A, C</details>

---

**3. Which of the following are restrictions for Consumer accounts on shared data?**  
- A. Cannot modify shared data  
- B. Cannot use Time Travel on shared data  
- C. Cannot create or modify Secure Views  
- D. Cannot clone shared data  

<details>**Answer** - A, B, D</details>

---

**4. What are some characteristics of Secure Data Sharing in Snowflake?**  
- A. Data is immediately visible to Consumers  
- B. Data can be modified by Consumers  
- C. Zero-Copy Cloning is available to Consumers  
- D. Data is not copied or moved  

<details>**Answer** - A, D</details>

---

**5. What types of accounts can be Consumers in Snowflake?**  
- A. Full Account Consumer  
- B. Reader Account Consumer  
- C. External Account Consumer  
- D. Managed Account Consumer  

<details>**Answer** - A, B</details>

---

**6. Which features are available in Snowflake when using Secure Views for data sharing?**  
- A. Data filtering based on context functions  
- B. Ability to modify data  
- C. Share different sets of data with different Consumers  
- D. Automatic data replication  

<details>**Answer** - A, C</details>

---

**7. Who manages the Reader Account in Snowflake?**  
- A. Consumer  
- B. Provider  
- C. Third-party administrator  
- D. Snowflake  

<details>**Answer** - B. Provider</details>

---

**8. What types of objects can be shared in Snowflake?**  
- A. Tables  
- B. External tables  
- C. Secure views  
- D. Stored procedures  

<details>**Answer** - A, B, C</details>

---

**9. Which of the following is true about sharing data in Snowflake?**  
- A. Providers can monetize shared data  
- B. Data shared in a Share is mutable by Consumers  
- C. Snowflake charges for creating shares  
- D. Consumer accounts can re-share data  

<details>**Answer** - A</details>

---

**10. Which Snowflake editions support data sharing?**  
- A. Standard  
- B. Enterprise  
- C. Business Critical  
- D. All Snowflake Editions  

<details>**Answer** - D. All Snowflake Editions</details>

### 1. Which of the following features are available when using Secure Data Sharing in Snowflake?
- A. Data can be cloned by the Consumer  
- B. Data is read-only for Consumers  
- C. Data is immediately updated for Consumers  
- D. Consumers incur storage costs for shared data  
<details><summary>Answer</summary> B. Data is read-only for Consumers, C. Data is immediately updated for Consumers</details>

### 2. What are the restrictions of using a Reader Account in Snowflake for shared data?
- A. The Reader Account can modify shared data  
- B. The Reader Account must use the Provider’s virtual warehouses  
- C. The Reader Account can only consume data from the Provider account  
- D. The Reader Account must replicate the data locally  
<details><summary>Answer</summary> B. The Reader Account must use the Provider’s virtual warehouses, C. The Reader Account can only consume data from the Provider account</details>

### 3. Which of the following statements are true about the Provider role in Snowflake?
- A. The Provider pays for the compute costs incurred by the Consumer  
- B. The Provider grants the Consumer access to data  
- C. The Provider can revoke privileges granted to the share at any time  
- D. The Provider shares data with consumers in a read-write manner  
<details><summary>Answer</summary> B. The Provider grants the Consumer access to data, C. The Provider can revoke privileges granted to the share at any time</details>

### 4. Which types of data can be shared using Secure Data Sharing in Snowflake?
- A. Tables  
- B. Views  
- C. Streams  
- D. UDFs  
<details><summary>Answer</summary> A. Tables, B. Views, D. UDFs</details>

### 5. What are the advantages of using Secure Views in data sharing?
- A. It allows sharing specific rows and columns based on filters  
- B. It allows modifying the data shared  
- C. It provides better performance for the shared data  
- D. It restricts access to all rows and columns  
<details><summary>Answer</summary> A. It allows sharing specific rows and columns based on filters</details>

...


---

### **True/False Questions**

**1. Consumers can modify shared data in Snowflake.**  
<details>**Answer** - False</details>

---

**2. Snowflake does not charge for creating data shares, but Providers may charge Consumers.**  
<details>**Answer** - True</details>

---

**3. A Provider can re-share data to another account in Snowflake.**  
<details>**Answer** - False</details>

---

**4. Data shared using Snowflake's Secure Data Sharing feature is read-only for Consumers.**  
<details>**Answer** - True</details>

---

**5. A Reader Account in Snowflake can access data from multiple Providers.**  
<details>**Answer** - False</details>

---

**6. Time Travel is available to Consumers on shared data.**  
<details>**Answer** - False</details>

---

**7. Data shared in Snowflake is copied to the Consumer's account.**  
<details>**Answer** - False</details>

---

**8. Providers can track and manage the queries run by Readers in Snowflake.**  
<details>**Answer** - True</details>

---

**9. A Consumer can create a Zero-Copy Clone of shared data in Snowflake.**  
<details>**Answer** - False</details>

---

**10. Shares in Snowflake can only be created between accounts in the same cloud and region.**  
<details>**Answer** - True</details>

---

### 1. Secure Data Sharing allows data to be shared between different accounts in Snowflake.
- True  
- False  
<details><summary>Answer</summary> True</details>

### 2. The Provider pays for the storage of the shared data, while the Consumer incurs the storage cost.
- True  
- False  
<details><summary>Answer</summary> False</details>

### 3. Consumers can modify the data that is shared with them in Snowflake.
- True  
- False  
<details><summary>Answer</summary> False</details>

### 4. Snowflake allows sharing data between accounts within the same organization only.
- True  
- False  
<details><summary>Answer</summary> False</details>

### 5. Data shared through Secure Views can be filtered based on Consumer-specific roles and accounts.
- True  
- False  
<details><summary>Answer</summary> True</details>

...
### 1. Data shared using Secure Data Sharing is immediately available for the Consumer to query.
- True  
- False  
<details><summary>Answer</summary> True</details>

### 2. Providers incur storage costs for the data shared with Consumers.
- True  
- False  
<details><summary>Answer</summary> False</details>

### 3. Secure Data Sharing allows for data to be shared between different accounts in Snowflake.
- True  
- False  
<details><summary>Answer</summary> True</details>

### 4. A Consumer can modify data shared with them by a Provider.
- True  
- False  
<details><summary>Answer</summary> False</details>

### 5. A Reader Account can be used by non-Snowflake users to consume data shared by a Provider.
- True  
- False  
<details><summary>Answer</summary> True</details>

...

