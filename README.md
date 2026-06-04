# 🚲 Adventure Works Sales Performance Dashboard
### End-to-End Power BI Project | ETL · Galaxy Schema · DAX · Storytelling with Data

## 📌 Problem Statement

Adventure Works is a fictional bicycle manufacturer with no visibility into which products and regions are actually profitable. Decisions were being made on gut feeling.

**Goal:** Build a self-service analytics dashboard that answers any business question in under 30 seconds.

**Stakeholder:** Sarah — Sales VP

---

## 📊 Dashboard Pages

| Page | Focus | Business Questions |
|------|-------|-------------------|
| 1 — Executive Summary | Revenue trends, KPIs, top products | Q1, Q5, Q6, Q9 |
| 2 — Product Analysis | Winners vs losers, margin vs revenue | Q2, Q6 |
| 3 — Regional Analysis | Geographic performance, return rates | Q4, Q12 |
| 4 — Customer Insights | Segmentation, demographics, top customers | Q7, Q11 |

---

## 🔍 Key Insights Discovered

- **💡 Accessories Hidden Gem** — 63% profit margin but less than 5% of revenue. Biggest untapped growth opportunity.
- **🚵 Mountain-200 Dominance** — Single model family contributes 28%+ of total product revenue.
- **⚠️ France Return Risk** — 3.55% return rate vs 3.23% average. Risk market before European expansion.
- **📉 Australia Volume Trap** — Highest revenue region but lowest profit margin. Volume without profitability.
- **👥 Core Customer Segment** — 55+ Medium Income Female customers = highest revenue, lowest return rate.
- **📅 Q2 Seasonal Peak** — Q2 consistently drives highest quarterly revenue across all years.

---

## 🏗️ Project Architecture

```
Raw CSV Files (7 files)
        ↓
Power Query (ETL)
→ Append: Sales 2015 + 2016 + 2017
→ Merge: Products + Subcategory + Category
→ Calculated columns: Revenue, Cost, Profit, Margin%
        ↓
Galaxy Schema (Data Model)
→ 2 Fact Tables: FactSales + FactReturns
→ 4 Dimensions: Products, Customers, Territories, Calendar
→ 7 Relationships (single direction)
        ↓
DAX Measures (20+)
→ Sales, Profitability, Time Intelligence, Ranking, Returns
        ↓
4-Page Dashboard
→ Storytelling with Data principles applied
```

---

## 🗂️ Data Model

**Schema Type:** Galaxy Schema (two fact tables sharing dimensions)

```
DimCalendar ──────────────────────────────────┐
DimProducts ────────┐                         │
DimCustomers ───────┼──► FactSales ◄──────────┤
DimTerritories ─────┘         │               │
                               └──► FactReturns
```

---

## 📐 DAX Measures



```dax
Total Revenue     = SUM(Sales[Revenue])
Total Cost        = SUM(Sales[Cost])
Total Profit      = SUM(Sales[Profit])
Total Orders      = COUNTROWS(Sales)
Total Quantity    = SUM(Sales[OrderQuantity])
Total Customers   = DISTINCTCOUNT(Sales[CustomerKey])
```


```dax
Profit Margin %      = DIVIDE([Total Profit], [Total Revenue], 0)
Avg Order Value      = DIVIDE([Total Revenue], [Total Orders], 0)
Revenue per Customer = DIVIDE([Total Revenue], [Total Customers], 0)
Return Rate %        = DIVIDE([Total Returns], [Total Orders], 0)
```


```dax
Revenue LY        = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR(Calendar[Date]))
Revenue YTD       = TOTALYTD([Total Revenue], Calendar[Date])
YoY Growth %      = DIVIDE([Total Revenue] - [Revenue LY], [Revenue LY], 0)
Revenue PrevMonth = CALCULATE([Total Revenue], PREVIOUSMONTH(Calendar[Date]))
MoM Growth %      = DIVIDE([Total Revenue] - [Revenue PrevMonth], [Revenue PrevMonth], 0)
```


```dax
Product Rank         = RANKX(ALL(Products[ProductName]), [Total Revenue],, DESC)
Customer Rank by Name = RANKX(ALL(customers[Name]), [Total Revenue],, DESC)

---

## 📈 Key Metrics

| Metric | Value |
|--------|-------|
| Total Revenue | $24.91M |
| Total Profit | ~$10M |
| Profit Margin | 41.97% |
| Avg Order Value | $444.54 |
| Total Customers | 17,000+ |
| Total Products | 293 |
| Data Range | Jan 2015 – Jun 2017 |

---

## 🛠️ Tech Stack

| Layer | Tool |
|-------|------|
| Visualization | Microsoft Power BI Desktop |
| ETL | Power Query (M Language) |
| Calculations | DAX |
| Data Modeling | Galaxy Schema |
| Design Reference | Storytelling with Data — Cole Nussbaumer Knaflic |
| Documentation | Word + Markdown |

---

## 🐛 Bugs Found & Fixed

| Bug | Root Cause | Fix |
|-----|-----------|-----|
| Revenue blank for 2016/2017 | ETL applied to Sales 2015 instead of combined Sales table | Redid all transforms on correct table |
| OrderDate null for 2015 rows | StockDate vs OrderDate column name mismatch in append | Renamed StockDate → OrderDate before appending |
| Values appearing blank | Revenue columns created as Whole Number type | Changed to Fixed Decimal Number |
| RANKX showing 1 for all rows | ALL() used CustomerKey but visual had Name column | Aligned ALL() to match visual field |

---

## ⚠️ Data Limitations

- **2017 H2 Missing** — Sales 2017 file contains January–June only. Trend lines after June 2017 reflect 2015+2016 data only.
- **No Discount Data** — Q3 (discount impact) could not be answered.
- **No Salesperson Data** — Q8 (salesperson performance) could not be answered.


[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/balu-analytics)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/balu1613)
