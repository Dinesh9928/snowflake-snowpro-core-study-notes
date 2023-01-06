# Data Protection #

> [Continuous Data Protection](https://docs.snowflake.com/en/user-guide/data-cdp.html)

## Encryption at Rest and in Transit ##
* Transit is over HTTPS using TLS 1.2 SSL
* At rest data is encrypted using AES 128 or AES 256 encryption keys
* [Hierarchical encryption key model](https://docs.snowflake.com/en/user-guide/security-encryption-manage.html#hierarchical-key-model)
  * Encryption at every level: Root > Account > Table > Micro-Partition
* Automatic key rotation every 30 days
* Enterprise Edition and above can enable automatic annual re-keying
* Tri-Secret Secure or Bring Your Own Key
  * uses a composite master key which is a composite of a customer-maintained key with a Snowflake-managed key
  * the customer-maintained key must be available at all times for continuous access
* Staging Encryption
  * In Snowflake-managed internal stages, Snowflake manages encryption
  * Customers can also choose to manage their own cloud storage stages where they would need to secure and encrypt the data prior to loading into Snowflake

## Continuous Availability ##
* Cloud providers replicate Snowflake storage across at least three availability zones within each cloud region where Snowflake is deployed
  * Each availability zone consists of multiple data centers which are:
    * geographically separated
    * have their own security
    * are on separate power grids
    * have their own individual power backups
* This allows fully transparent online updates and patches with no maintenance downtime windows

## Snowflake-specific Data Security Features ##

### Time Travel ###

> [Understanding & Using Time Travel](https://docs.snowflake.com/en/user-guide/data-time-travel.html)

Access historical data at any point of time within a defined retention time period. Allows querying/rollback to a previous state of a table/schema/DB within the retention period.

* Maximum retention period can be configured per account, DB, schema and/or table by setting `DATA_RETENTION_TIME_IN_DAYS`:
  * Standard Edition - up to 1 day
  * Enterprise Edition and higher - up to 90 days
  * The default retention time is 1 day in all editions unless explicitly changed
  * Can be disabled by setting `DATA_RETENTION_TIME_IN_DAYS = 0`
  * To view current settings, use [SHOW PARAMETERS](https://docs.snowflake.com/en/sql-reference/sql/show-parameters.html)
* Allows fixing common mistakes
  * `UNDROP` a DB/Schema/Table
  * Access the previous state of a table with [AT or BEFORE](https://docs.snowflake.com/en/sql-reference/constructs/at-before.html)
    * Query the data in its previous state
    * Clone a DB/Schema/Table from its preivous state 
* Adds to storage costs for maintaining the additional historical data 

### Zero-Copy Cloning ###
Enables taking a quick snapshot of a DB/schema/table.

* Zero-copy cloning is a metadata-only operation.
* Originally, both the original and the clone's objects are pointing to the same micro-partitions
* No data is copied, so no additional storage costs are incurred until data is changed in the original or in the clone
* Usage
  * often used to quickly spin up a Dev or Test environment as a zero-copy clone of Production
  * can be used to create backups or data snapshots at given points of time
  * can be used to create read-only, read-only snapshots for reporting as of a given point of time, e.g.: month-, year-end, etc.
* When cloning a database or a schema, internal stages are not cloned

### Fail-Safe ###
Non-configurable, 7-day retention for historical data in after the Time Travel expiration
* Only accessible by Snowflake personnel
* Fail-safe cannot be disabled
* Admins can view Fail Safe storage use in the Web UI under `Account > Billing & Usage`
* Available only for Permanent tables. Not supported for Temporary, Transient or External tables

### Snowflake replication features ###
Snowflake provides customers with additional replication features, over and above the cloud providers' replication across availability zones.