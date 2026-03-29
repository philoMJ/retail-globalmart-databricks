# ✈️ Flight Recorder: GlobalMart Retail Project
*A chronological log of technical maneuvers, mid-air repairs, and successful landings.*
> **MISSION STATUS:** IN PROGRESS  
> **OPERATOR:** philoMJ  
> **MISSION:** Ensuring all-weather delivery of retail data from source to warehouse.  
> **OBJECTIVE:** 100% cargo integrity (no data loss) and fuel-efficient flight paths (optimized ETL compute).  

---

## 🗂️ MISSION LOG SECTIONS
- [ ] **Pre-Flight (Setup):** Environment config, library installs, and "Day 1" architecture.
- [ ] **Cruising Altitude (Development):** New features, smooth wins, and logic breakthroughs.
- [ ] **Emergency Procedures (Bug Fixes):** The "Mayday" moments and how you survived them.

---

## 🛠️ LOG ENTRIES

### 📅 Stardate: 2024-05-22
#### 🛠 Maintenance Performed:
* **The Issue:** API calls were returning 404s despite the URL looking correct.
* **The Fix:** Realized I was missing the `/v1/` prefix in my environment variable. 
* **Lesson Learned:** Always `console.log` the final constructed URL before fetching.

### 📅 Stardate: 2024-05-20
#### 🚀 New Altitude Reached:
* Successfully implemented the **Auth0** login flow.
* **Takeaway:** Documentation is your best friend; don't try to wing the configuration.

---
*End of Log.*
