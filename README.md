# Myntra Sales Analytics Dashboard — Power BI

A 3-page interactive sales analytics dashboard built in Power BI, modeled on Myntra — India's leading fashion e-commerce platform. This project covers the full pipeline from synthetic data generation to a branded, interactive dashboard ready for business storytelling.

---

## Dashboard Preview

### Overview
![Myntra Overview](Myntra%20Overview.png)

### Customer Insights
![Customer Insights](Myntra%20Customer%20Insighs.png)

### Product Performance
![Product Performance](Myntra%20Product%20Performance.png)

---

## Project Structure

```
myntra-powerbi-dashboard/
├── MYNTRA.pbix                      # Power BI report file
├── Myntra_PowerBI_Dataset.xlsx      # Dataset (4 sheets: Customers, Products, Orders, Date)
├── Myntra_Themejson.json            # Custom Myntra brand theme file
├── Myntra Overview.png              # Dashboard screenshot — Page 1
├── Myntra Customer Insighs.png      # Dashboard screenshot — Page 2
└── Myntra Product Performance.png   # Dashboard screenshot — Page 3
```

---

## Data Model

This project uses a proper **star schema** with 4 tables linked via relationships:

| Table | Rows | Description |
|---|---|---|
| Customers | 1,000 | Customer demographics, city, state, segment |
| Products | 400 | Category, brand, MRP, discount %, selling price |
| Orders | 6,000 | Order date, quantity, payment method, status, rating |
| Date | 901 | Full date dimension (Jan 2024 — Jun 2026) |

**Relationships:**
- `Orders[CustomerID]` → `Customers[CustomerID]` (Many-to-One)
- `Orders[ProductID]` → `Products[ProductID]` (Many-to-One)
- `Orders[OrderDate]` → `Date[Date]` (Many-to-One)

The dataset was synthetically generated using Python (Faker library) with realistic Indian city/state distributions, Myntra-style brand names, and weighted order status probabilities.

---

## DAX Measures

All measures are stored in a dedicated `M-Table` following Power BI best practices:

```DAX
Total Revenue = SUMX(Orders, Orders[Quantity] * RELATED(Products[SellingPrice]))

Total Orders = COUNTROWS(Orders)

Total Customers = DISTINCTCOUNT(Orders[CustomerID])

Average Order Value = DIVIDE([Total Revenue], [Total Orders])

Return Rate % = DIVIDE(
    CALCULATE(COUNTROWS(Orders), Orders[OrderStatus] = "Returned"),
    [Total Orders]
)

Cancellation Rate % = DIVIDE(
    CALCULATE(COUNTROWS(Orders), Orders[OrderStatus] = "Cancelled"),
    [Total Orders]
)

Revenue MTD = TOTALMTD([Total Revenue], 'Date'[Date])

Premium Segment % = DIVIDE(
    CALCULATE(DISTINCTCOUNT(Customers[CustomerID]), Customers[CustomerSegment] = "Premium"),
    DISTINCTCOUNT(Customers[CustomerID])
)

Top Category = CALCULATE(
    SELECTEDVALUE(Products[Category]),
    TOPN(1, VALUES(Products[Category]), [Total Revenue], DESC)
)
```

---

## Dashboard Pages

### Page 1 — Sales Overview
- KPI cards: Total Revenue, Total Orders, Average Order Value, Return Rate %, Cancellation Rate %
- Revenue trend line chart (monthly)
- Revenue by Category donut chart
- Orders by City bar chart
- Orders by Payment Method bar chart
- Interactive slicers: Date range, Category, Order Status

### Page 2 — Customer Insights
- KPI cards: Total Customers, Average Age, Average Rating, Premium Segment %
- Customers by Segment bar chart
- Orders by Gender donut chart
- Top Customers by Revenue table
- Customers by State bar chart

### Page 3 — Product Performance
- KPI cards: Total Products, Avg Discount %, Top Category, Avg Rating
- Revenue by SubCategory bar chart
- Top Brands by Revenue bar chart
- Top Performing Products table

---

## Tools & Technologies

| Tool | Usage |
|---|---|
| Power BI Desktop | Dashboard design, DAX, data modeling |
| Python (Faker, Pandas) | Synthetic dataset generation |
| DAX | KPI measures, time intelligence, filter context |
| Power Query (M) | Data type validation, null handling |
| JSON | Custom Myntra brand theme |

---

## Key Learnings

- Built a proper star schema with 4 related tables and active relationships
- Handled null values meaningfully (Rating/DeliveryDays null only for non-delivered orders)
- Debugged real-world DAX issues including filter context conflicts and percentage formatting mismatches
- Designed a branded UI using Myntra's actual color palette (`#FF3F6C`, `#282C3F`, `#FF905A`)

---

## Author

**Shibila Sherin M**

Data Science | Data Analysis | Power BI | Python |Statistics | Machine Learning | NLP | Deep Learning | SQL

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?style=flat&logo=linkedin)](https://www.linkedin.com/in/shibila-sherin-m-99881b3a1)
[![GitHub](https://img.shields.io/badge/GitHub-Follow-black?style=flat&logo=github)](https://github.com/shibilasherinn/E-Commerce-Sales-Dashboard)
