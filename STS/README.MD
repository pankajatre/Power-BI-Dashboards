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

The model contains 180+ DAX measures. Variable-enabled DAX (`VAR ... RETURN`) is used throughout for cleaner logic and easier debugging. Below are the key measures grouped by function.

### 📅 Time Intelligence — MTD / QTD / YTD
- `MTD Sales for Currency`, `QTD Sales for Currency`, `YTD Sales For Currency`
- `Last Year MTD Sales`, `Last Year QTD Sales`, `Last Year YTD Sales`
- `MTD Sales growth`, `QTD Sales Growth`, `YTD Sales Growth`
- `TDLY Sales` (Today vs Last Year) and `TMLY Margin` (This Month Last Year)

### 💰 Margin & Profitability
- `Profit for currency`, `MTD Margin`, `QTD Margin`, `YTD Margin`
- `MTD Profit Margin %`, `QTD Profit Margin %`, `YTD Profit Margin %`
- `MTD Margin (%) Tile` / `QTD Margin (%) Tile` / `YTD Margin (%) Tile` — formatted KPI-card measures combining a value and a % in one string

### 🌍 Multi-Currency Conversion
- `Cost Conv`, `Total Orders Using Currency Filter`, `Discount Value for Currency`
- Dynamically converts values into whatever currency is selected, by looking up the live rate from a currency-rate fact table (falls back to AUD when nothing is selected)

### 📦 Inventory & Orders
- `Total Quantity On Hand`, `Total On Hand Value`, `In transit value`, `Turns`
- `Orders MTD`, `Orders QTD`, `Orders YTD`, `Unit Orders`

### 🔁 Trailing 12-Month Rolling Analysis
- `Trailing 12 months Sales for Current month`, `Trailing 12 months Margin for Current month`, `Trailing 12 months Order for same month` (+ prior-year equivalents)
- Built using `USERELATIONSHIP` against an inactive **Previous Calendar Year** table, so rolling 12-month figures can be compared without disturbing the main date relationship

### 🌳 Dynamic Decomposition Tree
- `Matrix`, `Header`, `Sales Performance MTD for Line Chart`
- Single measures that switch between Sales / Orders / Margin / Margin % at runtime, driven by a parameter table and `SELECTEDVALUE` + `SWITCH`

<details>
<summary>Sample: MTD Sales Growth</summary>

```dax
QTD Sales Growth = 
VAR Growth = [QTD Sales for Currency] - [Last Year QTD Sales]
RETURN
IF(
    ISBLANK([Last Year QTD Sales]),
    BLANK(),
    DIVIDE(Growth, [Last Year QTD Sales], 0)
)
```
</details>

<details>
<summary>Sample: Dynamic currency conversion</summary>

```dax
Cost Conv = 
VAR To_curr = 
    IF(
        SELECTEDVALUE('Fact Currency Rate_Budget'[TO_CURRENCY]) = BLANK(),
        "AUD",
        SELECTEDVALUE('Fact Currency Rate_Budget'[TO_CURRENCY])
    )
VAR curr_rate =
    CALCULATE(
        MAX('Fact Currency Rate_Budget'[CURRENCY_RATE]),
        FILTER(ALL('Fact Currency Rate_Budget'), 'Fact Currency Rate_Budget'[TO_CURRENCY] = To_curr)
    )
VAR valuec =
    IF(
        To_curr = "AUD",
        MAX('Fact Inventory Summary'[ITEM_COST_AUD]),
        MAX('Fact Inventory Summary'[ITEM_COST_AUD]) * curr_rate * 1.00000
    )
RETURN
    valuec
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

### 📸 Screenshots

**Daily Sales**
![Daily Sales](Images/Daily%20Sales.png)

**Sales Summary**
![Sales Summary](Images/Sales%20Summary.png)

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
