# 🌊🏔️ STS — Sea to Summit | Sales & Margin Analytics Dashboard

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-0078D4?style=for-the-badge&logo=microsoft&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![Status](https://img.shields.io/badge/status-completed-brightgreen?style=for-the-badge)

A multi-country **sales & margin performance dashboard** built for **Sea to Summit**, a global outdoor/travel/watersports gear distributor operating stores across multiple countries with a wide portfolio of product brands and categories.

The project simulates a real-world retail analytics pipeline — from raw, multi-source data landing in a Data Warehouse, to curated views, to interactive Power BI reports used for MTD/YTD performance tracking.

---

## 📌 Project Overview

| | |
|---|---|
| **Domain** | Retail / Distribution — Outdoor & Travel Gear |
| **Scope** | Multi-country stores, multiple brands & product categories |
| **Goal** | Track Sales & Margin performance MTD, YTD, and YoY (vs This Month/Year Last Year) |
| **Tools** | Power BI, DAX, SQL (Stored Procedures, Views) |

---

## 🗂️ Data Architecture

```
Multiple Country Data Sources
          │
          ▼
  Stored Procedure (Incremental Load)
          │
          ▼
      DW Tables
          │
          ▼
   SQL Views (per report requirement)
          │
          ▼
  Power BI Semantic Model (DAX Measures)
          │
          ▼
      Dashboards
```

- **Multi-source ingestion:** Each country's sales data lands from its own source system.
- **Incremental loading:** A stored procedure loads only new/changed records into the DW on each run — avoiding full reloads.
- **Views:** Built on top of DW tables, scoped to what each visual/report actually needs.
- **Semantic layer:** DAX measures with variables for readability, consistency, and reuse.

---

## 📊 Key Measures (DAX)

Variable-enabled DAX (`VAR ... RETURN`) is used throughout for cleaner logic and easier debugging.

- `MTD Sales`
- `MTD Sales TMLY` (This Month Last Year)
- `% Change vs TMLY`
- `MTD Margin %`
- `TY Sales by Month` / `LY Sales by Month`
- `TY Margin by Month` / `LY Margin by Month`

<details>
<summary>Sample: % Change vs TMLY</summary>

```dax
% Change vs TMLY = 
VAR CurrentMTD = [MTD Sales]
VAR LastYearMTD = [MTD Sales TMLY]
VAR PctChange =
    DIVIDE ( CurrentMTD - LastYearMTD, LastYearMTD, BLANK() )
RETURN
    PctChange
```
</details>

---

## 📈 Visuals

- **Clustered Column Chart** — This Year vs Last Year, month-wise Sales & Margin comparison
  - Legends enabled (TY vs LY series)
  - Custom tooltip page (MTD Sales, Margin %, % Change vs TMLY on hover)
  - Dynamic title reflecting the selected/comparison years
- Country, Brand, and Category level slicers
- MTD / YTD KPI cards


📸 Add your dashboard screenshots here, e.g.:
[Sales Summary](Images/Sales Summary.png)
[Sales Performance](Images/Sales Performance.png)


---

## 🧠 What This Project Demonstrates

- Designing a DW-backed reporting pipeline for multi-source, multi-country data
- Writing maintainable, variable-based DAX for time-intelligence measures (MTD, YoY)
- Building recruiter/stakeholder-friendly visuals with proper legends, tooltips, and dynamic titles
- Structuring a BI project the way it would be handed off in a real analytics team

---

## 🚀 Tech Stack

- **Power BI Desktop** — modeling & visualization
- **DAX** — measures & time intelligence
- **SQL** — stored procedures, incremental loading, views

---

## 📬 Contact

*Add your name, LinkedIn, and portfolio link here.*
