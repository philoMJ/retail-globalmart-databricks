# Slowly Changing Dimensions (SCD) in Data Warehousing

## Overview
Slowly Changing Dimensions (SCD) is a data modeling technique used in analytical data warehouses (OLAP) to manage and preserve historical data over time as attributes change. 

## The Golden Rule of SCD and Normalization
⚠️ **Crucial Architectural Note:** Adding SCD (especially Type 2, 3, and 4) means **strict normalization is lost**. Under traditional Third Normal Form (3NF), data must not be redundant. However, to track history in a dimension table, we intentionally duplicate keys and descriptive attributes. We accept this "controlled denormalization" in the **Silver Layer** of a Lakehouse or the dimension tables of a Star Schema to prevent massive historical data loss!

---

## All Types of SCD with Examples
Let's use a standard retail customer moving from **New York** to **Miami** to demonstrate how different SCD types handle physical attribute changes.

### SCD Type 0: Retain Original (No Changes Allowed)
* **What it does:** Attributes are never updated. Once the data is written, it is locked forever.
* **Best used for:** Fixed, immutable data like Date of Birth, Original Enrollment Dates, or unchanging hardware IDs.
* **Example:** 
  * *Original:* `Cust_ID: C101 | Name: John Doe | DOB: 1990-05-15`
  * *Action:* John tries to correct his DOB to 1990-05-16.
  * *Result:* The system ignores or rejects the change. The row remains exactly as it was.

### SCD Type 1: Overwrite (No History)
* **What it does:** The new data simply overwrites the old data in the database. No history of the previous value is kept.
* **Best used for:** Correcting spelling errors or tracking non-critical data where history doesn't matter (e.g., a phone number).
* **Example:**
  * *Before:* `Cust_ID: C101 | Name: John Doe | City: New York`
  * *After:* `Cust_ID: C101 | Name: John Doe | City: Miami` (New York is gone forever).

### SCD Type 2: Add New Row (Full Historical Tracking)
* **What it does:** It creates a brand new row with a new surrogate key for every change. This is the industry standard for full history.
* **Best used for:** Core business metrics where point-in-time accuracy matters (e.g., calculating tax based on where a customer lived at the time of purchase).
* **Example:**
  * *State 1:* 

    | Customer_SK | Cust_ID | City | Is_Current | Start_Date | End_Date |
    | :--- | :--- | :--- | :--- | :--- | :--- |
    | 1 | C101 | New York | Y | 2026-01-01 | NULL |
  * *State 2 (After John moves):*

    | Customer_SK | Cust_ID | City | Is_Current | Start_Date | End_Date |
    | :--- | :--- | :--- | :--- | :--- | :--- |
    | 1 | C101 | New York | N | 2026-01-01 | 2026-04-01 |
    | **2** | C101 | **Miami** | **Y** | **2026-04-01** | NULL |

### SCD Type 3: Add New Column (Current and Previous Value)
* **What it does:** It keeps the current value and the immediately previous value side-by-side in columns on the same row.
* **Best used for:** When you only need to compare the "before and after" of a specific soft transition (like a territory realignment).
* **Example:**
  * *Before:* `Cust_ID: C101 | Current_City: New York | Previous_City: NULL`
  * *After:* `Cust_ID: C101 | Current_City: Miami | Previous_City: New York`

### SCD Type 4: History Table (Mini-Dimension)
* **What it does:** The main table only holds the current data (acting like Type 1), but every change is logged in a separate, dedicated "History" table.
* **Best used for:** High-frequency changing dimensions (rapid updates) to keep the main table small and fast.
* **Example:**
  * *Main Table:* `Cust_ID: C101 | City: Miami`
  * *History Table:*

    | History_ID | Cust_ID | City | Valid_From | Valid_To |
    | :--- | :--- | :--- | :--- | :--- |
    | 1 | C101 | New York | 2026-01-01 | 2026-04-01 |

### SCD Type 6: The Hybrid (Type 1 + Type 2 + Type 3)
* **What it does:** It combines the row-history of Type 2, the column-history of Type 3, and overwrites a master current column like Type 1 ($1 + 2 + 3 = 6$).
