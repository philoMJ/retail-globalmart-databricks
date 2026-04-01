# ✈️ Flight Recorder: GlobalMart Retail Project
*A chronological log of technical maneuvers, mid-air repairs, and successful landings.*
> **MISSION STATUS:** IN PROGRESS  
> **OPERATOR:** philoMJ  
> **MISSION:** Ensuring all-weather delivery of retail data from source to warehouse.  
> **OBJECTIVE:** 100% cargo integrity (no data loss) and fuel-efficient flight paths (optimized ETL compute).  

---

## 🗂️ MISSION LOG SECTIONS
- [ ] **📋Pre-Flight (Setup):** Environment config, library installs, and "Day 1" architecture.
- [ ] **💹Cruising Altitude (Development):** New features, smooth wins, and logic breakthroughs.
- [ ] **🚨Emergency Procedures (Bug Fixes):** The "Mayday" moments and how you survived them. "Everything is broken and I don't know why."
- [ ] **🛠 Maintenance Performed:** "It works, but I can make it better/cleaner."
- [ ] **🛬Landing:** end of project
---

## 🛠️ LOG ENTRIES

### 📅 Stardate: 2024-03-31
#### 🛠 Maintenance Performed:
* **The Issue:** Both Product and Customer tables had issues with keys. Same prodID had more than one PdtName. Same customer had more than one address.
    *   **The Struggle:** learned star schema modelling, scd and cdc
    *   **The Fix:** implement star schema modelling, scd
    *   **Lesson:** 

#### 💹Cruising Altitude:
* LEarned CDC, SCD. Diff between data modelling (oltp) and dimensional modelling(olap)
  
### 📅 Stardate: 2024-03-30
#### 🚨 Emergency Procedure:
* **The Issue:** Schema mismatch when overwriting delta table on running the notebook.
    *   **The Fix:** Deleted and reran the bronze layer job. Decided to not add code to handle the error durng runtime.
    *   **Lesson:** Not all errors need fixing! Some are present as warrnings!

#### 💹Cruising Altitude:
* did some EDA. Created Data Model for silver layer

### 📅 Stardate: 2024-03-29
#### 📋Pre-Flight:
* Set up a development environment on Databricks Free Edition.
* Refreshed Git fundamentals. Git docs is best for this.
* Linked Databricks to Github. Configured Databricks Repos to GitHub for enable seamless version control.

#### 🚨 Emergency Procedure:
* **The Issue:** Databricks-GitHub connection failure (Authentication Error).
    *   **The Struggle:** Followed tutorials exactly, but the connection kept timing out/denying access with vague errors.
    *   **The Fix:** Checked the Personal Access Token (PAT) in GitHub settings; it had expired. Generated a new token.
    *   **Lesson:** Always check the "Expiry Date" on your credentials first. High-fidelity transport requires valid passports! 🛂

#### 💹Cruising Altitude:
* Completed Bronze Layer. data lineage and governance cols; made delta table from csv.
* Created a job for bronze layer.

---
*End of Log.*
