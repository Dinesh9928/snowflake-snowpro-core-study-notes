# Organizations, Accounts, Databases And Schemas #

## Organization ##
Top level securable object which serves as a logical grouping of Accounts.
* The Organization object is not available by default. It can be enabled on request by Snowflake Support.
* Once enabled, it makes available features which make use of multiple accounts such as database replication and failover.
* It can be used for monitoring usage and billing across accounts.
* The `ORGADMIN` role is added to the primary account in the Organization to allow management of the contained Account objects
* By default there is a limit of 25 Accounts in an Organization, can be increased by Snowflake Support

## Account ##
Logical grouping of Databases.
* Each account is hosted on a single cloud provider and in a single geographic region
* By default, Snowflake does not move data between regions unless requested
* The region where the account is provisioned affects:
  * the account and storage price
  * which regulatory certifications you can achieve
  * some Snowflake features which may be region-specific
* The account's name must be unique within an organization, regardless of which Snowflake region the account is in.
* The URL of an account (e.g. `xy12345.us-est-2.aws.snowflakecomputing.com`) consists of
  * the account identifier (sometimes referred to as the account name) `xy12345.us-est-2.aws` which can consists any mixture of the following:
    * the account locator `xy12345`
    * the cloud services region ID `us-est-2` and
    * the cloud provider `aws`
  * and the snowflake domain, `snowflakecomputing.com`
* It is possible to request a vanity address to convert the identifier into your company's name.
* An account is created with the system-defined role `ACCOUNTADMIN`
* When an account is created by an `ORGADMIN`, the account name (`acme-marketing-account`) will contain a unique organization name `acme` and the account name `marketing-account`

## Database ##
Logical container of your data, organized in Schemas.
* A Database can be created as a clone from another Database
* A Database can be created as a replica of another Database
* A Database can be created from a Share object provided by another Account
* Databases can be created as `Transient`
  * Time Travel: 0 or 1 days
  * No Fail-Safe support

## Schema ##
Logical grouping of Tables, Views, Stored Procedures, UDFs, Stages, File Formats, Pipes, Sequences, Shares, etc. Every schema belongs to a single Database. When a database is created, there are two default schemas created in it
* A Schema can be created as a clone from another Schema
* The Database name and Schema name together form the Namespace for all Schema objects
* `public`: default schema for any object created without specifying a schema
* `information_schema`: stores metadata information
* Schemas can be created as `Transient`.
  * Time Travel: 0 or 1 days
  * No Fail-Safe support


### 1. Which of the following serves as a logical grouping of Snowflake accounts?  
- A. Database  
- B. Schema  
- C. Organization  
- D. Warehouse  

<details>**Answer:** C. Organization</details>

---

### 2. What role is assigned to manage the organization in Snowflake?  
- A. ACCOUNTADMIN  
- B. SECURITYADMIN  
- C. ORGADMIN  
- D. SYSADMIN  

<details>**Answer:** C. ORGADMIN</details>

---

### 3. What is the default limit of accounts in an organization in Snowflake?  
- A. 5  
- B. 10  
- C. 25  
- D. 50  

<details>**Answer:** C. 25</details>

---

### 4. Which of the following is true regarding Snowflake accounts?  
- A. Each account can span multiple cloud providers by default.  
- B. Each account is limited to a single cloud provider and geographic region.  
- C. Data can be freely moved between regions without any configuration.  
- D. An account does not require a unique name within an organization.  

<details>**Answer:** B. Each account is limited to a single cloud provider and geographic region.</details>

---

### 5. Which of the following affects Snowflake account pricing?  
- A. The number of schemas created  
- B. The region in which the account is provisioned  
- C. The number of sequences used  
- D. The number of stages in use  

<details>**Answer:** B. The region in which the account is provisioned</details>

---

### 6. Which system-defined role is assigned to a new Snowflake account?  
- A. SYSADMIN  
- B. SECURITYADMIN  
- C. ACCOUNTADMIN  
- D. PUBLIC  

<details>**Answer:** C. ACCOUNTADMIN</details>

---

### 7. What is the default schema created in a database for storing metadata?  
- A. information_schema  
- B. public_schema  
- C. metadata_schema  
- D. default_schema  

<details>**Answer:** A. information_schema</details>

---

### 8. Which of the following Snowflake database types has no Fail-Safe support?  
- A. Permanent Database  
- B. Replica Database  
- C. Transient Database  
- D. Shared Database  

