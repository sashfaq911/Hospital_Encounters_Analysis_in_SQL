<div align="center">
  
<h1 align="center"> 🏥 Hospital Analytics SQL Project </h1>

### *Maven Analytics Guided Case Study*  

</div>

<p align="center">
  <a href="#problem-statement">Problem Statement</a> •

  <a href="#objectives">Objectives</a> •
  <a href="#data-&-tools"></a>">Data & Tools</a> •
  <a href="#exploratory-analysis-steps">Exploratory Analaysis Steps</a> •
  <a href="#key-insights">Key Insights</a> •
  <a href="#what-i-learned">What I Learned</a> •
  <a href="#sql-highlights">SQL Highlights</a> • 
  <a href="#acknowledgements">Acknowledgements</a> •
  <a href="#license">License</a>
</p>

---


## 📖 Project Overview  

This project was completed as part of the **Maven Analytics “Hospital Anayltics” Guided SQL Project**, focused on analyzing a synthetic healthcare dataset to uncover meaningful business insights using SQL.  

Through this project, I developed practical experience in querying, transforming, and interpreting hospital data — a crucial capability for data analysts and scientists working at the intersection of **AI and healthcare**.  

---

## 🧩 Problem Statement  <a name="problem-statement"></a>

Hospitals manage vast volumes of patient encounter data daily. To optimize care delivery and operations, leaders need insights into:  

- How **encounter volumes** and **care types** evolve over time.  
- What proportion of visits are **inpatient, outpatient, or emergency**.  
- How many stays last **over 24 hours** vs. shorter durations.  
- Which **procedures** drive the most **volume** or **cost**.  
- How many patients are **readmitted within 30 days**, signaling care quality challenges.  

This project uses MySQL to extract, aggregate, and interpret such patterns directly from the database.  

---

## 🎯 Objectives  <a name="objectives"></a>

The analysis was divided into three core objectives:  

### **1️⃣ Encounters Overview**
- Determine annual encounter volumes.  
- Calculate encounter class mix (ambulatory, outpatient, wellness, urgent care, emergency, inpatient).  
- Identify percentage of encounters over vs. under 24 hours.  

### **2️⃣ Cost & Coverage Insights**
- Quantify encounters with **zero payer coverage**.  
- Identify **top 10 most frequent procedures** and their average base cost.  
- Identify **top 10 procedures by highest average cost**.  
- Calculate **average total claim cost** by payer.  

### **3️⃣ Patient Behavior Analysis**
- Count **unique patients admitted per quarter**.  
- Detect **readmissions within 30 days**.  
- Highlight patients with **most frequent readmissions**.  

---

## 🧠 Data & Tools  <a name="data-&-tools"></a>

**Dataset Source:** Maven Analytics  
**Database System:** MySQL  
**Dataset Name:** Hospital Patient Records
**# of Records:** 75592

**Tables Used:**  
- `encounters` – patient-level data including encounter class, dates, claim cost, and coverage.  
- `procedures` – medical procedures with base cost details.  
- `payers` – payer (insurance) identifiers and names.  

**Key SQL Concepts Applied:**  
- Aggregation: `SUM()`, `COUNT()`, `AVG()`  
- Conditional logic: `CASE WHEN`  
- Date/time operations: `YEAR()`, `QUARTER()`, `TIMESTAMPDIFF()`, `DATEDIFF()`  
- Window functions: `LEAD()` for readmission detection  
- Joins & CTEs for modular, reusable queries  

---

## 🔍 Exploratory Analysis Steps  <a name="exploratory-analysis-steps"></a>

### 🩺 Step 1: Encounters Trend Analysis  
Counted total hospital encounters by **year** and calculated encounter class distributions to understand care flow.  

### ⏱ Step 2: Length of Stay (LOS)  
Used `TIMESTAMPDIFF(HOUR, START, STOP)` to determine whether stays were **short (<24h)** or **extended (≥24h)**.  

