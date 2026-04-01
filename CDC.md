# Change Data Capture (CDC) in Data Engineering

## Overview
Change Data Capture (CDC) is a design pattern that captures inserts, updates, and deletes applied to a source database (OLTP) and pushes those changes to a target destination (like a Data Warehouse or Lakehouse Bronze layer).

## The "City Update" Collision Problem
If you rely on nightly batch copies of a database, you will experience severe data collisions:
1. **10:00 AM:** John Doe buys an item while living in **New York**.
2. **02:00 PM:** John Doe updates his profile address to **Miami**.
3. **12:00 AM (Midnight):** The batch ETL runs. It sees the order and joins it to John's current profile (Miami). 
* **Result:** The database falsely claims the order was made from Miami. 

## How CDC Solves This
CDC reads the database's native transaction logs (like PostgreSQL's WAL or MySQL's Binlog) and captures every single state change chronologically as it happens. 

## CDC Approaches & Trade-offs
### 1. Real-Time Streaming CDC
* **How it works:** Tools like Debezium or Fivetran listen to the log and stream changes instantly using a bus like Kafka.
* **Pros:** Zero latency, highly accurate point-in-time state.
* **Cons:** Expensive. Requires 24/7 active compute clusters and heavy maintenance.

### 2. Daily Log-Based Batch CDC
* **How it works:** The system extracts full transaction log files once a day at night.
* **Pros:** Highly cost-effective; prevents 24/7 server costs.
* **Cons:** Causes heavy data spikes on the source database at midnight. Your analytical data is always 24 hours behind.

### 3. Micro-Batch CDC (Every 6 Hours)
* **How it works:** Schedulers wake up a serverless job every 6 hours to pull targeted log files using incremental watermarks.
* **Pros:** The perfect architectural middle ground! It flattens compute spikes, costs very little because compute shuts down after 5 minutes, and keeps data reasonably fresh for BI reporting.

### Not: What's a transactional log?
Because the log records operations rather than current states, a single log file read by a CDC tool looks like a stream of ledger entries:

| Log Sequence No. | Timestamp | Operation | Table | Primary Key | Before Image | After Image |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 100451 | 10:00:01 AM | `INSERT` | Orders | Ord_99 | NULL | `Cust: C101, Qty: 2` |
| 100452 | 02:00:05 PM | `UPDATE` | Customers| C101 | `City: New York` | `City: Miami` |
| 100453 | 04:15:20 PM | `DELETE` | Orders | Ord_54 | `Cust: C202, Qty: 5` | NULL |

**CDC tools do not query the tables.** Instead, they "tail" or read the **Transaction Log** directly.