<details>**Answer:** C. Transient Database</details>

---

### 9. Which Snowflake object can be created as a clone of another object?  
- A. Role  
- B. Schema  
- C. Stage  
- D. Sequence  

<details>**Answer:** B. Schema</details>

---

### 10. What is the purpose of the `public` schema in Snowflake?  
- A. To store metadata information  
- B. To act as the default schema for objects created without specifying a schema  
- C. To manage access to external databases  
- D. To configure storage settings  

<details>**Answer:** B. To act as the default schema for objects created without specifying a schema</details>

---
### 11. Which of the following correctly describes the account URL format in Snowflake?  
- A. `<account_name>.<region>.<provider>.com`  
- B. `<account_identifier>.snowflakecomputing.com`  
- C. `<account_locator>.<cloud_region>.<cloud_provider>.snowflakecomputing.com`  
- D. `<account_name>.snowflake.com`  

<details>**Answer:** C. `<account_locator>.<cloud_region>.<cloud_provider>.snowflakecomputing.com`</details>

---

### 12. Which Snowflake object can be created as a replica of another database?  
- A. Stage  
- B. Schema  
- C. Database  
- D. Table  

<details>**Answer:** C. Database</details>

---

### 13. In Snowflake, which namespace combination uniquely identifies schema-level objects?  
- A. Account + Region  
- B. Schema + Table  
- C. Database + Schema  
- D. Table + View  

<details>**Answer:** C. Database + Schema</details>

---

### 14. What is a feature of Snowflake’s `Transient` databases and schemas?  
- A. Unlimited Time Travel  
- B. No Fail-Safe support  
- C. Can only store temporary tables  
- D. Supports real-time streaming  

<details>**Answer:** B. No Fail-Safe support</details>

---

### 15. How many days of Time Travel can be configured for a transient database in Snowflake?  
- A. 1 day  
- B. 3 days  
- C. 7 days  
- D. 14 days  

<details>**Answer:** A. 1 day</details>

---

### 16. Which role in Snowflake is responsible for creating new accounts within an organization?  
- A. ACCOUNTADMIN  
- B. SYSADMIN  
- C. ORGADMIN  
- D. SECURITYADMIN  

<details>**Answer:** C. ORGADMIN</details>

---

### 17. Which schema in a Snowflake database contains metadata information?  
- A. admin_schema  
- B. public_schema  
- C. information_schema  
- D. default_schema  

<details>**Answer:** C. information_schema</details>

---

### 18. Which object in Snowflake is used to generate a unique identifier for multiple sessions?  
- A. Stream  
- B. Sequence  
- C. Stage  
- D. Procedure  

<details>**Answer:** B. Sequence</details>

---

### 19. What does Snowflake use to logically group tables, views, and other objects within a database?  
- A. Organization  
- B. Account  
- C. Schema  
- D. Role  

<details>**Answer:** C. Schema</details>

---

### 20. Which of the following is true about Snowflake database replication?  
- A. It is only available for temporary tables.  
- B. It allows read-only replicas across different accounts.  
- C. Replicas can have different data retention policies.  
- D. It supports write operations on replicas.  

<details>**Answer:** B. It allows read-only replicas across different accounts.</details>

---

### 21. Which of the following roles is automatically created when a Snowflake account is provisioned?  
- A. SYSADMIN  
- B. SECURITYADMIN  
- C. ACCOUNTADMIN  
- D. ORGADMIN  

<details>**Answer:** C. ACCOUNTADMIN</details>

---

### 22. What happens if an account name is duplicated within an organization in Snowflake?  
- A. The second account will be created successfully.  
- B. Snowflake automatically appends a numeric suffix.  
- C. Snowflake prevents account creation.  
- D. The organization name is automatically changed.  

<details>**Answer:** C. Snowflake prevents account creation.</details>

---

### 23. Which schema-level object is used to stage data files before loading them into a table?  
- A. Sequence  
- B. Stage  
- C. Stream  
- D. Table  

<details>**Answer:** B. Stage</details>

---

### 24. What is a feature of a Snowflake schema marked as `Transient`?  
- A. It supports unlimited data retention.  
- B. It has reduced storage costs.  
- C. It supports automatic failover.  
- D. It can contain only external tables.  

<details>**Answer:** B. It has reduced storage costs.</details>

---

