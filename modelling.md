# Schema Modeling: Normal Forms vs. Star & Snowflake

When designing database architectures, engineers typically choose between relational normalization for transactional applications and dimensional schemas for business intelligence.

---

## 1. Normalization Forms (Used in OLTP)
Normalization is the process of organizing data in a database to reduce data redundancy and improve data integrity.

* **First Normal Form (1NF):** Eliminate duplicate columns, ensure all attributes are atomic (no arrays or lists in a single cell), and identify a primary key.
* **Second Normal Form (2NF):** Must meet 1NF, and all non-key columns must depend on the *entire* primary key (no partial dependencies).
* **Third Normal Form (3NF):** Must meet 2NF, and all non-key columns must depend *only* on the primary key (no transitive dependencies).
  * *Example:* If you have `ZipCode` and `City` in a table, `City` depends on `ZipCode`, not the Primary Key. In 3NF, you must split them into a separate table!

---

## 2. Dimensional Modeling (Used in OLAP)
To avoid making analysts join 15 different tables to answer a single question, dimensional modeling flattens and groups data into **Facts** (metrics) and **Dimensions** (context).

### The Star Schema (Highly Denormalized)
* **Concept:** A single, central Fact Table directly connected to flattened Dimension tables.
* **Pros:** Extremely fast read performance, simple queries, and highly supported by BI tools like Power BI or Tableau.
* **Cons:** Redundant data in dimensions. For example, a `Dim_Product` table will repeat the Category Name over and over again for thousands of products.

### The Snowflake Schema (Semi-Normalized)
* **Concept:** A variation of the Star Schema where dimension tables are normalized into multiple related tables (e.g., Product table points to a Sub-Category table, which points to a Category table).
* **Pros:** Saves storage space and prevents update anomalies in the dimensions.
* **Cons:** Requires more complex multi-table `JOIN` queries, which slows down reporting performance compared to a Star schema.
