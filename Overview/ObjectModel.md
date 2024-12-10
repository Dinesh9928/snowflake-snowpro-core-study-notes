# Objects #
* All objects in Snowflake are securable - privileges on objects can be granted to Roles and Roles are granted to Users.
* Additionally, all objects can be interacted with and configured via SQL given the user has sufficient privileges.
* To own an object means that a role (NOT USER!) has the OWNERSHIP privilege on the object.
* Each securable object is owned by a single role, which by default is the role used to create the object.
* Other than the Organization and Account objects, objects can be at the Account level or the Schema level. Examples:
  * Account level objects:
    * Network Policy
    * User
    * Role
    * Grants
    * Warehouse
    * Database
    * Share
    * Resource Monitor
    * Storage Integration
  * Schema level objects:
    * Table
    * External Table
    * View
    * Stream
    * Task
    * Stored Procedure
    * UDF (User Defined Function)
    * Sequence
    * Stage
    * File Format
    * Pipe

> * Note that object identifiers are case insensitive and cannot contain spaces or special characters unless they are enclosed in quotes.
> * When an object identifier is enclosed in quotes, it becomes case sensitive. This should be avoided as it can contribute to hard to debug issues!

![](../images/SecurableObjectsHierarchy.png)

* Schemas and Database Roles are Database Level Objects
* The definitions of most objects can be retrieved using the [GET_DDL()](https://docs.snowflake.com/en/sql-reference/functions/get_ddl.html) function.

## Table ##
Tables hold all the data in a database. 

### Permanent ###
* Default table type, used for the highest level of data protection and recovery
* Persists until dropped
* 0–90 days of Time Travel depending on the Snowflake edition (0–1 days in Standard Edition, 0-90 days in Enterprise Edition and up)
* 7 days of Fail-Safe
* Can be cloned to a permanent, transient or temporary table
  * If you clone a Permanent table to Transient or Temporary, the old partitions will remain Permanent, but any new partitions added to the clone will be Transients/Temporary

### Transient ###
* Persists until dropped
* Available across sessions
* Time Travel: 0 or 1 days
* No Fail-Safe support
* Can only be cloned to a Temporary or Transient table

### Temporary ###
* Used for transitory data
* Tied to and available only within the context a single user session. As such, they are not visible to other users or sessions
* Time Travel: 0 or 1 days
  * A temporary table is purged once the session ends, so the actual retention period is for 24 hours or the remainder of the session, whichever is less
* Temporary tables incur storage costs
* No Fail-Safe support
* Can only be cloned to a temporary or transient table
* Can be created with a clustering key, if needed

### External ###
* Snowflake table "over" data stored in an external stage
* Persists until removed
* Read-only
* No Time Travel
* No Fail-Safe support
* Cloning is not supported
* XML files are not supported for external tables

## View ##
Named definition of a SQL query which can be queried as if it were a table. Views can be created on all table types, including external tables.

### Standard ###
* Default view type
* No data is stored
* No clustering support
* Executes as the role which owns it
* DDL available to any role with access to the view
* [Streams](https://docs.snowflake.com/en/user-guide/streams-intro.html) are supported

### Materialized ###
* Enterprise Edition is required for Materialized views
* Results of underlying query are stored
  * require storage space and active maintenance, which incur both storage and compute costs
  * while Materialized Views behave more like Tables, Time Travel is not supported
* Results are auto-refreshed in the background so new data loaded into the table will be propagated into the Materialized View
* Consumes background compute for the auto-refresh
  * The background compute does not require a customer-provided warehouse; Snowflake manages the compute
* Clustering is supported
* NO support for [Streams](https://docs.snowflake.com/en/user-guide/streams-intro.html)
* Changes to the schema of base table are not automatically propagated to materialized views
* Best used for:
  * Query results contain a small number of rows and/or columns relative to the underlying table
  * Queries that require significant processing, e.g. analysis of semi-structured data or expensive aggregates
  * The underlying table is an external one, which might be slower to query directly
  * The view’s base table does not change frequently

### Secure ###
Both Standard and Materialized views can also be defined as Secure
* Executes as the role which owns it
* DDL available only to authorized users
* Can be shared; secure views are mandatory when sharing
* Snowflake query optimizer bypasses optimizations used for regular views so secure views are less performant

## Stage ##
Stages in Snowflake specify where data files are stored (staged) in cloud storage. They facilitate data loading and unloading.
* When staging uncompressed files in a Snowflake stage, the files are automatically compressed using `GZIP` unless compression is explicitly disabled
* Snowflake automatically generates metadata for staged files:
* `METADATA$FILENAME`: stage path and name of the data file the current row belongs to
* `METADATA$FILE_ROW_NUMBER`: row number for each record in the staged data file
* `$[COLUMN_NUMBER]`: staged data file column number; e.g. `$1` refers to the first column in the staged file, `$2` refers to the second column, etc.

### Named Stages ###
* Created manually
* Referenced with `@[STAGE_NAME]`
* can specify file format
* Supported Cloud Storage services:
  * Amazon S3 Buckets
  * Google Cloud Storage Buckets
  * Microsoft Azure Containers
* Named Stages can be Internal or External
  * Internal Named Stage: cloud Storage location managed by Snowflake; data is stored internally within Snowflake.
  * Named External Stage: references data files stored outside Snowflake.
    * managed by the user or respective cloud provider and can even be in a different cloud provider than the Snowflake account
    * Creating an external stage requires providing
      * URL where the files are located
      * credentials to access the files
      * decryption keys to enable decryption of the data

### Internal Stages ###
* Can only be accessed using the SnowSQL CLI
* Can be of two types: User and Table Stage

#### User Stage ####
* Each user has a Snowflake personal stage allocated to them by default
* Referenced with `@~`
* Can only be accessed by the user it belongs to
* User stages do not support setting file format options. Instead, you must specify file format and copy options as part of the [COPY INTO <table>](https://docs.snowflake.com/en/sql-reference/sql/copy-into-table) command.

#### Table Stage ####
* Each table has a Snowflake stage allocated to it by default
* Referenced with `@%[TABLE_NAME]`
* Accessible to multiple users 
* Files can only be loaded/unloaded to/from the table the stage belongs to

## File Format ##
Pre-defined format structure that describes a set of staged data to access or load into Snowflake tables for CSV, JSON, AVRO, ORC, PARQUET, and XML input types
* A File Format can be created as a clone from another File Format

## Storage Integration ##
A Snowflake object that stores a generated identity and access management (IAM) entity for your external cloud storage, along with an optional set of allowed or blocked storage locations. This option enables users to create stages and load and unload data without supplying credentials.
* Account-level object
* A single storage integration can support multiple external stages

## Warehouse ##
Warehouses are the compute part of the Snowflake engine. They are a set of virtual machines provided at runtime to help execute a given query.

## Resource Monitor ##
See [Resource Monitors](../VirtualWarehouses/ResourceMonitors.md)

## Sequence ##
> [Using Sequences](https://docs.snowflake.com/en/user-guide/querying-sequences)

Sequences are schema-level objects used to generate unique numbers across sessions and statements, including concurrent statements. They can generate values for a primary key or any column that requires a unique value. They have an initial value and an interval.

You can access sequences in queries as expressions. The function `nextval`, will generate the next unique sequential value.
```postgres-psql
CREATE OR REPLACE SEQUENCE seq START = 1 INCREMENT = 5;
CREATE OR REPLACE TABLE PEOPLE 
(
  ID NUMBER DEFAULT seq.nextval, 
  NAME VARCHAR(50)
);
```

## Pipe ##
A special type of object which enables the automatic loading of data from Stage files as soon as they are available.
See [Snowpipe](../DataMovement/ContinuousDataLoading.md)

## Stored Procedure ##
> [Stored Procedure](https://docs.snowflake.com/en/sql-reference/stored-procedures.html)

Extend the system to perform operations.
* A stored procedure can return a single value or (if you are using Snowflake Scripting) tabular data
* Stored procedures can be written in:
  * Java (using Snowpark)
  * JavaScript
  * Python (using Snowpark)
  * Scala (using Snowpark)
  * [Snowflake Scripting](https://docs.snowflake.com/en/developer-guide/snowflake-scripting/index.html), an extension to Snowflake SQL that adds support for procedural logic
* Stored Procedures can be secure or unsecure
* Stored Procedures support schema definition (DDL) and data modification (DML) SQL statements
* Stored Procedures can run multiple SQL statements
* Stored Procedures are allowed, but not required, to explicitly return a value (such as an error indicator)
* Stored Procedures are called as independent statements
  ```postgres-psql
  CALL MyStoredProcedure_1(argument_1);
  ```
* The returned values CANNOT be used directly in a SQL statement
* Every `CREATE PROCEDURE` statement must include a `RETURNS` clause that defines a return type, even if the procedure does not explicitly return anything
* Store Procedures CANNOT return a set of rows
* Stored Procedures can be defined to run as their owner (by default) or as the procedure caller (`EXECUTE AS CALLER`)
* Stored Procedures interact with Snowflake using an API in the respective language. Here are some JavaScript API Objects:
  * `Snowflake` - contains the methods of the Stored Procedure API; this object is accessible by default (there is no need to create it)
  * `Statement` - provides method to execute a query statement and access its metadata
  * `ResultSet` - iterable object which contains the query results;
  * `SfDate` - wrapper around the Snowflake SQL `TIMESTAMP` data types
* Snowflake Scripting allow migrating of other databases' stored procedures by embedding their SQL in the Snowflake Stored Procedure's code
* Argument names in the SQL portions of a Stored Procedure are case-insensitive

## UDF: User-Defined Function ##
> [User Defined Function](https://docs.snowflake.com/en/sql-reference/user-defined-functions.html)

UDFs extend Snowflake to perform operations that are not available through built-in, system-defined functions.
* UDFs accept 0 or more parameters.
* For each row passed to a UDF, the UDF must return either:
  * a scalar (i.e. single) value, or
  * if defined as a `RETURNS TABLE` function, a set of zero or more rows.
    * When the UDF returns a table, it can be called a User Defined Table Function (UDTF)
* A UDF can be written in:
  * SQL (default)
  * Java
  * JavaScript
  * Python
* UDFs can be secure or unsecure
* UDFs do not support schema definitions (DDL) or data modifications (DML)
* The returned value(s) CAN be used directly in statement SQL
* While system functions can be listed with `SHOW FUNCTIONS;`, to list UDFs, use `SHOW USER FUNCTIONS;`
* When calling a UDF which returns a table, you must wrap it in the `TABLE()` function
  ```postgres-psql
  SELECT * FROM TABLE(MY_UDTF_FUNCTION(param1, param2));
  ```

## Data Shares ##
See [Data Sharing](DataSharing.md)

### **Multiple-Choice Questions (MCQs)**



### 1. Which Snowflake object holds the highest level of privileges?  
- A. User  
- B. Role  
- C. Organization  
- D. Warehouse  

<details>**Answer:** C.</details>



### 2. What is the default privilege granted to the role that creates an object?  
- A. USAGE  
- B. OWNERSHIP  
- C. EXECUTE  
- D. READ  

<details>**Answer:** B.</details> 



### 3. Which of the following is NOT a schema-level object?  
- A. Table  
- B. Role  
- C. View  
- D. Stream  

<details>**Answer:** B.</details>



### 4. What is the default storage type for a Snowflake table?  
- A. Temporary  
- B. External  
- C. Permanent  
- D. Transient  

<details>**Answer:** C.</details>



### 5. How many days of Time Travel does a Permanent table offer in the Enterprise Edition of Snowflake?  
- A. 1 day  
- B. 7 days  
- C. 30 days  
- D. 90 days  

<details>**Answer:** D.</details>



### 6. Which Snowflake feature allows automatic loading of data from a stage?  
- A. Stored Procedure  
- B. Snowpipe  
- C. File Format  
- D. Sequence  

<details>**Answer:** B.</details>



### 7. What command is used to retrieve the DDL of a Snowflake object?  
- A. SHOW TABLE  
- B. GET_DDL()  
- C. DESCRIBE  
- D. LIST_DDL()  

<details>**Answer:** B.</details>



### 8. In Snowflake, which type of table is read-only and does not support Time Travel?  
- A. Permanent Table  
- B. Transient Table  
- C. Temporary Table  
- D. External Table  

<details>**Answer:** D.</details>



### 9. Which of the following objects stores metadata for data files staged in Snowflake?  
- A. Sequence  
- B. Pipe  
- C. Stage  
- D. Stream  

<details>**Answer:** C.</details>



### 10. Which object allows generating unique numbers across different sessions in Snowflake?  
- A. Stage  
- B. Sequence  
- C. Stream  
- D. View  

<details>**Answer:** B.</details>

11. **What type of object is a Snowflake `Warehouse`?**  
   A. Schema-level object  
   B. Account-level object  
   C. Database-level object  
   D. User-level object  
   <details>**Answer:** B</details>

12. **Which Snowflake object provides compute resources for query execution?**  
   A. Database  
   B. Role  
   C. Warehouse  
   D. Sequence  
   <details>**Answer:** C</details>

13. **In Snowflake, which of the following tables supports Fail-Safe?**  
   A. External Table  
   B. Transient Table  
   C. Permanent Table  
   D. Temporary Table  
   <details>**Answer:** C</details>

14. **Which of the following is used to define a sequence in Snowflake?**  
   A. CREATE SEQUENCE  
   B. CREATE INDEX  
   C. CREATE FUNCTION  
   D. CREATE STREAM  
   <details>**Answer:** A</details>

15. **What is the purpose of a `File Format` object in Snowflake?**  
   A. To define the structure of database objects  
   B. To define the format of staged data files  
   C. To create user-defined functions  
   D. To manage permissions for users  
   <details>**Answer:** B</details>

16. **Which type of view in Snowflake stores the query results and requires maintenance?**  
   A. Standard View  
   B. Secure View  
   C. Materialized View  
   D. Temporary View  
   <details>**Answer:** C</details>

17. **How can you access a User Stage in Snowflake?**  
   A. `@USER`  
   B. `@%TABLE_NAME`  
   C. `@~`  
   D. `@DEFAULT_STAGE`  
   <details>**Answer:** C</details>

18. **Which statement is true about Transient Tables in Snowflake?**  
   A. They support Time Travel for up to 30 days.  
   B. They have Fail-Safe for 7 days.  
   C. They persist until dropped and do not support Fail-Safe.  
   D. They are automatically dropped at the end of a session.  
   <details>**Answer:** C</details>

19. **In Snowflake, what does the `Pipe` object do?**  
   A. Automatically refreshes Materialized Views  
   B. Enables the automatic loading of staged files into a table  
   C. Executes stored procedures  
   D. Generates unique sequential values  
   <details>**Answer:** B</details>

20. **Which command is used to run a stored procedure in Snowflake?**  
   A. EXECUTE PROCEDURE  
   B. CALL  
   C. RUN PROCEDURE  
   D. INVOKE  
   <details>**Answer:** B</details>

21. **Which of the following Snowflake objects is read-only?**  
   A. Permanent Table  
   B. External Table  
   C. Transient Table  
   D. Temporary Table  
   <details>**Answer:** B</details>

22. **How are Materialized Views in Snowflake maintained?**  
   A. Manually by users  
   B. Automatically in the background  
   C. Through external scripts  
   D. By the cloud provider  
   <details>**Answer:** B</details>

23. **Which of the following objects allows for Secure Data Sharing in Snowflake?**  
   A. Virtual Warehouse  
   B. Secure View  
   C. Transient Table  
   D. Materialized View  
   <details>**Answer:** B</details>

24. **What is the default type of table created in Snowflake if no table type is specified?**  
   A. Temporary Table  
   B. Transient Table  
   C. Permanent Table  
   D. External Table  
   <details>**Answer:** C</details>

25. **Which of the following Snowflake objects is NOT schema-level?**  
   A. Table  
   B. Sequence  
   C. Warehouse  
   D. View  
   <details>**Answer:** C</details>

26. **Which Snowflake feature allows tables to be cloned with zero data copying?**  
   A. Data Replication  
   B. Zero-Copy Cloning  
   C. Data Sharing  
   D. Table Partitioning  
   <details>**Answer:** B</details>

27. **Which object in Snowflake is used to monitor and manage resource consumption?**  
   A. Warehouse  
   B. Resource Monitor  
   C. Task  
   D. Stage  
   <details>**Answer:** B</details>

28. **Which object holds metadata about files staged in Snowflake?**  
   A. File Format  
   B. Internal Stage  
   C. Table Stage  
   D. Resource Monitor  
   <details>**Answer:** C</details>

29. **What does the `GET_DDL` function in Snowflake do?**  
   A. Retrieves metadata for all files in a stage  
   B. Retrieves the definition of a database object  
   C. Deletes a database object  
   D. Grants privileges on a database object  
   <details>**Answer:** B</details>

30. **Which file format is NOT supported for External Tables in Snowflake?**  
   A. Parquet  
   B. Avro  
   C. XML  
   D. CSV  
   <details>**Answer:** C</details>

31. **What is the primary purpose of Snowflake’s Resource Monitor?**  
   A. To manage file formats for data loading  
   B. To monitor and control credit usage  
   C. To store the metadata of database objects  
   D. To define the clustering key for tables  
   <details>**Answer:** B</details>

32. **Which Snowflake object is referenced using `@~`?**  
   A. Table Stage  
   B. User Stage  
   C. Internal Named Stage  
   D. External Named Stage  
   <details>**Answer:** B</details>

33. **Which of the following commands is used to create a sequence in Snowflake?**  
   A. CREATE PIPE  
   B. CREATE INDEX  
   C. CREATE SEQUENCE  
   D. CREATE STAGE  
   <details>**Answer:** C</details>

34. **What is the default retention period for Time Travel on Permanent Tables in the Enterprise Edition of Snowflake?**  
   A. 1 day  
   B. 7 days  
   C. 30 days  
   D. 90 days  
   <details>**Answer:** D</details>

35. **Which type of stage in Snowflake is automatically available for every table?**  
   A. User Stage  
   B. Named Stage  
   C. External Stage  
   D. Table Stage  
   <details>**Answer:** D</details>

36. **Which object in Snowflake enables automatic data loading as soon as new data is available?**  
   A. Sequence  
   B. Materialized View  
   C. Pipe  
   D. UDF  
   <details>**Answer:** C</details>

37. **What type of view in Snowflake is required for Secure Data Sharing?**  
   A. Standard View  
   B. Materialized View  
   C. Secure View  
   D. Temporary View  
   <details>**Answer:** C</details>

38. **Which privilege must a role have to be considered the owner of a Snowflake object?**  
   A. USAGE  
   B. OWNERSHIP  
   C. SELECT  
   D. MANAGE  
   <details>**Answer:** B</details>

39. **Which Snowflake object allows external data files to be accessed by Snowflake without being copied into Snowflake storage?**  
   A. Internal Stage  
   B. External Table  
   C. File Format  
   D. Database Share  
   <details>**Answer:** B</details>

40. **Which of the following is a schema-level object in Snowflake?**  
   A. Role  
   B. Warehouse  
   C. Table  
   D. Network Policy  
   <details>**Answer:** C</details>

41. **Which of the following statements is true about Temporary Tables in Snowflake?**  
   A. They support Fail-Safe for 7 days.  
   B. They are visible across all sessions.  
   C. They are automatically dropped at the end of a session.  
   D. They support 30 days of Time Travel.  
   <details>**Answer:** C</details>

42. **Which Snowflake function can be used to retrieve the DDL of an object?**  
   A. GET_METADATA()  
   B. GET_OBJECT()  
   C. SHOW_OBJECT()  
   D. GET_DDL()  
   <details>**Answer:** D</details>

43. **Which type of Snowflake table does not support Time Travel or Fail-Safe?**  
   A. Permanent Table  
   B. Temporary Table  
   C. External Table  
   D. Transient Table  
   <details>**Answer:** C</details>

44. **What is the key difference between a Secure View and a Standard View in Snowflake?**  
   A. Secure Views can only be used for internal queries.  
   B. Secure Views execute with the privileges of the caller.  
   C. Secure Views can be shared with other accounts.  
   D. Secure Views are less performant due to bypassed optimizations.  
   <details>**Answer:** C</details>, D

45. **Which Snowflake object allows the execution of multiple SQL statements and can return a single value or tabular data?**  
   A. UDF  
   B. Stored Procedure  
   C. Sequence  
   D. Materialized View  
   <details>**Answer:** B</details>

46. **What file format is NOT supported by Snowflake’s File Format objects?**  
   A. CSV  
   B. JSON  
   C. XML  
   D. RTF  
   <details>**Answer:** D</details>

47. **What Snowflake feature allows for cloning an object without copying its data?**  
   A. Data Replication  
   B. Secure Data Sharing  
   C. Zero-Copy Cloning  
   D. Materialized Views  
   <details>**Answer:** C</details>

48. **Which Snowflake object stores the credentials required to access external cloud storage?**  
   A. Stage  
   B. Resource Monitor  
   C. Storage Integration  
   D. Pipe  
   <details>**Answer:** C</details>

49. **What is a valid use case for Materialized Views in Snowflake?**  
   A. Real-time streaming data  
   B. Frequently changing base tables  
   C. Queries requiring expensive aggregates  
   D. Storing user authentication data  
   <details>**Answer:** C</details>

50. **Which command allows you to list all User-Defined Functions (UDFs) in Snowflake?**  
   A. SHOW FUNCTIONS;  
   B. SHOW USER FUNCTIONS;  
   C. DESCRIBE FUNCTIONS;  
   D. LIST FUNCTIONS;  
   <details>**Answer:** B</details>

51. **Which Snowflake feature automatically compresses uncompressed files staged in an internal stage?**  
    A. DEFLATE  
    B. ZIP  
    C. GZIP  
    D. LZW  
    <details>**Answer:** C</details>

52. **What does `@%[TABLE_NAME]` reference in Snowflake?**  
    A. Table Stage  
    B. User Stage  
    C. Named Stage  
    D. External Stage  
    <details>**Answer:** A</details>

53. **Which of the following operations can be performed by Snowflake Stored Procedures?**  
    A. Modify schema definitions (DDL)  
    B. Perform data modifications (DML)  
    C. Execute multiple SQL statements  
    D. Return sets of rows directly into SQL statements  
    <details>**Answer:** A</details>, B, C

54. **Which of the following is an example of a Snowflake schema-level object?**  
    A. Network Policy  
    B. Sequence  
    C. Role  
    D. Warehouse  
    <details>**Answer:** B</details>

55. **What is the purpose of the `GET_DDL()` function in Snowflake?**  
    A. To retrieve data from a table  
    B. To list all files in a stage  
    C. To retrieve the DDL (Data Definition Language) for an object  
    D. To create a materialized view  
    <details>**Answer:** C</details>

56. **Which Snowflake object allows users to generate unique numbers across sessions and statements?**  
    A. Table  
    B. Sequence  
    C. UDF  
    D. Pipe  
    <details>**Answer:** B</details>

57. **What is required when creating an External Named Stage in Snowflake?**  
    A. File format configuration only  
    B. Only the stage URL  
    C. URL, access credentials, and decryption keys (if required)  
    D. Internal storage settings  
    <details>**Answer:** C</details>

58. **What type of Snowflake table is read-only and does not support cloning?**  
    A. Permanent Table  
    B. External Table  
    C. Transient Table  
    D. Temporary Table  
    <details>**Answer:** B</details>

59. **What is a key feature of Secure Views in Snowflake?**  
    A. Can be optimized automatically by Snowflake query optimizer  
    B. Allows modification of source table schema  
    C. DDL visibility is restricted to authorized users  
    D. Can be shared with external accounts  
    <details>**Answer:** C</details>, D

60. **What metadata field in Snowflake automatically stores the path and name of a staged file?**  
    A. METADATA$PATH  
    B. METADATA$ROW_NUMBER  
    C. METADATA$FILENAME  
    D. METADATA$FILE_FORMAT  
    <details>**Answer:** C</details>

61. **Which of the following are benefits of using Materialized Views in Snowflake?**  
    A. Auto-refreshing query results  
    B. Storing no data, similar to Standard Views  
    C. Supporting streams for incremental changes  
    D. Reducing query processing for frequently accessed data  
    <details>**Answer:** A</details>, D

62. **Which file format is NOT supported natively by Snowflake File Formats?**  
    A. ORC  
    B. JSON  
    C. CSV  
    D. DOCX  
    <details>**Answer:** D</details>

63. **What distinguishes a User-Defined Function (UDF) from a Stored Procedure in Snowflake?**  
    A. UDFs can be used directly in SQL statements, Stored Procedures cannot  
    B. UDFs modify data, while Stored Procedures cannot  
    C. Stored Procedures execute multiple SQL statements, UDFs execute one  
    D. UDFs can return tabular data, while Stored Procedures cannot  
    <details>**Answer:** A</details>, C

64. **Which of the following commands lists all User-Defined Functions in a Snowflake account?**  
    A. SHOW USER FUNCTIONS;  
    B. LIST FUNCTIONS;  
    C. SHOW ALL FUNCTIONS;  
    D. DESCRIBE FUNCTIONS;  
    <details>**Answer:** A</details>

65. **Which Snowflake object is required to load data automatically using Snowpipe?**  
    A. Materialized View  
    B. Sequence  
    C. Pipe  
    D. External Table  
    <details>**Answer:** C</details>

66. **In Snowflake, what object is referenced using `@~`?**  
    A. Table Stage  
    B. Named Stage  
    C. User Stage  
    D. Internal Stage  
    <details>**Answer:** C</details>

67. **Which Snowflake table type supports cloning but has no Fail-Safe?**  
    A. Permanent Table  
    B. Temporary Table  
    C. Transient Table  
    D. External Table  
    <details>**Answer:** C</details>

68. **Which of the following statements is true about Transient Tables in Snowflake?**  
    A. They support 7 days of Fail-Safe.  
    B. They persist across user sessions.  
    C. They can only be cloned to Permanent Tables.  
    D. They support 90 days of Time Travel in Enterprise Edition.  
    <details>**Answer:** B</details>

69. **Which Snowflake feature allows querying external data without copying it into Snowflake?**  
    A. Materialized View  
    B. External Table  
    C. Zero-Copy Cloning  
    D. Resource Monitor  
    <details>**Answer:** B</details>

70. **Which Snowflake function is used to access the next unique value from a sequence?**  
    A. NEXTVAL  
    B. GETVAL  
    C. SEQ_NEXT  
    D. GENERATEVAL  
    <details>**Answer:** A</details>

71. **What type of stage is automatically allocated to each table in Snowflake?**  
    A. Named Stage  
    B. User Stage  
    C. Internal Stage  
    D. Table Stage  
    <details>**Answer:** D</details>

72. **Which Snowflake object is responsible for storing identity and access management (IAM) credentials for external cloud storage?**  
    A. Resource Monitor  
    B. Storage Integration  
    C. User Stage  
    D. Warehouse  
    <details>**Answer:** B</details>

73. **What is the primary function of Snowflake Resource Monitors?**  
    A. Managing storage integrations  
    B. Monitoring and controlling credit usage  
    C. Managing data sharing across accounts  
    D. Providing encryption for external tables  
    <details>**Answer:** B</details>

74. **Which of the following data types can a Sequence in Snowflake generate?**  
    A. Boolean  
    B. Numeric  
    C. String  
    D. Binary  
    <details>**Answer:** B</details>

75. **Which Snowflake table type is purged once the session ends?**  
    A. Permanent Table  
    B. Transient Table  
    C. Temporary Table  
    D. External Table  
    <details>**Answer:** C</details>

76. **What is the default role when an object is created in Snowflake?**  
    A. ACCOUNTADMIN  
    B. The role used to create the object  
    C. SYSADMIN  
    D. PUBLIC  
    <details>**Answer:** B</details>

77. **Which command is used to retrieve the DDL of a database object in Snowflake?**  
    A. SHOW OBJECT DDL;  
    B. GET_OBJECT_DDL();  
    C. GET_DDL();  
    D. SHOW CREATE OBJECT;  
    <details>**Answer:** C</details>

78. **What is a key benefit of Snowflake’s Secure Views over Standard Views?**  
    A. Higher query performance  
    B. Automatic schema propagation  
    C. Restricted DDL access for enhanced security  
    D. Support for data sharing with external accounts  
    <details>**Answer:** C</details>, D

79. **Which of the following Snowflake stages can access multiple cloud storage providers?**  
    A. Table Stage  
    B. User Stage  
    C. Named External Stage  
    D. Internal Stage  
    <details>**Answer:** C</details>

80. **In Snowflake, which object type persists until explicitly dropped?**  
    A. User Stage  
    B. Transient Table  
    C. Permanent Table  
    D. Temporary Table  
    <details>**Answer:** C</details>

81. **Which Snowflake object is responsible for automatically loading data as soon as it is available?**  
    A. Pipe  
    B. Stored Procedure  
    C. Sequence  
    D. UDF  
    <details>**Answer:** A</details>

82. **Which Snowflake view type automatically refreshes data in the background?**  
    A. Standard View  
    B. Materialized View  
    C. Secure View  
    D. External View  
    <details>**Answer:** B</details>

83. **What is the purpose of clustering keys in Snowflake tables?**  
    A. Enhance query performance by optimizing data access patterns  
    B. Increase the storage efficiency of large tables  
    C. Enable automatic data replication across regions  
    D. Define user-level permissions for specific tables  
    <details>**Answer:** A</details>

84. **What happens if you clone a Permanent Table into a Transient Table in Snowflake?**  
    A. The entire table becomes permanent  
    B. The new partitions become transient, while old partitions remain permanent  
    C. The table automatically becomes temporary  
    D. Fail-Safe is enabled for the cloned table  
    <details>**Answer:** B</details>

85. **What is the default time travel retention period for Permanent Tables in the Enterprise Edition of Snowflake?**  
    A. 1 day  
    B. 7 days  
    C. 30 days  
    D. 90 days  
    <details>**Answer:** D</details>

86. **Which Snowflake object can generate values for a primary key in a table?**  
    A. Pipe  
    B. Sequence  
    C. Stored Procedure  
    D. Materialized View  
    <details>**Answer:** B</details>

87. **Which stage in Snowflake can only be accessed using the SnowSQL CLI?**  
    A. Named Stage  
    B. External Stage  
    C. Internal Stage  
    D. Table Stage  
    <details>**Answer:** C</details>

88. **What function is used to generate the next sequential value from a Snowflake sequence?**  
    A. NEXTVAL  
    B. SEQGEN  
    C. INCREMENTVAL  
    D. GENERATENUMBER  
    <details>**Answer:** A</details>

89. **Which Snowflake feature allows extending its functionality with user-defined functions?**  
    A. Named Stages  
    B. Sequences  
    C. UDF (User-Defined Functions)  
    D. Resource Monitors  
    <details>**Answer:** C</details>

90. **What is the main advantage of Snowflake’s Zero-Copy Cloning feature?**  
    A. Increases performance of large queries  
    B. Creates copies of objects without consuming additional storage  
    C. Automatically compresses staged files  
    D. Allows real-time data sharing between accounts  
    <details>**Answer:** B</details>

91. **Which cloud providers are supported for Snowflake Named External Stages?**  
    A. Amazon S3  
    B. Google Cloud Storage  
    C. Microsoft Azure  
    D. All of the above  
    <details>**Answer:** D</details>

92. **Which statement about Snowflake’s File Formats is true?**  
    A. File formats can only be created for CSV files  
    B. File formats describe how staged data should be accessed or loaded  
    C. File formats automatically encrypt data  
    D. File formats cannot be cloned  
    <details>**Answer:** B</details>

93. **What is the purpose of Snowflake's `COPY INTO` command?**  
    A. Clone a table  
    B. Copy a database object  
    C. Load data from a stage into a table  
    D. Create a new user  
    <details>**Answer:** C</details>

94. **Which of the following is a valid use case for Materialized Views in Snowflake?**  
    A. To store results of frequently executed queries  
    B. To enable Time Travel for historical analysis  
    C. To define data sharing permissions  
    D. To execute external scripts  
    <details>**Answer:** A</details>

95. **Which data type is NOT natively supported by Snowflake sequences?**  
    A. Integer  
    B. Decimal  
    C. String  
    D. Numeric  
    <details>**Answer:** C</details>

96. **Which statement about Snowflake’s Named Internal Stages is true?**  
    A. They are managed by the cloud provider  
    B. They allow specifying file formats  
    C. They are accessible only via SQL commands  
    D. They support external data sources  
    <details>**Answer:** B</details>

97. **Which Snowflake object is used to automatically load data from cloud storage?**  
    A. Stored Procedure  
    B. Pipe  
    C. Sequence  
    D. Resource Monitor  
    <details>**Answer:** B</details>

98. **What is the main difference between Standard and Secure Views in Snowflake?**  
    A. Secure Views automatically optimize queries  
    B. Standard Views cannot be shared, while Secure Views can  
    C. Secure Views store data, whereas Standard Views do not  
    D. Standard Views require special permissions to be queried  
    <details>**Answer:** B</details>

99. **What type of Snowflake object is used to store metadata for staged files?**  
    A. File Format  
    B. Table Stage  
    C. Resource Monitor  
    D. Named Stage  
    <details>**Answer:** D</details>

100. **Which Snowflake feature allows SQL-based procedural logic execution?**  
    A. UDFs  
    B. Sequences  
    C. Snowflake Scripting  
    D. Materialized Views  
    <details>**Answer:** C</details>



## Multiple-Select Questions (MSQ)

### 1. Which of the following Snowflake SQL commands can be used to load data into a table?  
- A. SELECT INTO  
- B. COPY INTO  
- C. INSERT INTO  
- D. UPDATE  

<details>**Answer**: B, C</details>

---

### 2. Which Snowflake functions can be used to retrieve metadata about tables?  
- A. SHOW TABLES  
- B. INFORMATION_SCHEMA.TABLES  
- C. GET_TABLE_METADATA  
- D. FETCH_TABLE_INFO  

<details>**Answer**: A, B</details>

---

### 3. What are valid file formats that can be used in Snowflake for data loading?  
- A. CSV  
- B. JSON  
- C. PARQUET  
- D. XML  

<details>**Answer**: A, B, C</details>

---

### 4. Which of the following are characteristics of Snowflake’s Virtual Warehouses?  
- A. They can be resized on demand  
- B. They support auto-suspend and auto-resume  
- C. They manage file formats  
- D. They store data directly  

<details>**Answer**: A, B</details>

---

### 5. Which Snowflake stages can be used for data loading?  
- A. User Stage  
- B. Named Internal Stage  
- C. Table Stage  
- D. Resource Monitor  

<details>**Answer**: A, B, C</details>

---

### 6. Which DDL commands in Snowflake are used to modify table structure?  
- A. ALTER TABLE  
- B. DROP TABLE  
- C. TRUNCATE TABLE  
- D. DELETE TABLE  

<details>**Answer**: A, B</details>

---

### 7. Which of the following statements about Snowflake Time Travel are true?  
- A. Time Travel allows querying historical data  
- B. Time Travel period can vary by table type  
- C. Time Travel data is stored in Fail-Safe  
- D. Time Travel is only available for views  

<details>**Answer**: A, B</details>

---

### 8. What are valid Snowflake data types?  
- A. VARCHAR  
- B. NUMBER  
- C. BOOLEAN  
- D. TABLE  

<details>**Answer**: A, B, C</details>

---

### 9. Which objects are considered Schema-level objects in Snowflake?  
- A. Database  
- B. Table  
- C. Stage  
- D. Role  

<details>**Answer**: B, C</details>

---

### 10. Which of the following table types in Snowflake support cloning?  
- A. Permanent  
- B. Transient  
- C. Temporary  
- D. External  

<details>**Answer**: A, B, C</details>

---

### 11. What are characteristics of a Materialized View in Snowflake?  
- A. Stores the results of the underlying query  
- B. Auto-refreshes in the background  
- C. Supports Time Travel  
- D. Can be created in Standard Edition  

<details>**Answer**: A, B</details>

---

### 12. Which objects can be used to manage external cloud storage integrations in Snowflake?  
- A. Network Policy  
- B. Storage Integration  
- C. External Stage  
- D. Resource Monitor  

<details>**Answer**: B, C</details>

---

### 13. Which Snowflake features allow for zero-copy data replication?  
- A. Data Cloning  
- B. Secure Data Sharing  
- C. Fail-Safe  
- D. Time Travel  

<details>**Answer**: A, B</details>

---

### 14. Which of the following are true for Snowflake’s Secure Views?  
- A. They can be shared  
- B. They bypass Snowflake’s query optimizations  
- C. They store data  
- D. They improve performance  

<details>**Answer**: A, B</details>

---

### 15. What Snowflake objects are considered account-level objects?  
- A. Database  
- B. Role  
- C. Warehouse  
- D. User  

<details>**Answer**: B, C, D</details>

---

### 16. Which Snowflake table types do **not** support Fail-Safe?  
- A. Permanent  
- B. Transient  
- C. Temporary  
- D. External  

<details>**Answer**: B, C, D</details>

---

### 17. Which Snowflake objects allow for version control using Time Travel?  
- A. External Tables  
- B. Permanent Tables  
- C. Transient Tables  
- D. Materialized Views  

<details>**Answer**: B, C</details>

---

### 18. Which features of Snowflake ensure data security during transmission?  
- A. Tri-Secret Secure  
- B. Network Policies  
- C. Data Cloning  
- D. End-to-End Encryption  

<details>**Answer**: A, D</details>

---

### 19. Which Snowflake objects support Stored Procedures?  
- A. Schema  
- B. Role  
- C. Table  
- D. Warehouse  

<details>**Answer**: A, C</details>

---

### 20. Which file formats can be defined for a Named Stage in Snowflake?  
- A. CSV  
- B. AVRO  
- C. ORC  
- D. JSON  

<details>**Answer**: A, B, C, D</details>

---

### 21. Which Snowflake object allows the sharing of data between Snowflake accounts?  
- A. Data Share  
- B. Secure View  
- C. Resource Monitor  
- D. Database  

<details>**Answer**: A</details>

---

### 22. Which of the following statements are true about Transient Tables?  
- A. They can be cloned to Permanent tables  
- B. They support Fail-Safe  
- C. They persist only during the session  
- D. Time Travel can be configured for up to 1 day  

<details>**Answer**: A, D</details>

---

### 23. What is the default behavior of Time Travel in Snowflake?  
- A. It is available for up to 90 days  
- B. It allows querying historical data  
- C. It is only available for Permanent Tables  
- D. Time Travel is disabled by default  

<details>**Answer**: B, C</details>

---

### 24. What is the primary purpose of Snowflake’s Virtual Warehouse?  
- A. Storage of Data  
- B. Execution of Queries  
- C. Monitoring Resource Usage  
- D. Managing Security Roles  

<details>**Answer**: B</details>

---

### 25. Which Snowflake object can be used to limit query execution time or warehouse compute consumption?  
- A. Role  
- B. Resource Monitor  
- C. Table  
- D. Stage  

<details>**Answer**: B</details>

---

### 26. Which table type in Snowflake does NOT support Time Travel?  
- A. Permanent  
- B. Transient  
- C. Temporary  
- D. External  

<details>**Answer**: D</details>

---

### 27. Which of the following are valid methods to query external data in Snowflake?  
- A. External Tables  
- B. Materialized Views  
- C. External Stages  
- D. External Sequences  

<details>**Answer**: A, C</details>

---

### 28. Which of the following can be used to create a new schema in Snowflake?  
- A. CREATE SCHEMA  
- B. CREATE DATABASE  
- C. CREATE TABLE  
- D. CREATE DATABASE SCHEMA  

<details>**Answer**: A</details>

---

### 29. What is the default retention period for Permanent Tables in Snowflake?  
- A. 1 Day  
- B. 7 Days  
- C. 90 Days  
- D. Indefinite  

<details>**Answer**: C</details>

---

### 30. Which data formats can be used with Snowflake File Formats for data loading?  
- A. Parquet  
- B. JSON  
- C. CSV  
- D. AVRO  

<details>**Answer**: A, B, C, D</details>

---

### 31. What are some characteristics of Materialized Views in Snowflake?  
- A. Auto-refresh  
- B. Supports Time Travel  
- C. Stores Query Results  
- D. Can be Cloned  

<details>**Answer**: A, C</details>

---

### 32. What is the minimum number of days Time Travel is available for Transient Tables?  
- A. 0  
- B. 1  
- C. 7  
- D. 30  

<details>**Answer**: A</details>

---

### 33. Which object in Snowflake holds a user’s role and permissions?  
- A. Role  
- B. Warehouse  
- C. User  
- D. Table  

<details>**Answer**: A, C</details>

---

### 34. Which objects in Snowflake support Time Travel?  
- A. Views  
- B. Tables  
- C. Stages  
- D. Schemas  

<details>**Answer**: B</details>

---

### 35. Which of the following are valid Snowflake SQL functions for querying a schema’s metadata?  
- A. SHOW SCHEMAS  
- B. INFORMATION_SCHEMA.SCHEMAS  
- C. GET_DDL()  
- D. GET_METADATA()  

<details>**Answer**: A, B</details>

---

### 36. Which of the following are best practices for using Snowflake Tables?  
- A. Partition large tables  
- B. Use Transient Tables for temporary data  
- C. Avoid clustering  
- D. Always use Secure Views  

<details>**Answer**: A, B</details>

---

### 37. What types of queries can be run on Snowflake External Tables?  
- A. SELECT  
- B. INSERT  
- C. DELETE  
- D. UPDATE  

<details>**Answer**: A</details>

---

### 38. Which of the following statements about Snowflake’s Fail-Safe are true?  
- A. Fail-Safe is always enabled for all Snowflake objects  
- B. Fail-Safe is used for data recovery after Time Travel expiration  
- C. Fail-Safe can be configured for specific tables  
- D. Fail-Safe is part of Snowflake's service-level agreement (SLA)  

<details>**Answer**: B</details>

---

### 39. Which Snowflake object is used to set up roles and permissions?  
- A. User  
- B. Role  
- C. Privileges  
- D. Permission Set  

<details>**Answer**: B</details>

---

### 40. Which of the following are characteristics of Snowflake's Resource Monitors?  
- A. They control the consumption of warehouse resources  
- B. They are available at the database level  
- C. They send alerts when thresholds are exceeded  
- D. They help optimize query execution  

<details>**Answer**: A, C</details>

---

### 41. Which of the following statements are true about Snowflake User-Defined Functions (UDFs)?  
- A. They can be written in JavaScript, Python, or SQL  
- B. They can be used in SQL statements directly  
- C. They can modify data in tables  
- D. UDFs return a set of rows  

<details>**Answer**: A, B</details>

---

### 42. Which of the following storage options can Snowflake use?  
- A. Amazon S3  
- B. Google Cloud Storage  
- C. Microsoft Azure Blob Storage  
- D. IBM Cloud Storage  

<details>**Answer**: A, B, C</details>

---

### 43. Which of the following Snowflake features are designed to improve query performance?  
- A. Clustering Keys  
- B. Materialized Views  
- C. Secure Views  
- D. Partitioning  

<details>**Answer**: A, B</details>

---

### 44. Which Snowflake objects can be cloned?  
- A. Tables  
- B. Schemas  
- C. Databases  
- D. Views  

<details>**Answer**: A, B, C</details>

---

### 45. Which of the following file formats support schema-on-read in Snowflake?  
- A. JSON  
- B. Avro  
- C. ORC  
- D. CSV  

<details>**Answer**: A, B, C</details>

---

### 46. Which of the following features is supported by Snowflake Secure Views?  
- A. Automatic Data Cloning  
- B. Data Encryption  
- C. Sharing with other Snowflake accounts  
- D. Performance Optimizations  

<details>**Answer**: B, C</details>

---

### 47. What are the primary use cases for Snowflake's External Tables?  
- A. Storing data within Snowflake  
- B. Loading data from external cloud storage without ingesting it  
- C. Querying external files directly  
- D. Cloning external data  

<details>**Answer**: B, C</details>

---

### 48. Which Snowflake objects are used to create a continuous data pipeline?  
- A. Stream  
- B. Pipe  
- C. Stage  
- D. Warehouse  

<details>**Answer**: A, B</details>

---

### 49. What are the key benefits of using Snowflake’s multi-cluster architecture?  
- A. Increased Query Performance  
- B. Better Data Security  
- C. Scalable Compute Resources  
- D. Enhanced Data Compression  

<details>**Answer**: A, C</details>

---

### 50. Which of the following Snowflake objects are used for querying and managing semi-structured data?  
- A. Variant  
- B. JSON  
- C. Avro  
- D. Parquet  

<details>**Answer**: A, B, C, D</details>

## Ture/False

---

### 1. Snowflake’s Virtual Warehouses are responsible for both computing and storage.  
<details>**Answer**: False. Virtual Warehouses only handle computation, not storage.</details>

---

### 2. Snowflake automatically scales compute resources based on workload demand.  
<details>**Answer**: True.</details>

---

### 3. Snowflake’s Secure Views can be shared between Snowflake accounts.  
<details>**Answer**: True.</details>

---

### 4. Snowflake supports both semi-structured and structured data natively.  
<details>**Answer**: True.</details>

---

### 5. Snowflake’s Fail-Safe option is used to recover data lost due to Time Travel expiration.  
<details>**Answer**: True.</details>

---

### 6. Snowflake allows for only one active virtual warehouse per account.  
<details>**Answer**: False. Multiple virtual warehouses can be active at the same time.</details>

---

### 7. The maximum retention period for Time Travel in Snowflake is 90 days.  
<details>**Answer**: False. The maximum retention period is 1 day for transient tables and 90 days for permanent tables.</details>

---

### 8. Snowflake’s Resource Monitors can be used to limit compute usage based on the cost.  
<details>**Answer**: True.</details>

---

### 9. A Snowflake Temporary Table will persist across sessions.  
<details>**Answer**: False. Temporary tables are session-specific and will be dropped after the session ends.</details>

---

### 10. Snowflake External Tables support querying semi-structured data formats like JSON and Avro.  
<details>**Answer**: True.</details>

---

### 11. Snowflake’s materialized views automatically refresh after every data change.  
<details>**Answer**: False. They refresh based on specific conditions, like the refresh interval.</details>

---

### 12. Snowflake allows the use of SQL, JavaScript, and Python to write User-Defined Functions (UDFs).  
<details>**Answer**: True.</details>

---

### 13. Snowflake provides a "Query History" feature that allows users to track historical queries.  
<details>**Answer**: True.</details>

---

### 14. Snowflake stores all data in its proprietary file format.  
<details>**Answer**: False. Snowflake supports multiple formats such as Parquet, Avro, and JSON.</details>

---

### 15. Snowflake automatically scales its compute resources to handle varying workloads.  
<details>**Answer**: True.</details>

---

### 16. Snowflake automatically compacts the data in all tables without user intervention.  
<details>**Answer**: True.</details>

---

### 17. Snowflake supports data partitioning to enhance query performance.  
<details>**Answer**: False. Snowflake uses clustering instead of traditional partitioning.</details>

---

### 18. Snowflake’s Data Sharing feature allows sharing of data across accounts securely.  
<details>**Answer**: True.</details>

---

### 19. Snowflake’s warehouses are automatically paused when not in use.  
<details>**Answer**: False. Virtual warehouses need to be manually paused unless configured with auto-suspend.</details>

---

### 20. Snowflake External Stages allow users to load data from external locations like S3 without copying it.  
<details>**Answer**: True.</details>

---

### 21. Snowflake supports the creation of read-only roles for security purposes.  
<details>**Answer**: True.</details>

---

### 22. In Snowflake, temporary tables are subject to Time Travel.  
<details>**Answer**: False. Temporary tables do not support Time Travel.</details>

---

### 23. Snowflake’s database tables can be cloned without consuming additional storage space.  
<details>**Answer**: True.</details>

---

### 24. Snowflake supports querying data directly from AWS Redshift clusters.  
<details>**Answer**: False. Snowflake does not natively support querying Redshift data.</details>

---

### 25. Snowflake offers support for multi-cloud data storage across AWS, Azure, and GCP.  
<details>**Answer**: True.</details>

---

### 26. Snowflake’s Object Storage can store large objects such as media files.  
<details>**Answer**: True.</details>

---

### 27. Snowflake’s external tables allow users to perform inserts directly into external files.  
<details>**Answer**: False. External tables allow querying external files, but inserts are not supported.</details>

---

### 28. A Snowflake Data Warehouse automatically scales down if the system detects inactivity for 60 minutes.  
<details>**Answer**: False. The scaling behavior depends on the configured settings, such as auto-suspend and auto-resume.</details>

---

### 29. Snowflake can natively read and write to HDFS.  
<details>**Answer**: False. Snowflake supports cloud storage services like S3, Azure Blob, and Google Cloud Storage.</details>

---

### 30. Snowflake stores data in columnar format to optimize query performance.  
<details>**Answer**: True.</details>

---

### 31. Snowflake’s Secure Data Sharing feature allows data sharing without needing to copy or move data.  
<details>**Answer**: True.</details>

---

### 32. Snowflake has a built-in mechanism to automatically resolve schema conflicts during data loading.  
<details>**Answer**: False. Users need to handle schema conflicts manually.</details>

---

### 33. Snowflake automatically encrypts data both at rest and in transit.  
<details>**Answer**: True.</details>

---

### 34. Snowflake can use multiple virtual warehouses simultaneously to improve performance.  
<details>**Answer**: True.</details>

---

### 35. Snowflake provides real-time data replication between multiple cloud regions.  
<details>**Answer**: True.</details>

---

### 36. Snowflake allows external tables to access data directly from S3 and Google Cloud Storage.  
<details>**Answer**: True.</details>

---

### 37. A Snowflake clone of a database will have independent storage.  
<details>**Answer**: False. Cloning does not use additional storage until modifications are made.</details>

---

### 38. Snowflake’s role-based access control is fully integrated into the platform.  
<details>**Answer**: True.</details>

---

### 39. Snowflake’s Service-Level Agreement (SLA) guarantees zero downtime for maintenance.  
<details>**Answer**: False. Snowflake aims for high availability, but maintenance can result in brief downtimes.</details>

---

### 40. Snowflake does not require indexes to optimize query performance.  
<details>**Answer**: True. Snowflake uses automatic clustering for performance optimization.</details>

---

### 41. Snowflake’s clustering feature can improve performance for large, unstructured datasets.  
<details>**Answer**: True.</details>

---

### 42. Snowflake allows users to share data with external third-party organizations securely.  
<details>**Answer**: True.</details>

---

### 43. Snowflake supports real-time streaming and continuous data loads.  
<details>**Answer**: False. Snowflake is designed for batch data loading.</details>

---

### 44. Snowflake’s data-sharing capabilities allow for both read and write access to shared data.  
<details>**Answer**: False. Shared data is read-only in Snowflake.</details>

---

### 45. Snowflake supports querying data from other cloud-based databases directly.  
<details>**Answer**: False. Snowflake does not natively support querying data from other cloud databases.</details>

---

### 46. Snowflake’s Snowpipe feature can automatically load data as it arrives.  
<details>**Answer**: True.</details>

---

### 47. Snowflake supports native query optimization for both structured and semi-structured data.  
<details>**Answer**: True.</details>

---

### 48. Snowflake supports data sharing between users within the same organization only.  
<details>**Answer**: False. Snowflake allows data sharing across different Snowflake accounts, even between organizations.</details>

---

### 49. Snowflake’s Data Transfer feature can automatically move data between Snowflake accounts.  
<details>**Answer**: True.</details>

---

### 50. Snowflake provides users with an option to manage queries using a central control interface.  
<details>**Answer**: True.</details>
---

### 51. Snowflake automatically converts semi-structured data into relational format when loaded.  
<details>**Answer**: True.</details>

---

### 52. Snowflake allows data sharing between virtual warehouses.  
<details>**Answer**: False. Data sharing is between accounts, not between warehouses.</details>

---

### 53. Snowflake's clustering keys are required for every table.  
<details>**Answer**: False. Clustering keys are optional.</details>

---

### 54. Snowflake supports automatic data optimization and re-clustering.  
<details>**Answer**: True.</details>

---

### 55. Snowflake provides built-in machine learning capabilities.  
<details>**Answer**: False. Snowflake does not include machine learning, but can integrate with external ML tools.</details>

---

### 56. Snowflake’s “Time Travel” feature allows recovery of data up to 1 year.  
<details>**Answer**: False. The maximum is 90 days for permanent tables.</details>

---

### 57. Snowflake’s query optimizer automatically chooses the most efficient execution plan.  
<details>**Answer**: True.</details>

---

### 58. Snowflake's data sharing feature does not allow sharing of encrypted data.  
<details>**Answer**: False. Data sharing allows encrypted data to be shared securely.</details>

---

### 59. Snowflake provides automatic scaling of virtual warehouses based on demand.  
<details>**Answer**: True.</details>

---

### 60. Snowflake allows users to create user-defined SQL functions only.  
<details>**Answer**: False. Snowflake supports user-defined functions in SQL, JavaScript, and Python.</details>

---

### 61. Snowflake provides the capability to clone a table without consuming additional storage until changes are made.  
<details>**Answer**: True.</details>

---

### 62. Snowflake does not support loading data directly into external tables.  
<details>**Answer**: True. External tables are read-only.</details>

---

### 63. Snowflake supports real-time data streaming from Kafka.  
<details>**Answer**: False. Snowflake uses batch-based loading for data, not real-time streaming.</details>

---

### 64. Snowflake supports direct access to data stored in Google Cloud Storage.  
<details>**Answer**: True.</details>

---

### 65. Snowflake’s automatic scaling can increase the number of nodes in a virtual warehouse when needed.  
<details>**Answer**: True.</details>

---

### 66. Snowflake’s query result caching can help reduce query response time for repeated queries.  
<details>**Answer**: True.</details>

---

### 67. Snowflake allows users to create user-defined tables for storing data.  
<details>**Answer**: True.</details>

---

### 68. Snowflake does not support querying large semi-structured files like JSON and Parquet.  
<details>**Answer**: False. Snowflake supports querying semi-structured data formats.</details>

---

### 69. Snowflake’s Virtual Warehouses are required for every operation in the platform.  
<details>**Answer**: False. Snowflake uses virtual warehouses for computation but storage operations are independent.</details>

---

### 70. Snowflake supports both multi-cloud and hybrid-cloud data architectures.  
<details>**Answer**: True.</details>
---

### 71. Snowflake requires users to manually scale virtual warehouses for performance.  
<details>**Answer**: False. Snowflake supports automatic scaling of virtual warehouses.</details>

---

### 72. A user can access a Snowflake stage without needing any privileges.  
<details>**Answer**: False. Users need appropriate privileges to access stages.</details>

---

### 73. Snowflake automatically handles data encryption both in-transit and at-rest.  
<details>**Answer**: True.</details>

---

### 74. Snowflake supports both structured and semi-structured data types.  
<details>**Answer**: True.</details>

---

### 75. Clustering keys in Snowflake are required to optimize query performance.  
<details>**Answer**: False. Clustering keys are optional for performance optimization.</details>

---

### 76. Snowflake’s Snowpipe automatically triggers when new data is available in a stage.  
<details>**Answer**: True.</details>

---

### 77. Snowflake's "Time Travel" feature allows users to access data as it was at any point in the past 10 years.  
<details>**Answer**: False. Time Travel is limited to a maximum of 90 days.</details>

---

### 78. Snowflake only supports loading data in CSV format.  
<details>**Answer**: False. Snowflake supports various formats like JSON, AVRO, Parquet, and more.</details>

---

### 79. Snowflake supports automatic data pruning during query execution.  
<details>**Answer**: True.</details>

---

### 80. Snowflake charges users for storage based on the amount of data stored only.  
<details>**Answer**: False. Snowflake also charges for compute resources.</details>

---

### 81. Snowflake allows you to create tables in external databases using SQL.  
<details>**Answer**: True.</details>

---

### 82. Snowflake’s query execution engine automatically selects the best query plan.  
<details>**Answer**: True.</details>

---

### 83. Snowflake supports parallel query execution for all types of queries.  
<details>**Answer**: True.</details>

---

### 84. Snowflake automatically compresses staged files when loaded.  
<details>**Answer**: True.</details>

---

### 85. Snowflake supports transactional consistency for concurrent users.  
<details>**Answer**: True.</details>

---

### 86. Snowflake tables can be cloned without consuming additional storage unless new data is added.  
<details>**Answer**: True.</details>

---

### 87. Snowflake can run in on-premises environments.  
<details>**Answer**: False. Snowflake is a cloud-based platform.</details>

---

### 88. Snowflake allows users to set custom retention periods for Time Travel.  
<details>**Answer**: False. Retention periods depend on the Snowflake edition, up to 90 days.</details>

---

### 89. Snowflake automatically adds clustering keys to tables to optimize query performance.  
<details>**Answer**: False. Clustering keys must be manually added to tables.</details>

---

### 90. Snowflake integrates with third-party ETL tools for data loading.  
<details>**Answer**: True.</details>

---

### 91. Snowflake supports both SQL and procedural programming languages for writing stored procedures.  
<details>**Answer**: True.</details>

---

### 92. Snowflake has built-in support for Apache Kafka integration.  
<details>**Answer**: False. Snowflake requires connectors for Kafka integration.</details>

---

### 93. Snowflake virtual warehouses can be resized dynamically based on workload demand.  
<details>**Answer**: True.</details>

---

### 94. Snowflake only supports direct access to data stored in Amazon S3.  
<details>**Answer**: False. Snowflake supports data storage in Amazon S3, Google Cloud Storage, and Microsoft Azure.</details>

---

### 95. Snowflake allows users to define custom functions in both SQL and JavaScript.  
<details>**Answer**: True.</details>

---

### 96. Snowflake’s virtual warehouses are used to manage compute resources for query execution.  
<details>**Answer**: True.</details>

---

### 97. Snowflake supports storing data in a non-relational format for flexibility.  
<details>**Answer**: True. Snowflake supports semi-structured data formats like JSON, Avro, etc.</details>

---

### 98. Snowflake only supports data sharing within the same region.  
<details>**Answer**: False. Snowflake supports cross-region data sharing.</details>

---

### 99. Snowflake's built-in security features include encryption, access control, and audit logging.  
<details>**Answer**: True.</details>

---

### 100. Snowflake’s compute and storage are tightly coupled, meaning they cannot be scaled independently.  
<details>**Answer**: False. Compute and storage in Snowflake are decoupled and can be scaled independently.</details>

---