### 25. In Snowflake, what is the primary purpose of the `information_schema`?  
- A. To store user-generated data.  
- B. To provide metadata information.  
- C. To manage storage costs.  
- D. To store temporary data only.  

<details>**Answer:** B. To provide metadata information.</details>

---

### 26. What happens when a database is cloned in Snowflake?  
- A. The data is moved to a new region.  
- B. A full copy of the data is created.  
- C. Metadata pointers to the original data are created.  
- D. The original database is deleted.  

<details>**Answer:** C. Metadata pointers to the original data are created.</details>

---

### 27. Which of the following can be stored inside a Snowflake schema?  
- A. Roles  
- B. Tables  
- C. Stages  
- D. Sequences  

<details>**Answer:** B, C, D</details>

---

### 28. How many default schemas are created when a new database is provisioned in Snowflake?  
- A. One  
- B. Two  
- C. Three  
- D. Four  

<details>**Answer:** B. Two</details>

---

### 29. Which of the following statements about Snowflake's `public` schema is correct?  
- A. It can be renamed.  
- B. It is created automatically in every new database.  
- C. It cannot store views.  
- D. It is used only for external tables.  

<details>**Answer:** B. It is created automatically in every new database.</details>

---

### 30. What is a key feature of Snowflake's failover mechanism?  
- A. It is enabled by default for all accounts.  
- B. It allows replication across different cloud providers.  
- C. It requires manual intervention for database replication.  
- D. It ensures read-only database replicas during failover.  

<details>**Answer:** D. It ensures read-only database replicas during failover.</details>

### 31. Which of the following statements is true about Snowflake's `ORGADMIN` role?  
- A. It manages user authentication settings.  
- B. It is automatically available in all Snowflake accounts.  
- C. It is added to the primary account in an organization to manage other accounts.  
- D. It is used only for managing databases.  

<details>**Answer:** C. It is added to the primary account in an organization to manage other accounts.</details>

---

### 32. What is the default storage type for a newly created database in Snowflake?  
- A. Permanent  
- B. Transient  
- C. External  
- D. Temporary  

<details>**Answer:** A. Permanent</details>

---

### 33. Which of the following URL components uniquely identifies a Snowflake account?  
- A. Cloud region ID  
- B. Account locator  
- C. Snowflake domain name  
- D. File format  

<details>**Answer:** B. Account locator</details>

---

### 34. What is the maximum Time Travel period for a transient schema in Snowflake?  
- A. 7 days  
- B. 0 or 1 day  
- C. 30 days  
- D. 90 days  

<details>**Answer:** B. 0 or 1 day</details>

---

### 35. What happens if a database replica in Snowflake becomes unavailable?  
- A. The original database is automatically deleted.  
- B. All connected queries are lost.  
- C. The replica can be used as a failover database.  
- D. Snowflake stops replication entirely.  

<details>**Answer:** C. The replica can be used as a failover database.</details>

---

### 36. How many accounts are allowed in a Snowflake organization by default?  
- A. 5  
- B. 10  
- C. 25  
- D. 50  

<details>**Answer:** C. 25</details>

---

### 37. Which Snowflake feature helps monitor usage and billing across accounts within an organization?  
- A. Information schema  
- B. Snowpipe  
- C. Organization object  
- D. Fail-Safe  

<details>**Answer:** C. Organization object</details>

---

### 38. Which role in Snowflake allows a user to create new databases?  
- A. SYSADMIN  
- B. ACCOUNTADMIN  
- C. SECURITYADMIN  
- D. ORGADMIN  

<details>**Answer:** A. SYSADMIN</details>

---

### 39. Which of the following affects the cost of storage in Snowflake?  
- A. The size of the account name  
- B. The number of roles created  
- C. The geographic region of the account  
- D. The number of schemas in a database  

<details>**Answer:** C. The geographic region of the account</details>

---

### 40. What is a key characteristic of Snowflake's vanity URL?  
- A. It uses a random account locator.  
- B. It can include a custom company name.  
- C. It cannot be customized after creation.  
- D. It increases storage costs.  

<details>**Answer:** B. It can include a custom company name.</details>

### 41. What is the purpose of Snowflake's `information_schema` in a database?  
- A. To store user-generated data  
- B. To store metadata information  
- C. To handle external file storage  
- D. To monitor replication  

<details>**Answer:** B. To store metadata information</details>

---

### 42. Which of the following components is included in a Snowflake account URL?  
- A. Schema name  
- B. Account locator  
- C. Database object  
- D. Table structure  