### 💵 Step 3: Coverage Review  
Identified how many encounters had **zero payer coverage**, signaling uninsured or uncompensated cases.  

### 💉 Step 4: Cost Analysis  
Ranked procedures by **frequency** and **average base cost** to find top drivers of hospital cost and activity.  

### 👥 Step 5: Readmission Tracking  
Applied a **window function (`LEAD()`)** to calculate the gap between a patient’s discharge and their next admission, identifying readmissions within **30 days**.  

---

## 💡 Key Insights  <a name="key-insights"></a>

✨ A large share of encounters were **short-duration (<24h)**, highlighting strong outpatient activity.  
✨ **Emergency and urgent care** encounters dominated, indicating potential strain on acute care units.  
✨ A meaningful portion of visits had **no payer coverage**, pointing to financial vulnerability.  
✨ A few **high-cost procedures** accounted for a disproportionate share of total spending.  
✨ **Readmission analysis** revealed repeat-visit patients — a crucial metric for quality and performance management.  

---

## 🧮 SQL Highlights  <a name="sql-highlights"></a>

```sql
-- Annual encounter totals
SELECT YEAR(START) AS yr, COUNT(Id) AS total_encounters
FROM encounters
GROUP BY yr
ORDER BY yr;

-- Encounter class mix
SELECT YEAR(START) AS yr,
  ROUND(SUM(CASE WHEN ENCOUNTERCLASS='emergency' THEN 1 ELSE 0 END)/COUNT(*)*100,1) AS emergency_pct,
  ROUND(SUM(CASE WHEN ENCOUNTERCLASS='inpatient' THEN 1 ELSE 0 END)/COUNT(*)*100,1) AS inpatient_pct
FROM encounters
GROUP BY yr;

-- 30-day readmission detection
WITH cte AS (
  SELECT PATIENT, START, STOP,
         LEAD(START) OVER (PARTITION BY PATIENT ORDER BY START) AS next_start_date
  FROM encounters
)
SELECT COUNT(DISTINCT PATIENT) AS num_readmitted_30d
FROM cte
WHERE DATEDIFF(next_start_date, STOP) < 30;
```

---

## 📊 Visualization Ideas

If integrated into a BI dashboard (Tableau, Power BI, or Looker Studio):

📈 Encounters Over Time – line chart showing growth or decline.
🏥 Encounter Class Distribution – stacked bar to visualize care types.
💲 Zero-Coverage Rate – KPI card or gauge.
🧾 Top Procedures by Cost – dual-axis bar comparing frequency vs. cost.
💳 Avg Claim Cost by Payer – bar chart of payer-level averages.
🔁 30-Day Readmissions – trend line or cohort table.

---

## 🚀 What I Learned  <a name="what-i-learned"></a>

✅ How to design modular SQL scripts for structured healthcare analysis.
✅ How to derive operational and financial KPIs from hospital data.
✅ How to detect readmissions and compute LOS metrics using SQL logic.
✅ How SQL outputs translate directly into BI dashboards and AI-ready feature sets.

---

## 🧠 Relevance to AI & Healthcare

This project bridges the gap between data analytics and applied AI by establishing features and metrics that feed into:

- Predictive readmission risk modeling
- Length-of-stay forecasting
- Payer performance optimization
- Cost-of-care and operational efficiency models

It reflects my capability to translate domain-specific questions into data-driven insights, a critical skill for AI-driven healthcare decision systems.

---

## 📦 Project Structure

```bash
hospital-analytics-sql/
│
├── hospital_analytics_questions.sql    # Full SQL analysis file
├── README.md                           # Documentation & insights
└── /assets                             # (Optional visuals or charts)
```

---

## 🙏 Acknowledgements <a name="acknowledgements"></a>


---

## 📄 License <a name="license"></a>

This project is licensed under the **Apache License 2.0**. See the [LICENSE](./LICENSE) file for details.

---

## ❤️  Support

Contributions, issues, and suggestions are welcome!

Give a ⭐️ if you like this project!


