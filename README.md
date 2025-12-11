![Status](https://img.shields.io/badge/Status-Complete-brightgreen)
![Power BI](https://img.shields.io/badge/Built%20with-Power%20BI-teal?logo=powerbi)
![Excel](https://img.shields.io/badge/Analysis-Excel-orange?logo=microsoft-excel)
![DAX](https://img.shields.io/badge/Measures-DAX-blueviolet)
![Dataset](https://img.shields.io/badge/Dataset-Classic%20Models-darkblue)

# Classic Models | Purchase & Sales Analysis

This project analyzes sales and purchase performance for the **Classic Models** company using **Power BI** and **Excel**.  
The report helps understand **which product lines, countries, and customers drive revenue and profit**, and how sales evolve over time.

The solution combines:

- A **Power BI dashboard** for interactive exploration  
- **Excel pivot tables & charts** for supporting purchase and customer behavior analysis  
- Clean **data model + DAX measures** for reusable business metrics  

---

## 📌 Business Objective

> Provide a unified view of Classic Models’ sales and purchases to support decisions on **profitable product lines, key markets, and high-value customers**.

Specifically, the report answers:

- Which **product lines** generate the highest sales and profit?
- How do **sales and net profit** trend over time (MoM, YTD)?
- Which **offices and customer countries** contribute most to revenue?
- What is the **average sales value per order**?
- How do **customer segments** behave by **credit limit, delays, and order mix** (via Excel analysis)?

---

## 🧩 Dataset

- Source: **Classic Models** sample sales data (orders, customers, products, offices)
- Main table: `Sales Data for Power BI`
- Key columns:  
  `ordernumber`, `orderdate`, `office_country`, `customer_country`,  
  `productLine`, `productName`, `QuantityOrdered`, `sales_value`, `cost_of_sales`, `buyPrice`, `customer_credit_limit`, etc.

---

## 🛠️ Tech Stack

- **Power BI Desktop**
  - Data Model, Relationships
  - Power Query for basic cleaning & transformation
  - **DAX measures** for KPIs and time intelligence
- **Microsoft Excel**
  - Pivot tables
  - Conditional formatting
  - Supporting charts for purchase / credit analysis
- **GitHub** for version control and documentation

---

## 📊 Power BI Report – Highlights

### Page 2 – Sales Overview (Decomposition Tree & Time Analysis)

- **Decomposition tree** showing how **Net Profit** breaks down by:
  - Customer Country → Product Line → Customer Name
- **Sales overview table** with:
  - Monthly Sales Value
  - **MoM% change**
  - **YTD Sales**

### Page 1 – Sales Dashboard

- **KPIs**
  - Total Sales
  - Net Profit
  - Count of unique orders
  - Average value of each order
- **Visuals**
  - Bar chart: **Sales by Product Line**
  - Scatter/line: **Sales by Cost of Sales** (relationship between revenue and cost)
  - Donut chart: **Sales by Office**
  - Column chart: **Sales by Customer Country**
  - Time-series: **Total Sales over time**
  - Time-series: **Count of unique orders**
  - Time-series: **Average value of each order**

- **Filters / Slicers**
  - Date range (from–to)
  - Office city
  - Customer city
  - Product line
  - Customer

These allow users to drill into specific regions, product lines, and customer segments.

---

## 📈 Excel Analysis – Purchase & Customer Behavior

In addition to Power BI, exploratory purchase and customer analysis was done in Excel:

- **Credit limit groups vs total sales**
  - Pivot table + bar chart for total sales by credit limit band  
  - KPI: **average sales per order** per credit limit group
- **Credit card holders vs non-holders**
  - Pie chart showing number of customers with/without credit cards
- **Delivery performance**
  - Pie chart showing **% of delayed vs on-time orders**
- **Purchase difference analysis**
  - Conditional formatting on `purchase_diff` to highlight customers with high positive/negative differences
- **Purchase mix heatmap**
  - Heatmap of product line combinations per order (e.g. Classic Cars vs Motorcycles, Planes, Ships etc.)
- **Top purchases by product**
  - Sorted bar chart of highest purchase amounts by product name
- **Sales by office vs customer country**
  - Bar chart visual comparing total sales by **office_country** and **customer_country**

These Excel views complement the Power BI report and demonstrate traditional BI + spreadsheet skills.

---

## 🔍 Key Insights and Findings
#### 1. Classic Cars Dominate Revenue

- Classic Cars is the strongest product line by a large margin, generating the highest total sales and profit.
- It consistently outperforms other lines such as Motorcycles, Trucks & Buses, and Vintage Cars.
- This indicates strong customer preference and should be the primary category for inventory and marketing focus.

#### 2. Product Line Profitability Varies Sharply

- While Classic Cars lead in sales, Motorcycles and Vintage Cars show stronger profit margins in several months.
- Some product lines generate high sales but low profit due to:

  - Higher cost of goods
  - Discount-heavy orders
  - Inefficient shipping or credit terms

#### 3. Country-Level Trends Highlight Core Markets

- USA, France, and Spain generate the largest share of revenue.
- Smaller markets like Australia and Singapore show better average profit per order, despite lower order volume.
- Indicates opportunities to:

  - Expand into high-margin smaller regions
  - Optimize logistics in large but low-margin countries

#### 4. Office-Level Sales Performance Shows Geographic Imbalance

- Offices in San Francisco and Paris outperform others significantly.
- Offices such as Boston and Madrid contribute less to overall revenue.
- Suggests uneven distribution of customer demand and potential opportunities for:

  - Reallocation of resources
  - Region-specific sales strategies

#### 5. Monthly Sales Trend Shows Seasonality

- Sales and net profit show consistent month-over-month fluctuations, with peaks in Q2 and Q4.
- MoM% analysis reveals:

  - Strong growth months driven by large B2B orders
  - Declines linked to slowed reorders and supply chain delays
  - Year-to-date (YTD) sales track strongly upward.

#### 6. Customers With Higher Credit Limits Spend Significantly More

- Customers with credit limits above $80,000–$100,000 generate:

  - Highest total sales
  - Higher average order values
  - Customers with smaller credit limits show:
  - Lower purchase frequency
  - Smaller basket size

- Indicates credit capacity directly affects sales potential.

## 🧮 Key DAX Measures

### 🔹 Average Sales Value per Order

```DAX
Average Sales Value per Order =
DIVIDE(
    SUM ( 'Sales Data for Power BI'[sales_value] ),
    DISTINCTCOUNT ( 'Sales Data for Power BI'[ordernumber] )
)
```

### 🔹 Net Profit

```DAX
Net Profit =
VAR SalesValue =
    SUM ( 'Sales Data for Power BI'[sales_value] )
VAR CostValue =
    SUM ( 'Sales Data for Power BI'[cost_of_sales] )
RETURN
    SalesValue - CostValue
```

### 🔹 Sales Value MoM%

```DAX
sales_value MoM% =
IF(
    ISFILTERED ( 'Sales Data for Power BI'[orderdate] ),
    ERROR (
        "Time intelligence quick measures can only be grouped or filtered
        by the Power BI-provided date hierarchy or primary date column."
    ),
    VAR __PREV_MONTH =
        CALCULATE(
            SUM ( 'Sales Data for Power BI'[sales_value] ),
            DATEADD ( 'Sales Data for Power BI'[orderdate].[Date], -1, MONTH )
        )
    RETURN
        DIVIDE(
            SUM ( 'Sales Data for Power BI'[sales_value] ) - __PREV_MONTH,
            __PREV_MONTH
        )
)
```

### 🔹 Sales Value YTD

```DAX
sales_value YTD =
IF(
    ISFILTERED ( 'Sales Data for Power BI'[orderdate] ),
    ERROR (
        "Time intelligence quick measures can only be grouped or filtered
        by the Power BI-provided date hierarchy or primary date column."
    ),
    TOTALYTD(
        SUM ( 'Sales Data for Power BI'[sales_value] ),
        'Sales Data for Power BI'[orderdate].[Date]
    )
)
```

### 🔹 Selected Metric (Dynamic Toggle Between Sales & Net Profit)

```DAX
Selected Metric =
VAR SalesValue =
    SUM ( 'Sales Data for Power BI'[sales_value] )
VAR ProfitValue =
    SalesValue
        - SUM ( 'Sales Data for Power BI'[cost_of_sales] )
RETURN
    SWITCH(
        SELECTEDVALUE ( 'measures table'[Number ID] ),
        1, SalesValue,
        2, ProfitValue,
        SalesValue
    )
```

## 🔗 Data Model

![ModelView](screenshots/model-view.png)

## 📊 Power BI Dashboard Screenshots

### Page 1 – Sales Dashboard
![Page1](screenshots/classic-models-page_1.png)

### 📄 Page 2 – Profit Decomposition
![Page 1](screenshots/classic-models-page_2.png)

## 📈 Excel Purchase Analysis

### Excel Dashboard for Purchase Analysis
![ExcelDashboard](excel-analysis-screenshots/excel-dashboard.png)

### Delayed Orders
![Delays](excel-analysis-screenshots/delayed.png)

### Sales Overview
![Sales](excel-analysis-screenshots/sales-overview.png)

### Sales per Country
![Country](excel-analysis-screenshots/sales-per-country.png)

### Purchase Orders Heatmap
![Heatmap](excel-analysis-screenshots/purchase-orders-heatmap.png)

## 📁 Project Structure
```
Classic-Models-Purchase-Analysis/
│
├── PowerBI/ → Dashboard PBIX (only snapshots)
├── excel-analysis-screenshots
├── excel
├── screenshots
└── README.md