<details>**Answer:** B. Account locator</details>

---

### 43. What is the namespace in Snowflake defined by?  
- A. Database and table name  
- B. Account and schema name  
- C. Database and schema name  
- D. Cloud provider and region  

<details>**Answer:** C. Database and schema name</details>

---

### 44. Which schema is automatically created with every new Snowflake database?  
- A. default_schema  
- B. user_schema  
- C. information_schema  
- D. admin_schema  

<details>**Answer:** C. information_schema</details>

---

### 45. What is the function of a Snowflake schema object?  
- A. Group multiple organizations  
- B. Store configuration details  
- C. Group database objects like tables and views  
- D. Manage network settings  

<details>**Answer:** C. Group database objects like tables and views</details>

---

### 46. Which of the following statements is true about transient schemas?  
- A. They have unlimited Time Travel.  
- B. They support up to 90 days of Fail-Safe.  
- C. They offer 0 or 1 day of Time Travel.  
- D. They do not store metadata.  

<details>**Answer:** C. They offer 0 or 1 day of Time Travel.</details>

---

### 47. What is the role of the ACCOUNTADMIN system-defined role in Snowflake?  
- A. Manage network configurations  
- B. Create and manage databases  
- C. Monitor usage across organizations  
- D. Control virtual warehouse scaling  

<details>**Answer:** B. Create and manage databases</details>

---

### 48. What happens when a database is cloned in Snowflake?  
- A. All data is copied physically.  
- B. Metadata is cloned without physical duplication.  
- C. The clone has no access to original data.  
- D. Data is moved to a different region.  

<details>**Answer:** B. Metadata is cloned without physical duplication.</details>

---

### 49. Which of the following statements is true about a Snowflake organization?  
- A. It can host unlimited accounts by default.  
- B. It groups databases under a single schema.  
- C. It is the highest-level securable object.  
- D. It does not support failover features.  

<details>**Answer:** C. It is the highest-level securable object.</details>

---

### 50. Which object in Snowflake can be created from a share provided by another account?  
- A. Sequence  
- B. External table  
- C. Database  
- D. Virtual warehouse  

<details>**Answer:** C. Database</details>

## Multiple-Select Questions (MSQs)

### 1. Which of the following components are included in a Snowflake account URL?  
- A. Account locator  
- B. Schema name  
- C. Cloud provider  
- D. Region identifier  

<details>**Answer:** A. Account locator, C. Cloud provider, D. Region identifier</details>

---

### 2. What characteristics are true for Snowflake's transient databases and schemas?  
- A. Support Time Travel for 0 or 1 day  
- B. Have full Fail-Safe support  
- C. Provide metadata storage  
- D. Do not support Fail-Safe  

<details>**Answer:** A. Support Time Travel for 0 or 1 day, D. Do not support Fail-Safe</details>

---

### 3. Which roles are system-defined in a Snowflake account?  
- A. ACCOUNTADMIN  
- B. SYSADMIN  
- C. DATAENGINEER  
- D. ORGADMIN  

<details>**Answer:** A. ACCOUNTADMIN, B. SYSADMIN, D. ORGADMIN</details>

---

### 4. Which actions can an ORGADMIN role perform?  
- A. Manage billing across multiple accounts  
- B. Monitor usage across multiple regions  
- C. Create a new schema  
- D. Create and manage accounts  

<details>**Answer:** A. Manage billing across multiple accounts, B. Monitor usage across multiple regions, D. Create and manage accounts</details>

---

### 5. Which of the following are true about Snowflake organization objects?  
- A. They serve as a logical grouping of databases.  
- B. They are the top-level securable object.  
- C. They require a request to Snowflake Support to enable.  
- D. They are available in all accounts by default.  

<details>**Answer:** B. They are the top-level securable object, C. They require a request to Snowflake Support to enable</details>

---

### 6. Which of the following objects belong to a schema in Snowflake?  
- A. Tables  
- B. Views  
- C. Roles  
- D. Stored Procedures  

<details>**Answer:** A. Tables, B. Views, D. Stored Procedures</details>

---

### 7. What factors affect the pricing of a Snowflake account?  
- A. Account region  
- B. Account name  
- C. Cloud provider  
- D. Regulatory certifications  

<details>**Answer:** A. Account region, C. Cloud provider, D. Regulatory certifications</details>

---

