# 📊 Sales Analysis Performance Dashboard

> **Multi-page interactive Power BI dashboard** — Power BI · DAX · SQL Server · Power Query · Star Schema

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-F2C811?style=for-the-badge&logo=powerbi&logoColor=black)
![SQL Server](https://img.shields.io/badge/SQL%20Server-CC2927?style=for-the-badge&logo=microsoftsqlserver&logoColor=white)
![Power Query](https://img.shields.io/badge/Power%20Query-217346?style=for-the-badge&logo=microsoft&logoColor=white)

> 🔒 Interactive version available on request — connect via [LinkedIn](https://www.linkedin.com/in/olugbenga-adeoye-72713b253/)

---

## 📌 Project Overview

Analysed **$17.74M in global sales** across FY2009–2011 for a UK-based online retailer operating across 41 countries. Built an end-to-end Power BI solution on a star schema data model — covering ETL in Power Query, SQL Server data warehousing, and a 5-page interactive dashboard with drill-through, bookmarks, and dynamic DAX measures for executive decision-making.

**Key finding:** The UK drives ~$15M (85%) of total revenue, yet international markets like Singapore (5.27× YoY growth), Australia (4.96×), and Brazil (4.26×) represent the fastest-growing opportunity — pointing to an underweighted international strategy.

---

## 📊 Dashboard KPIs

| Metric | Value |
|---|---|
| Total Sales (incl. Unknown) | $17.74M |
| Total Sales (excl. Unknown) | $16.25M |
| **YoY Sales Growth** | **88.7%** |
| Total Quantity Sold | 10M units |
| Average Order Value (AOV) | $447.01 |
| Total Customers | 5,878 |
| Repeat Customers | 5,767 **(98.1%)** |
| Total Products | 4,202 |
| Product Repeatability | 96.1% |
| MoM Growth | 3.0% |

---

## 🗂️ Repository Structure

```
sales-performance-dashboard/
│
├── screenshots/
│   ├── dashboard_overview.png
│   ├── product_performance.png
│   ├── customer_insights.png
│   ├── dashboard_executive_summary.png
│   └── business_insights.png
│
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| Data Storage | SQL Server | Raw transactional data storage |
| ETL | Power Query (M) | Data transformation and shaping |
| Data Model | Star Schema | Dimensional modelling for performance |
| Visualisation | Power BI Desktop & Service | 5-page interactive dashboard |
| Calculations | DAX | KPIs, time intelligence, dynamic measures |

---

## ⚙️ Data Architecture

```
SQL Server (Raw Data)
        │
        ▼
┌─────────────────────┐
│   Power Query (ETL) │  → Clean, transform, shape
│   M Language        │  → Load into star schema model
└─────────────────────┘
        │
        ▼
┌─────────────────────────────────────────────┐
│   Star Schema Data Model                    │
│                                             │
│   FactSales ──── DimCustomer               │
│       │ ──────── DimProduct                │
│       └ ──────── DimDate                   │
└─────────────────────────────────────────────┘
        │
        ▼
┌─────────────────────┐
│   Power BI          │  → 5-page dashboard
│   DAX Measures      │  → Drill-through, bookmarks, RLS
└─────────────────────┘
```

---

## 📐 Data Model

Built on a **star schema** with four tables for optimal query performance and clean DAX:

| Table | Type | Description |
|---|---|---|
| `FactSales` | Fact | Transactional sales records — quantity, revenue, order IDs |
| `DimCustomer` | Dimension | Customer details and segmentation |
| `DimProduct` | Dimension | Product catalogue — 4,202 SKUs |
| `DimDate` | Dimension | Full date table powering all time intelligence |

---

## 📋 Dashboard Pages

### Page 1 — Sales Overview
*Global sales snapshot across FY2009–2011*

![Sales Overview](screenshots/dashboard_overview.png)

- Total sales trend by date (Jan 2010 – Dec 2011)
- Top 10 products and customers by total sales
- Sales breakdown by country — UK leading at ~$15M
- Product contribution analysis (Top 10 vs Others)
- Slicers: Country, Product, Year

---

### Page 2 — Product Performance
*Top products · Sales & Quantity · FY2009–2011*

![Product Performance](screenshots/product_performance.png)

- Top selling product: **Regency Cakestand 3 Tier** ($286.49K)
- Avg units per product: 2,570 · Avg sales per product: $4,220
- Repeatable products: 4,038 of 4,202 (96.1%)
- Top 10 product table with total sales and quantity
- Key insight: High-value products contribute disproportionately to revenue despite lower volume

---

### Page 3 — Customer Insights
*Customer behaviour, segmentation & growth*

![Customer Insights](screenshots/customer_insights.png)

- 98.1% repeat customer rate — strong loyalty indicator
- Top 20 customers ranked by total spend across 41 countries
- UK dominates with $15M in customer sales
- Customer growth trend over time (Jan 2010 – Dec 2011)
- Filterable by Country, Product, and Year

---

### Page 4 — Executive Summary
*High-level KPIs for leadership reporting*

![Executive Summary](screenshots/dashboard_executive_summary.png)

- Top 3 countries contribute $1.45M (8.2% of total)
- Top 10 products account for 14.7% of total revenue
- Regional Sales & YoY Growth map — geographic distribution of revenue hotspots
- Customer segmentation by spend: High ($9.0M) · Low ($6.0M) · Medium ($2.8M)
- Dynamic titles and bookmarks for executive presentations

---

### Page 5 — Business Insights
*Advanced analytics · Trends & Segmentation*

![Business Insights](screenshots/business_insights.png)

- Product sales concentration — Top 10 as % of total
- YoY growth by country: Singapore (5.27×), Australia (4.96×), Brazil (4.26×)
- Total sales vs 3-month moving average trend line
- Customer segmentation by spend per month
- High / Low / Medium value customer split over time
- MoM Growth: 3.0%

---

## 📏 Key DAX Measures

```dax
-- Year-over-Year Sales Growth
YoY Sales Growth % =
VAR _currentYear = [Total Sales]
VAR _priorYear   = CALCULATE([Total Sales], SAMEPERIODLASTYEAR(DimDate[Date]))
RETURN
    DIVIDE(_currentYear - _priorYear, _priorYear, 0) * 100

-- 3-Month Moving Average
3M Moving Avg Sales =
AVERAGEX(
    DATESINPERIOD(DimDate[Date], LASTDATE(DimDate[Date]), -3, MONTH),
    [Total Sales]
)

-- Repeat Customer Rate
Repeat Customer Rate % =
DIVIDE(
    COUNTROWS(FILTER(DimCustomer, [Purchase Count] > 1)),
    COUNTROWS(DimCustomer),
    0
) * 100

-- Average Order Value
AOV =
DIVIDE([Total Sales], [Total Orders], 0)
```

---

## 💡 Key Insights

1. **88.7% YoY sales growth** — from FY2009–2010 to FY2010–2011, demonstrating strong business momentum across the two-year period.

2. **98.1% repeat customer rate** — near-perfect retention signals an exceptionally loyal customer base and strong product-market fit.

3. **UK accounts for ~85% of revenue** ($15M of $17.74M) — while dominant, this represents a concentration risk if UK demand softens.

4. **Singapore, Australia, and Brazil are the fastest-growing markets** — growing 5×, 5×, and 4× respectively YoY, pointing to significant underexplored international opportunity.

5. **Top 10 products drive only 14.7% of revenue** — sales are well-distributed across 4,202 SKUs, meaning no single product poses a revenue concentration risk.

6. **High-value customers ($9M segment) outspend medium-value customers 3×** — targeted retention of this segment should be a primary commercial priority.

---

## 🚀 How to Use

1. Clone or download this repository
2. Open the `.pbix` file in **Power BI Desktop**
3. Connect to your SQL Server data source
4. Refresh the data model
5. Navigate pages using the tabs at the bottom

> 🔒 The `.pbix` file is available on request. Connect via [LinkedIn](https://www.linkedin.com/in/olugbenga-adeoye-72713b253/)

---

## 👤 Author

**Olugbenga Adeoye**
Data Analyst & Power BI Developer | Transitioning into Data Engineering · London, UK

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/olugbenga-adeoye-72713b253/)
[![GitHub](https://img.shields.io/badge/GitHub-OlugbengaHub-181717?logo=github)](https://github.com/OlugbengaHub)

> 💼 Open to Data Analyst, Power BI Developer, and Data Engineering opportunities — feel free to reach out!

---

*Built as part of a data engineering transition portfolio. See also:*
*→ [Customer Churn Analysis Dashboard](https://github.com/OlugbengaHub/customer-churn-analysis)*
*→ Healthcare Patient Analytics Dashboard (coming soon)*
