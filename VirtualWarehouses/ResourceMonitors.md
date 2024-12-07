# Resource Monitor #
> [Resource Monitor](https://docs.snowflake.com/en/user-guide/resource-monitors.html)

To help control costs and avoid unexpected credit usage caused by running warehouses, Snowflake provides resource monitors. A resource monitor can be used to monitor credit usage by virtual warehouses and the cloud services needed to support those warehouses.
* Limits can be set for a specified interval or date range. When these limits are reached and/or are approaching, the resource monitor can trigger various actions, such as sending alert notifications and/or suspending user-managed warehouses.
* Resource Monitors can only be created by account administrators (i.e. users with the ACCOUNTADMIN role)
* Account administrators can choose to grant users with other roles the `MONITOR & MODIFY` grants to view and modify Resource Monitors
* Resource Monitors can be set at two different Monitor Levels:
  * Account Level: a single Monitor can be set to control credit usage for all warehouses in your account.
  * Warehouse Level: a monitor can be assigned to one or more warehouses, controlling the credit usage for each warehouse
  * NOTE: A warehouse can only be assigned to a single Resource Monitor below the account level
* The used credits for a resource monitor reflects the sum of credits consumed by all assigned warehouses within the specified interval, along with the cloud services used to support those warehouses during the same interval.
![](../images/ResourceMonitor.png)
* Resource Monitors can be replicated when replicating Databases

## Configuring Resource Monitors ##
We need to specify several parameters when creating a Resource Monitor:
* Credit Quota: specifies the number of Snowflake credits allocated to the monitor for the specified frequency interval.
  * Any number can be specified
  * Snowflake tracks the used credits/quota within the specified frequency interval by all warehouses assigned to the monitor
  * At the specified interval, this number resets back to 0
* Monitor Level: specifies whether the resource monitor is used to monitor the credit usage for your entire Account (i.e. all warehouses in the account) or a specific set of individual warehouses.
* Schedule: Consists of several properties
  * Start: Date and time (i.e. timestamp) when the resource monitor starts monitoring the assigned warehouses. Supported values:
    * Immediately (i.e. current timestamp)
    * Later (i.e. any future timestamp)
    * Note, however, that regardless of the time specified in the start date and time, resource monitors reset at 12:00 AM UTC.
  * Frequency: The interval at which the used credits reset relative to the specified start date. Supported values:
    * Daily
    * Weekly
    * Monthly
    * Yearly
    * Never (used credits never reset; assigned warehouses continue using credits until the credit quota is reached)
  * End: Date and time (i.e. timestamp) when Snowflake suspends the warehouses associated with the resource monitor, regardless of whether the used credits reached any of the thresholds defined for the resource monitor 
  * Actions/Triggers: the action to perform if a given threshold is reached within the specified interval
    * Thresholds are usually expressed as a percentage of the credit quota
    * Notification are sent to all account administrators who have enabled receipt of notifications and non-administrator users in the notification list
    * Resource monitors support the following actions:
      * Notify & Suspend: send a notification and suspend all assigned warehouses after all queries in flight have completed.
      * Notify & Suspend Immediately: send a notification and suspend all assigned warehouses immediately, which cancels any queries in flight.
      * Notify: Perform no action, but send an alert notification

```
USE ROLE accountadmin;

CREATE OR REPLACE RESOURCE MONITOR limit1 WITH CREDIT_QUOT=2000
    FREQUNCY = WEEKLY
    START_TIMESTAMP = '2019-03-04 00:00 PST'
    TRIGGERS ON 80 PERCENT DO SUSPEND
             ON 100  PERCENT DO SUSPEND_IMMEDIATE;

ALTER WAREHOUSE wh1 SET RESOURCE_MONITOR = limit1;
ALTER WAREHOUSE wh2 SET RESOURCE_MONITOR = limit1;
```

## Warehouse Suspension and Resumption ##
If a Resource Monitor suspends a Warehouse, it cannot be restarted unless one of these conditions are met:
* The next interval, if any, starts, as dictated by the start date for the monitor. 
* The credit quota for the monitor is increased. 
* The credit threshold for the suspend action is increased. 
* The warehouses are no longer assigned to the monitor. 
* The monitor is dropped.

##MCQs

**1. Who can create a Resource Monitor in Snowflake?**  
- A. Any user with the appropriate grants  
- B. Only account administrators  
- C. Users with the MONITOR & MODIFY privilege  
- D. Role-based access control (RBAC) enabled users  
<details><summary>Answer</summary> B. Only account administrators</details>

**2. What happens when the credit limit of a Resource Monitor is reached?**  
- A. All warehouses are deleted  
- B. Queries in flight are completed, and assigned warehouses are suspended  
- C. All assigned warehouses stop immediately, canceling any in-flight queries  
- D. Snowflake sends an alert but no action is taken  
<details><summary>Answer</summary> B. Queries in flight are completed, and assigned warehouses are suspended</details>

**3. Which monitor level allows monitoring credit usage for all warehouses in an account?**  
- A. Warehouse Level  
- B. Instance Level  
- C. Account Level  
- D. Cloud Level  
<details><summary>Answer</summary> C. Account Level</details>

**4. What is the default start time for a Resource Monitor schedule?**  
- A. 12:00 AM UTC  
- B. 12:00 PM UTC  
- C. Configurable by the user  
- D. Timezone-dependent  
<details><summary>Answer</summary> A. 12:00 AM UTC</details>

**5. What frequency option ensures used credits never reset?**  
- A. Never  
- B. Monthly  
- C. Yearly  
- D. Unlimited  
<details><summary>Answer</summary> A. Never</details>

**6. Which of the following actions can a Resource Monitor trigger?**  
- A. Notify only  
- B. Notify & Suspend  
- C. Notify & Suspend Immediately  
- D. All of the above  
<details><summary>Answer</summary> D. All of the above</details>

**7. What must happen to restart a warehouse suspended by a Resource Monitor?**  
- A. The monitor’s credit quota is increased  
- B. The credit threshold is increased  
- C. The warehouse is unassigned from the monitor  
- D. Any of the above conditions are met  
<details><summary>Answer</summary> D. Any of the above conditions are met</details>

**8. How are notifications sent when thresholds are reached?**  
- A. Only to the account administrator  
- B. To all administrators with enabled notifications  
- C. To all Snowflake users  
- D. Notifications are not supported  
<details><summary>Answer</summary> B. To all administrators with enabled notifications</details>

**9. How often do Resource Monitors reset credits for a weekly frequency?**  
- A. Every 7 days  
- B. Every Monday  
- C. Every Sunday  
- D. Based on the start timestamp  
<details><summary>Answer</summary> D. Based on the start timestamp</details>

**10. What happens if no action is specified for a threshold?**  
- A. Warehouses are suspended  
- B. Warehouses continue operating normally  
- C. Alerts are automatically sent  
- D. All associated queries are canceled  
<details><summary>Answer</summary> B. Warehouses continue operating normally</details>

**11. What does the ‘Notify & Suspend Immediately’ action do?**  
- A. Suspends warehouses after completing in-flight queries  
- B. Suspends warehouses immediately, canceling in-flight queries  
- C. Suspends warehouses after a 5-minute buffer period  
- D. Sends an alert but does not suspend any warehouse  
<details><summary>Answer</summary> B. Suspends warehouses immediately, canceling in-flight queries</details>

**12. How can Resource Monitors be replicated?**  
- A. Using Snowflake replication for Databases  
- B. Using a SQL script  
- C. Manually through the UI  
- D. Resource Monitors cannot be replicated  
<details><summary>Answer</summary> A. Using Snowflake replication for Databases</details>

**13. What happens to used credits at the end of the interval?**  
- A. They carry over to the next interval  
- B. They reset to 0  
- C. They are added to the quota  
- D. They remain as a log entry  
<details><summary>Answer</summary> B. They reset to 0</details>

**14. How is the credit quota tracked within an interval?**  
- A. As the sum of credits consumed by assigned warehouses  
- B. Based on daily snapshots  
- C. Using the average credit usage  
- D. Based on the warehouse runtime only  
<details><summary>Answer</summary> A. As the sum of credits consumed by assigned warehouses</details>

**15. What happens when the end date of a Resource Monitor is reached?**  
- A. Monitoring stops, and no further alerts are sent  
- B. All associated warehouses are suspended  
- C. Credits are refunded  
- D. The monitor continues until manually dropped  
<details><summary>Answer</summary> B. All associated warehouses are suspended</details>

**16. Which parameter is used to allocate Snowflake credits to a Resource Monitor?**  
- A. Monitor Quota  
- B. Credit Limit  
- C. Credit Quota  
- D. Credit Allocation  
<details><summary>Answer</summary> C. Credit Quota</details>

**17. What does the `MONITOR & MODIFY` grant enable for Resource Monitors?**  
- A. The ability to create new Resource Monitors  
- B. The ability to delete Resource Monitors  
- C. The ability to view and modify existing Resource Monitors  
- D. The ability to adjust warehouse credits  
<details><summary>Answer</summary> C. The ability to view and modify existing Resource Monitors</details>

**18. What happens if the start date for a Resource Monitor is set in the future?**  
- A. The monitor starts at the specified date and time  
- B. The monitor starts immediately  
- C. The start date is ignored, and monitoring is disabled  
- D. The monitor starts with an end date override  
<details><summary>Answer</summary> A. The monitor starts at the specified date and time</details>

**19. What is the minimum supported auto-suspend time for a warehouse assigned to a Resource Monitor?**  
- A. 5 minutes  
- B. 10 minutes  
- C. 1 minute  
- D. 15 minutes  
<details><summary>Answer</summary> C. 1 minute</details>

**20. Which of the following frequencies can be set for Resource Monitor intervals?**  
- A. Hourly  
- B. Monthly  
- C. Quarterly  
- D. Never  
<details><summary>Answer</summary> B. Monthly, D. Never</details>

**21. What SQL command is used to assign a warehouse to a Resource Monitor?**  
- A. `SET MONITOR`  
- B. `ALTER WAREHOUSE`  
- C. `CREATE MONITOR`  
- D. `ASSIGN RESOURCE`  
<details><summary>Answer</summary> B. `ALTER WAREHOUSE`</details>

**22. What is the default frequency when creating a Resource Monitor without specifying a frequency?**  
- A. Daily  
- B. Weekly  
- C. Never  
- D. Monthly  
<details><summary>Answer</summary> C. Never</details>

**23. How are credits calculated for a Resource Monitor at the warehouse level?**  
- A. Based on total runtime of the warehouse cluster  
- B. As a percentage of account-level usage  
- C. By summing credits consumed by all assigned warehouses  
- D. By dividing total credits by active clusters  
<details><summary>Answer</summary> C. By summing credits consumed by all assigned warehouses</details>

**24. Which trigger action sends an alert without affecting the warehouse's operation?**  
- A. Notify  
- B. Notify & Suspend  
- C. Notify & Suspend Immediately  
- D. Alert Only  
<details><summary>Answer</summary> A. Notify</details>

**25. What determines whether a warehouse can resume after being suspended by a Resource Monitor?**  
- A. Adjusting the monitor's schedule  
- B. Modifying the monitor's credit quota  
- C. Removing the warehouse from the monitor  
- D. Any of the above  
<details><summary>Answer</summary> D. Any of the above</details>

**26. What happens to credits used by a suspended warehouse in a Resource Monitor?**  
- A. They are refunded  
- B. They are added to the next interval's quota  
- C. They remain counted in the current interval  
- D. They are divided across all other active warehouses  
<details><summary>Answer</summary> C. They remain counted in the current interval</details>

**27. Can a Resource Monitor suspend a warehouse without notifying users?**  
- A. Yes, if explicitly configured  
- B. No, notification is mandatory  
- C. Only if the credit threshold is 100%  
- D. Only if there are no administrators in the notification list  
<details><summary>Answer</summary> A. Yes, if explicitly configured</details>

**28. How can a Resource Monitor action be modified?**  
- A. By dropping and recreating the monitor  
- B. Using the `ALTER RESOURCE MONITOR` command  
- C. By editing the configuration in the web UI  
- D. Actions cannot be modified once created  
<details><summary>Answer</summary> B. Using the `ALTER RESOURCE MONITOR` command</details>

**29. What is the default suspension behavior for a warehouse when the credit quota reaches 100%?**  
- A. Notify only  
- B. Suspend immediately  
- C. Suspend after completing in-flight queries  
- D. Depends on the specified trigger action  
<details><summary>Answer</summary> D. Depends on the specified trigger action</details>

**30. Can a warehouse have more than one Resource Monitor assigned at the same time?**  
- A. Yes, but only for different monitor levels  
- B. No, only one monitor can be assigned per warehouse  
- C. Yes, if the monitors have overlapping quotas  
- D. No, unless specified during creation  
<details><summary>Answer</summary> B. No, only one monitor can be assigned per warehouse</details>

### 1. Warehouse Suspension Scenario
A company sets up a resource monitor with the following configuration:
- Credit Quota: 500 credits
- Frequency: Monthly
- Triggers:  
  - Notify at 80%  
  - Suspend at 100%  

If the warehouse usage reaches 400 credits mid-month, what will happen?  
- A. The warehouse is suspended immediately.  
- B. A notification is sent to account administrators.  
- C. Nothing happens until 500 credits are used.  
- D. Usage is reset automatically to 0.  

<details><summary>Answer</summary> B. A notification is sent to account administrators.</details>

---

### 2. Threshold Configuration
A company configures a resource monitor with:  
- Start: Immediately  
- Frequency: Weekly  
- Triggers:  
  - Notify at 50%  
  - Suspend immediately at 90%  

What happens if the quota is set to 1,000 credits, and the warehouse consumes 450 credits in the first three days?  
- A. Nothing happens because it hasn’t reached 50%.  
- B. A notification is sent to account administrators.  
- C. All warehouses are suspended after a warning.  
- D. Usage resets after three days.  

<details><summary>Answer</summary> B. A notification is sent to account administrators.</details>

---

### 3. Replication Use Case
When replicating a database and its objects, how does Snowflake handle associated resource monitors?  
- A. Resource monitors are not replicated.  
- B. Resource monitors are replicated with databases.  
- C. Resource monitors are only replicated if manually specified.  
- D. Resource monitors can’t be replicated between cloud providers.  

<details><summary>Answer</summary> B. Resource monitors are replicated with databases.</details>

---

### 4. Dynamic Scheduling
A monitor is created with:  
- Frequency: Weekly  
- Start Timestamp: `2024-12-01 00:00 UTC`  
- Trigger: Suspend at 90%

What will happen at `2024-12-02 00:00 UTC` if the warehouse has consumed 950 credits?  
- A. No action will be taken.  
- B. The warehouse will be suspended immediately.  
- C. A notification will be sent to administrators.  
- D. The quota will be reset automatically.  

<details><summary>Answer</summary> B. The warehouse will be suspended immediately.</details>

---

### 5. Resource Monitor Configuration
Which of the following actions can be performed once a resource monitor reaches its threshold?  
- A. Only notifications can be triggered.  
- B. Notifications and suspensions can be triggered based on thresholds.  
- C. Only suspension is allowed.  
- D. No actions can be triggered after reaching the threshold.  

<details><summary>Answer</summary> B. Notifications and suspensions can be triggered based on thresholds.</details>

### 6. Resource Monitor Levels
Which of the following is true about the resource monitor levels in Snowflake?  
- A. A resource monitor can only be applied at the account level.  
- B. A warehouse can only be assigned to a single resource monitor below the account level.  
- C. Resource monitors can only be created at the warehouse level.  
- D. Account administrators can create resource monitors only at the warehouse level.  

<details><summary>Answer</summary> B. A warehouse can only be assigned to a single resource monitor below the account level.</details>

---

### 7. Credit Quota Behavior
What happens when a resource monitor's credit quota is exceeded?  
- A. The warehouse continues running until the end of the monitoring period.  
- B. The warehouse is automatically suspended based on the specified action.  
- C. Snowflake automatically doubles the credit quota.  
- D. Snowflake issues an alert, but the warehouse continues operating.  

<details><summary>Answer</summary> B. The warehouse is automatically suspended based on the specified action.</details>

---

### 8. Resource Monitor Frequency
Which of the following frequencies is NOT a valid option for a resource monitor's schedule?  
- A. Hourly  
- B. Daily  
- C. Weekly  
- D. Monthly  

<details><summary>Answer</summary> A. Hourly</details>

---

### 9. Monitoring Period Behavior
If the monitoring period ends without reaching the quota, what happens?  
- A. The quota resets and a new period starts.  
- B. The warehouse is suspended immediately.  
- C. The monitor continues to accumulate credits until the next period.  
- D. The credits are refunded.  

<details><summary>Answer</summary> A. The quota resets and a new period starts.</details>

---

### 10. Resource Monitor Suspension Action
When a resource monitor triggers a suspension, which of the following is true?  
- A. The warehouse is suspended immediately, terminating all queries in flight.  
- B. The warehouse is suspended only after all queries in flight are completed.  
- C. The warehouse continues running until manually suspended.  
- D. The warehouse is suspended after 24 hours.  

<details><summary>Answer</summary> B. The warehouse is suspended only after all queries in flight are completed.</details>

### 11. Resource Monitor Notification
Which of the following is true about the notification actions for resource monitors?  
- A. Notifications are sent only to account administrators.  
- B. Notifications are sent to all users who have been added to the notification list.  
- C. Notifications are sent only when the warehouse is suspended.  
- D. Notifications are not supported in resource monitors.  

<details><summary>Answer</summary> B. Notifications are sent to all users who have been added to the notification list.</details>

---

### 12. Resource Monitor Scheduling - Start Date
Which of the following is true about the start timestamp for resource monitors?  
- A. The start date must always be set in the future.  
- B. The monitor will always start at 12:00 AM UTC.  
- C. The start timestamp can be set to any time, including immediately.  
- D. The start date is optional, and the monitor starts immediately.  

<details><summary>Answer</summary> C. The start timestamp can be set to any time, including immediately.</details>

---

### 13. Resource Monitor Actions
What action does a resource monitor perform if it reaches 100% of the credit quota?  
- A. It automatically suspends all warehouses.  
- B. It sends an alert but continues running the warehouses.  
- C. It suspends all warehouses immediately, canceling queries in flight.  
- D. It reduces the credit quota by 50%.  

<details><summary>Answer</summary> C. It suspends all warehouses immediately, canceling queries in flight.</details>

---

### 14. Creating Resource Monitors
Who can create and modify resource monitors?  
- A. Any user with the MONITOR role.  
- B. Only account administrators.  
- C. Only users with the RESOURCE_MONITOR role.  
- D. Any user with sufficient permissions in the web UI.  

<details><summary>Answer</summary> B. Only account administrators.</details>

---

### 15. Resource Monitor Credit Quota
If a resource monitor's credit quota is exhausted, which action is taken?  
- A. The warehouse is suspended according to the specified action.  
- B. The warehouse continues to run but incurs additional charges.  
- C. The quota is automatically reset, and the warehouse continues running.  
- D. Snowflake issues a warning and continues billing.  

<details><summary>Answer</summary> A. The warehouse is suspended according to the specified action.</details>

### 1. What is the purpose of a Resource Monitor in Snowflake?
- A. To monitor credit usage by virtual warehouses
- B. To track data storage costs
- C. To optimize query performance
- D. To control data transfers between regions
<details><summary>Answer</summary> A. To monitor credit usage by virtual warehouses</details>

### 2. Who can create a Resource Monitor in Snowflake?
- A. Any user with the `MONITOR` role
- B. Any user with the `ACCOUNTADMIN` role
- C. Any user with the `SYSADMIN` role
- D. Only the owner of the virtual warehouse
<details><summary>Answer</summary> B. Any user with the `ACCOUNTADMIN` role</details>

### 3. Which of the following actions can be triggered by a Resource Monitor in Snowflake?
- A. Notify & Suspend
- B. Notify & Suspend Immediately
- C. Notify Only
- D. All of the above
<details><summary>Answer</summary> D. All of the above</details>

### 4. What is the default behavior of a Resource Monitor when the credit quota is reached?
- A. Automatically scale up the warehouse
- B. Automatically suspend the warehouse
- C. No action is taken
- D. Send a notification and suspend the warehouse after queries are completed
<details><summary>Answer</summary> D. Send a notification and suspend the warehouse after queries are completed</details>

### 5. How often can the credit quota for a Resource Monitor be reset?
- A. Daily
- B. Weekly
- C. Monthly
- D. Based on frequency set during the configuration
<details><summary>Answer</summary> D. Based on frequency set during the configuration</details>

### 6. Which SQL command is used to assign a Resource Monitor to a virtual warehouse in Snowflake?
- A. `CREATE RESOURCE MONITOR`
- B. `ALTER WAREHOUSE`
- C. `GRANT RESOURCE MONITOR`
- D. `ASSIGN RESOURCE MONITOR`
<details><summary>Answer</summary> B. `ALTER WAREHOUSE`</details>

### 7. Which of the following is true about Resource Monitors?
- A. They can only monitor one warehouse at a time
- B. They can only be assigned at the account level
- C. They can trigger actions like suspending warehouses
- D. They require a SQL script to be created in Snowflake
<details><summary>Answer</summary> C. They can trigger actions like suspending warehouses</details>

### 8. What happens if a warehouse is suspended by a Resource Monitor?
- A. The warehouse automatically resumes after 1 hour
- B. The warehouse remains suspended until the next interval or quota increase
- C. The warehouse is deleted
- D. The warehouse performs automatic scaling
<details><summary>Answer</summary> B. The warehouse remains suspended until the next interval or quota increase</details>

### 9. Which of the following is NOT a valid frequency for Resource Monitor credit resets?
- A. Daily
- B. Weekly
- C. Monthly
- D. Continuous
<details><summary>Answer</summary> D. Continuous</details>

### 10. What does the `CREDIT_QUOTA` parameter in a Resource Monitor define?
- A. The maximum number of queries allowed
- B. The number of credits that can be consumed within the specified interval
- C. The storage limit for each warehouse
- D. The frequency at which the monitor resets
<details><summary>Answer</summary> B. The number of credits that can be consumed within the specified interval</details>

### 11. Which of the following is true about the `START_TIMESTAMP` parameter when creating a Resource Monitor?
- A. It is the time when the resource monitor starts tracking credit usage.
- B. It must be a past timestamp for monitoring to begin.
- C. It only affects the frequency of credit resets.
- D. It defines when the credit quota is increased.
<details><summary>Answer</summary> A. It is the time when the resource monitor starts tracking credit usage.</details>

### 12. What does the `MONITOR LEVEL` parameter in a Resource Monitor determine?
- A. The frequency of credit quota resets
- B. The scope of the credit usage monitoring (account or warehouse level)
- C. The number of warehouses to monitor
- D. The size of the warehouse being monitored
<details><summary>Answer</summary> B. The scope of the credit usage monitoring (account or warehouse level)</details>

### 13. What happens when the credit quota is exceeded by a monitored warehouse?
- A. The warehouse is resized to a larger size
- B. The warehouse is suspended and notifications are sent
- C. The warehouse automatically scales out
- D. The warehouse continues to use credits without interruption
<details><summary>Answer</summary> B. The warehouse is suspended and notifications are sent</details>

### 14. Which of the following is NOT a valid action for a Resource Monitor trigger?
- A. Notify only
- B. Suspend and resume immediately
- C. Notify & Suspend after completion of queries
- D. Notify & Suspend Immediately
<details><summary>Answer</summary> B. Suspend and resume immediately</details>

### 15. What is the default action when a Resource Monitor reaches the credit quota?
- A. Scale out the warehouse
- B. No action, continue processing
- C. Notify and suspend after completing in-flight queries
- D. Terminate all queries immediately
<details><summary>Answer</summary> C. Notify and suspend after completing in-flight queries</details>

### 16. What does the `FREQUENCY` parameter in the Resource Monitor configuration specify?
- A. How often the credit usage is billed
- B. How often the credit quota is checked and reset
- C. How often warehouses are suspended
- D. How frequently queries are logged
<details><summary>Answer</summary> B. How often the credit quota is checked and reset</details>

### 17. How can a warehouse be reactivated if suspended by a Resource Monitor?
- A. By increasing the credit quota for the monitor
- B. By deleting the Resource Monitor
- C. By manually resuming the warehouse using SQL
- D. All of the above
<details><summary>Answer</summary> D. All of the above</details>

### 18. What happens when a Resource Monitor is assigned at the warehouse level?
- A. It monitors credit usage for individual warehouses only
- B. It monitors credit usage across the entire account
- C. It suspends all warehouses in the account when the quota is exceeded
- D. It automatically resizes the warehouse based on usage
<details><summary>Answer</summary> A. It monitors credit usage for individual warehouses only</details>

### 19. Which command is used to modify an existing Resource Monitor in Snowflake?
- A. `ALTER RESOURCE MONITOR`
- B. `UPDATE RESOURCE MONITOR`
- C. `CREATE RESOURCE MONITOR`
- D. `MODIFY RESOURCE MONITOR`
<details><summary>Answer</summary> A. `ALTER RESOURCE MONITOR`</details>

### 20. When using the `NOTIFY & SUSPEND` action, when will the warehouses be suspended?
- A. Immediately, without finishing running queries
- B. After all queries in-flight are completed
- C. Only after a scheduled time
- D. After the next reset interval
<details><summary>Answer</summary> B. After all queries in-flight are completed</details>