### 8. Which statements are true about Snowflake external tables?  
- A. They support Time Travel for 7 days.  
- B. They are read-only.  
- C. They are stored outside of Snowflake.  
- D. They can be cloned.  

<details>**Answer:** B. They are read-only, C. They are stored outside of Snowflake</details>

---

### 9. What features are available with Snowflake's organization-level functionality?  
- A. Cross-account database replication  
- B. Cross-region database migration  
- C. Centralized billing and usage monitoring  
- D. Integration with third-party cloud providers  

<details>**Answer:** A. Cross-account database replication, C. Centralized billing and usage monitoring</details>

---

### 10. Which components form the namespace for Snowflake schema objects?  
- A. Organization and account name  
- B. Database and schema name  
- C. Schema and table name  
- D. Warehouse and role  

<details>**Answer:** B. Database and schema name</details>

### 11. Which of the following are true about Snowflake databases?  
- A. Can be cloned  
- B. Can be created from a Share object  
- C. Can be created as Transient  
- D. Automatically replicate across regions  

<details>**Answer:** A. Can be cloned, B. Can be created from a Share object, C. Can be created as Transient</details>

---

### 12. What roles and privileges are required to create a new account in an organization?  
- A. ORGADMIN role  
- B. ACCOUNTADMIN role  
- C. CREATE ACCOUNT privilege  
- D. CREATE USER privilege  

<details>**Answer:** A. ORGADMIN role, C. CREATE ACCOUNT privilege</details>

---

### 13. Which properties define a Snowflake account identifier?  
- A. Account locator  
- B. Database name  
- C. Cloud region  
- D. Cloud provider  

<details>**Answer:** A. Account locator, C. Cloud region, D. Cloud provider</details>

---

### 14. Which statements are true about Snowflake's Time Travel feature?  
- A. Available for Permanent and Transient databases  
- B. Can be disabled for certain objects  
- C. Allows users to query historical data  
- D. Supports up to 90 days of data recovery for Permanent tables  

<details>**Answer:** B. Can be disabled for certain objects, C. Allows users to query historical data, D. Supports up to 90 days of data recovery for Permanent tables</details>

---

### 15. Which of the following can be used to monitor and manage billing in Snowflake?  
- A. ORGADMIN role  
- B. ACCOUNTADMIN role  
- C. INFORMATION_SCHEMA billing views  
- D. SNOWFLAKE shared database  

<details>**Answer:** A. ORGADMIN role, D. SNOWFLAKE shared database</details>

---

### 16. Which actions can be performed using the ACCOUNTADMIN role?  
- A. Create and manage databases  
- B. Configure warehouses  
- C. Create new accounts within the organization  
- D. Assign roles and privileges to users  

<details>**Answer:** A. Create and manage databases, B. Configure warehouses, D. Assign roles and privileges to users</details>

---

### 17. What are some characteristics of Snowflake's schema objects?  
- A. Schemas can be cloned  
- B. Schemas are always public by default  
- C. Schemas store metadata in the information_schema  
- D. Schemas can contain sequences and stages  

<details>**Answer:** A. Schemas can be cloned, C. Schemas store metadata in the information_schema, D. Schemas can contain sequences and stages</details>

---

### 18. Which components are affected by the region where a Snowflake account is provisioned?  
- A. Account and storage price  
- B. Supported Snowflake features  
- C. Regulatory certifications available  
- D. Schema default collation  

<details>**Answer:** A. Account and storage price, B. Supported Snowflake features, C. Regulatory certifications available</details>

---

### 19. Which of the following describe transient schemas in Snowflake?  
- A. No Fail-Safe support  
- B. Supports up to 7 days of Time Travel  
- C. Stores metadata information in INFORMATION_SCHEMA  
- D. Can be replicated across accounts  

<details>**Answer:** A. No Fail-Safe support, C. Stores metadata information in INFORMATION_SCHEMA</details>

---

### 20. Which of the following objects are created automatically when a database is created in Snowflake?  
- A. PUBLIC schema  
- B. INFORMATION_SCHEMA schema  
- C. DEFAULT role  
- D. USER schema  

<details>**Answer:** A. PUBLIC schema, B. INFORMATION_SCHEMA schema</details>


### 21. Which features are enabled after requesting Snowflake's Organization object?  
- A. Account-level database replication  
- B. Failover support across accounts  
- C. Support for external stages  
- D. Billing and usage monitoring across accounts  

<details>**Answer:** A, B, D</details>

---

### 22. What affects Snowflake account pricing?  
- A. The cloud provider selected  
- B. The geographic region of the account  
- C. The number of roles created  
- D. The amount of data stored  

<details>**Answer:** A, B, D</details>

---

### 23. Which objects can be cloned in Snowflake?  
- A. Stages  
- B. Databases  
- C. Schemas  
- D. Roles  

<details>**Answer:** B, C</details>

---

### 24. Which properties are true for a `Transient` schema in Snowflake?  
- A. Supports 0 or 1 day Time Travel  
- B. Has Fail-Safe support  
- C. Cannot be replicated  
- D. Offers reduced storage cost  

<details>**Answer:** A, D</details>

---

### 25. What is included in the account URL structure in Snowflake?  
- A. Account locator  
- B. Cloud provider  
- C. Warehouse name  
- D. Cloud services region  

<details>**Answer:** A, B, D</details>

---

## True/False

### 1. The `ORGADMIN` role is responsible for managing accounts within a Snowflake organization.  
<details>**Answer:** True</details>

---

### 2. A Snowflake organization can only have a maximum of 10 accounts.  
<details>**Answer:** False</details>

---

### 3. Snowflake databases can be cloned and replicated.  
<details>**Answer:** True</details>

---

### 4. A database created from a Share object cannot be modified by the recipient.  
<details>**Answer:** True</details>

---

### 5. Transient databases in Snowflake offer up to 7 days of Time Travel.  
<details>**Answer:** False</details>

---

### 6. The PUBLIC schema is the default schema for any object created without specifying a schema.  
<details>**Answer:** True</details>

---

### 7. Snowflake automatically moves data between regions for disaster recovery.  
<details>**Answer:** False</details>

---

### 8. The account URL for Snowflake includes the account locator, cloud region, and cloud provider.  
<details>**Answer:** True</details>

---

### 9. Snowflake provides support for vanity URLs to customize the account URL with a company’s name.  
<details>**Answer:** True</details>

---

### 10. The ACCOUNTADMIN role has privileges to manage all database and schema objects in Snowflake.  
<details>**Answer:** True</details>

---

### 11. A Snowflake schema cannot contain stages or sequences.  
<details>**Answer:** False</details>

---

### 12. The INFORMATION_SCHEMA schema stores metadata information about Snowflake objects.  
<details>**Answer:** True</details>

---

### 13. A Snowflake account can be hosted on multiple cloud providers simultaneously.  
<details>**Answer:** False</details>

---

### 14. The cloud region affects the pricing and available regulatory certifications for a Snowflake account.  
<details>**Answer:** True</details>

---

### 15. By default, when a database is created, it includes only one schema.  
<details>**Answer:** False</details>

---

### 16. Transient schemas in Snowflake do not support Fail-Safe.  
<details>**Answer:** True</details>

---

### 17. An ORGADMIN role can create new accounts within a Snowflake organization.  
<details>**Answer:** True</details>

---

### 18. Snowflake stages are used to store metadata for data files.  
<details>**Answer:** False</details>

---

### 19. The primary purpose of a schema in Snowflake is to group related tables and views.  
<details>**Answer:** True</details>

---

### 20. The ORGANIZATION object in Snowflake is available to all users by default.  
<details>**Answer:** False</details>


### 21. Snowflake accounts can span multiple cloud providers by default.  
<details>**Answer:** False</details>

---

### 22. An organization can contain a maximum of 25 accounts by default.  
<details>**Answer:** True</details>

---

### 23. The `ORGADMIN` role is required to manage the organization in Snowflake.  
<details>**Answer:** True</details>

---

### 24. Databases in Snowflake can be created from shared objects provided by other accounts.  
<details>**Answer:** True</details>

---

### 25. Time Travel in transient databases can be configured for up to 7 days.  
<details>**Answer:** False</details>

---

### 26. The `information_schema` is used to store user data in Snowflake.  
<details>**Answer:** False</details>

---

### 27. The region of a Snowflake account does not affect the account’s storage price.  
<details>**Answer:** False</details>

---

### 28. A schema in Snowflake can belong to multiple databases.  
<details>**Answer:** False</details>

---

### 29. Snowflake provides support for creating a vanity URL for accounts.  
<details>**Answer:** True</details>

---

### 30. The `public` schema is automatically created in every new Snowflake database.  
<details>**Answer:** True</details>

---



